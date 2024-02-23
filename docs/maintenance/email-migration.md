---
tags:
  - Server Pro
---

<style>
table th:first-of-type {
    width: 25%;
    word-break: break-all;
}
table th:nth-of-type(2) {
    width: 75%;
}
</style>

From a login perspective, the primary identifier for a user is their email address. If you are migrating from locally based authentication to SSO, you your are migrating from one IdP to another, you may be required to updated users email addresses.

To help with this, a script is provided in {{ versions['server-pro-short'] }} that will migrate user emails using a CSV file with the following format:

oldEmail,newEmail

The script will iterate through the CSV file and update the users's email addresses from **oldEmail** to **newEmail**, after performing some validation checks.

#### Validation ####

Before running the migration the `<csv_file>` will be checked to ensure that:

- For each row, both the **oldEmail** and the **newEmail** are valid email addresses
- There are no duplicate entries, for example, you are not attempting to update different users to the same new email address or you are attempting to update the same user account with an already-used email address

#### Usage ####

```
docker cp <csv_file> sharelatex:/overleaf/services/web/<csv_file>
docker exec sharelatex /bin/bash -c "cd /overleaf/services/web;node ./modules/server-ce-scripts/scripts/migrate-user-emails.js [--commit] [--continue|--ignore-missing] [--admin-id=ADMIN_USER_ID] <csv_file>"
```

#### Flags ####

| Name | Description |
|------|-------------|
| `--commit` | The inclusion of the `--commit` flag will actually do the migration. When omitted, the migration will happen in a dry-run mode where no changes are made but validation is still performed. <br /><br /> **Default**:`false` |
| `--continue` | The `--continue` flag will continue the migration process if a user account if the email address on the account has already been updated |
| `--ignore-missing` | The `--ignore-missing` flag will continue the migration process if a user account was not found |
| `--admin-id` | The `--admin-id` should be set to the ID of the Administrator who is performing the migration and will be used for audit log entries. Go to **Admin** > **Manage Users**  and search for your email address and click on the first result to find your user-id. |
| `<csv_file>` | The `<csv_file>` is the file with the old and new email addresses in |

#### Completion ####

While executing the migration script, depending on the flags that you have chosen will determine whether an error is logged to the console and then continues, or logged to the console and exits. If the script terminates prematurely due to an error, the script will exit with code 1 to denote that it was unsuccessful.

On completion of the migration, the script will log the number of successful, failed and skipped updates to the console. If the migration is completed successfully it will exit with code 0.