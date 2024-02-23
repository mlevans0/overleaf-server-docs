## Searching for users ##

## Provisioning users ##

Once you are logged in as an admin user, you can visit /admin/register on your Overleaf instance and create new users. If you have an email backend configured in your settings file, new users will be sent an email with a URL to set their password. If not, you will have to distribute the password reset URLs manually. These are shown when you create a user.

### Creating administrators ###

## Deleting users ##

## Updating user details ##

## Generating password reset links ##

## Viewing user audit logs ##

## Viewing user session information ##

## Understanding license usage ##

{{ versions['server-pro-full'] }} licensing is quite different from other cloud-based solution in that there is no concept of free collaboration, any user who logs in and accesses a project is considered an active user by the system, necessitating a corresponding license. 

!!! tip

    Administrators can view the total number of active users on your instance via **Admin** > **Manage Users** > **License Usage**. An active user is defined as someone who has opened a project on your {{ versions['server-pro-short'] }} instance in the last 12 months.

The active/inactive flag for an account is just a high-level indicator for the administrator to help indicate usage. If a user who holds an {{ versions['server-pro-short'] }} license leaves your organization, there is no immediate necessity to delete their account and their associated projects. This action is often required for regulatory and compliance purposes, but from a licensing point of view, the license allocated to the departing user can be reassigned to their successor, allowing for a seamless transition.

In general, it's advisable not to delete accounts (unless they are no longer required), as this action would lead to the removal of the account's projects and prevent any collaborators from accessing them. 

Presently, there is no provision for deactivating accounts; they either exist with retained data or don't exist at all. The recommended approach is to leave the account and its associated data within {{ versions['server-pro-short'] }}, ensuring any projects are part of your backup process.

Over time, unused accounts will transition to an inactive state (as they will not have accessed a project within the past 12 months), denoting that they no longer require a license, even though the accounts and associated projects are retained. In the event a user returns, they can access their account and projects, reinstating their status as an active user and once again requiring a licence.

It's important to highlight that in {{ versions['server-pro-short'] }}, Administrators retain the capability to access the projects of any user, even after that user has left the organization. This maintains a certain degree of access and authority over projects. Additionally, Administrators hold the ability to transfer ownership of projects from one user to another, offering full control over your data.

When it comes to renewals, as {{ versions['server-pro-short'] }} doesn't transmit usage information back to us, we rely on customers to report their user count for license compliance. {{ versions['server-pro-short'] }} doesn't enforce a strict user limit, and there's no explicit 'inactive' setting for users. This means that if you're aware of inactive users, you have the option to exclude them when reporting your seat count to us. 

One way of doing this would be to report the number of users within your authorized group, rather than the total number of users in the {{ versions['server-pro-short'] }} instance, as the latter might encompass older, inactive accounts.






