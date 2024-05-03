If you never ran Server Pro version 5.0.1 or Community Edition version 5.0.1, or you started a brand new instance with 5.0.1, you do not need to run this recovery process.

!!! note "Update 1"

      (2024-04-22 13:40 BST): Added step "Stop new updates from getting into the system and flush all changes to MongoDB".

!!! note "Update 2"

      (2024-04-23 11:45 BST): Account for broken flushes in 5.0.1 and skip flushes when 5.0.2 was started.

The duration of the recovery will depend on the number and size of projects in your instance and the storage backend used by the history store for chunks (as defined in `OVERLEAF_HISTORY_CHUNKS_BUCKET`).

The recovery process will delay the application start inside the Server Pro container. The site will appear offline during that time. We only support running the recovery from a single instance of the Server Pro container, all other horizontal scaling workers need to be offline.

You can stop and resume the recovery process if needed.

Based on our performance tests, the recovery process can process approximately 10k small projects per minute on modern hardware (3GHz CPU clock speed and local NVMe storage). As an example, for an instance with 100k projects, schedule a maintenance window that allows at least 10+2min of downtime.
Use the following query to estimate the number of projects in your instance:
```
$ docker exec mongo mongosh sharelatex --quiet --eval 'db.projects.estimatedDocumentCount() + db.deletedProjects.estimatedDocumentCount()'
```

Please read the following recovery steps in full before you start. Server Pro customers are more than welcome to reach out to support@overleaf.com with any questions.

Steps of the recovery process:
   1. Pull the `5.0.3` release images.
   2. Identify a few projects by id that are missing history; ideally you have permission to make a change to one of them.
   3. Schedule a maintenance window for the downtime.
   4. Stop all but one worker when using a horizontal scaling setup.
   5. Stop new updates from getting into the system and flush all changes to MongoDB:
       1. Close the editor and disconnect all users manually via the admin panel on `https://my-server-pro.example.com/admin#open-close-editor` in the "Open/Close Editor" tab.
       2. Stop the Websocket/real-time service.
          ```
          $ docker exec sharelatex sv stop real-time-overleaf
          ```
       3. Wait for the real-time service to exit, as indicated by `down:`.
          ```
          $ docker exec sharelatex sv status real-time-overleaf
          run: real-time-sharelatex: (pid 394) 50s, want down, got TERM
          # wait a little longer...

          $ docker exec sharelatex sv status real-time-overleaf
          down: real-time-sharelatex: 7s, normally up
          ```
       4. Stop the git-bridge container if enabled.
          ```
          $ docker stop git-bridge
          ```
       5. If you never ran 5.0.2: Issue a manual flush for document updates and wait for it to finish with success.

          You can repeat the command on error. In case you see a non-zero `failureCount` in successive runs, please stop the migration (restore the services via `docker restart git-bridge sharelatex`) and reach out to support.
          ```
          $ docker exec sharelatex bash -c 'source /etc/container_environment.sh && source /etc/overleaf/env.sh && cd services/document-updater && LOG_LEVEL=info node scripts/flush_all.js'
          ...
          {"name":"default","hostname":"...","pid":324,"level":30,"successCount":...,"failureCount":0,"msg":"finished flushing all projects","time":"...","v":0}
          Done flushing all projects
          ```
       6. If you never ran 5.0.2: Ensure that all changes have been flushed out of redis.

          If you get any output from `redis-cli`, please stop the migration (restore the services via `docker restart git-bridge sharelatex`) and reach out to support.
          ```
          $ docker exec redis redis-cli --scan --pattern 'DocVersion:*'
          # no output from redis-cli indicates success, check exit code of redis-cli next, it should be zero
          $ echo $?
          0
          ```
       7. Try to flush any pending history changes.

          This will need to be a best effort flush as some projects have broken histories due to the bad database migration. Any failures will be addressed with a re-sync of the history at the end of the recovery process.
          ```
          $ docker exec sharelatex bash -c 'source /etc/container_environment.sh && source /etc/overleaf/env.sh && cd /overleaf/services/project-history && LOG_LEVEL=info node scripts/flush_all.js' 
          ```
   7. Consider taking a consistent backup of the instance.
   8. Upgrade to version `5.0.3`.
   9. The recovery process runs on container start automatically.
   10. You can follow the progress of the script by tailing the log file `/var/lib/overleaf/data/history/doc-version-recovery.log`. It will print the total number of projects at the start and a summary after every 1000 projects processed.
       ```
       $ docker exec sharelatex tail --retry --follow /var/lib/overleaf/data/history/doc-version-recovery.log
       ```
   11. Wait for the recovery process to finish by either tailing the above log file until a `Done.` line has been printed or waiting for `Finished recovery of doc versions.` to be printed to the standard output of the Server Pro container.
   12. Validate the recovery process by opening the history pane for a few of the projects with previously missing history.
       1. Expedite the resync for the projects to test (They will get processed eventually, but we do not want to wait for them to get their turn.)
          ```
          $ docker exec sharelatex curl -X POST --silent "http://127.0.0.1:3054/project/000000000000000000000000/resync?force=true"
          ```
          (Repeat with each of the project-ids to test, replace `000000000000000000000000` with one project-id at a time.)
       2. Open the project editor for the projects `https://my-server-pro.example.com/project/000000000000000000000000`
       3. Open the "History" pane for the project and see the latest content.
       4. Optional: Close the "History" pane again. Make a code change, such as adding a comment to the header.
       5. Optional: Issue a re-compile to trigger a flush of the local change. Open the "History" pane again and see the change. When done, undo the change.

   13. Horizontal scaling: Start the other workers again.
   14. Please keep the instance running that executed the recovery process. It will resync the history for all projects in the background with a concurrency of 1. This will result in slightly elevated base load. (You can restart the instance, but it will need to start over with the resyncs.)

   15. Server Pro customers: Please let the support team know when you have completed the recovery process.
