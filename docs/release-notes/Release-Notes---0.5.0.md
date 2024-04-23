## 0.5.0 ##

- Code Check mode, shows tex errors in the editor
- Improved support for plain-text emails
- Show the current user email in the "Account" dropdown
- Improved project search
- Better cookie expiry behaviour
- Improved spell-check underline effect
- Old user accounts are automatically upgraded to modern feature set


## Server Pro 0.5.0 ##

- New SAML integration option, link ShareLaTeX to SAML/Shibboleth/etc.
- Improved LDAP integration, no configuration changes required
- Option to have user details updated on every login (LDAP and SAML) [SHARELATEX_LDAP_UPDATE_USER_DETAILS_ON_LOGIN](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-LDAP-Config#config)


## Server Pro 0.5.2 ##

- Correct an issue with LDAP configuration, requiring upgrade to a new configuration format.
- Add an option to restrict project invites to existing user accounts


## Server Pro 0.5.3 ##

- Fix a bug where the `/register` link in nav-bar was not being hidden when using an external auth source.


## Server Pro 0.5.4 ##

- Fix an issue with [SHARELATEX_HEADER_NAV_LINKS](https://github.com/sharelatex/sharelatex/wiki/Configuring-Headers,-Footers-&-Logo) option


## Server Pro 0.5.8 ##

- New Launchpad page to help with first setup: (`/launchpad`)
- Fixed an issue with project invites and Saml
- Various bugfixes

## Server Pro 0.5.9 ##

- Fixed an issue where logins were not being counted for LDAP users
- Internal improvements to page rendering

## Server Pro 0.5.11 ##

- Sandboxed Compiles with [Sibling Containers](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-sandboxed-compiles#sibling-containers)
- Various bugfixes