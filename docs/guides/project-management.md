## Recovering deleted docs ##

1. Log in using your administrator credentials
2. Click **Admin** and select **Manage Users**
3. Using the search field, enter the email of the user you want to restore the doc for
4. Click their email address in the list of returned results to open their user info page
5. Click the **Projects** tab and use the search field to search for the project
6. Click on the information icon next to the project name to open the **Project Info**
7. Click on the **Deleted Docs** tab
8. Click on the **Undelete** button next to the deleted doc to restore it back to the users project. 

The restored file will take the following format: `FILENAME-TIMESTAMP.EXTENSION`. For example, a restored version of `main.tex` will be restored to the root of the project with the file name `main-2024-02-23-130441542.tex`.

## Transferring ownership of a project ##

The admin panel in {{ versions['server-pro-short'] }} has a dedicated page per project. You can either get to it by searching for the user on "/admin/user", then going to their **Projects** tab, finding the project in the list and clicking on the :material-information: icon; or by directly navigating to the page with a known project-id "/admin/project/<the project id>". On the project info page, you can find a **Transfer Ownership** button, which opens a modal where you can specify any user as the new owner. The new owner does not need to be an existing collaborator on the project.

!!! info

    Note that the previous owner will be added as a collaborator with read-and-write access to the project as part of the ownership transfer process.
