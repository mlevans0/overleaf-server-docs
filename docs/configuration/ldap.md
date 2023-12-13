---
tags:
  - Server Pro
---

{{ versions['server-pro-short'] }} provides LDAP server integration for user authentication and is compatible with Active Directory systems. For {{ versions['toolkit-short'] }} deployments, the LDAP integration is configured via the [`variables.env`](/configuration/overleaf-toolkit#the-variablesenv-file) file

## Configuration

Internally, the Overleaf LDAP integration uses the [passport-ldapauth](https://github.com/vesse/passport-ldapauth) library. Most of these configuration options are passed through to the `server` config object which is used to configure `passport-ldapauth`. If you are having issues configuring LDAP, it is worth reading the README for `passport-ldapauth` to get a feel for the configuration it expects.

To enable LDAP authentication the `EXTERNAL_AUTH` variable **must** be set to `ldap`:

```
EXTERNAL_AUTH=ldap
```

You can see a full list of available configuration options for [LDAP/AD](environment-variables.md#ldapad) over on the [Environments variables](environment-variables.md) page. 

After bootstrapping {{ versions['server-pro-short'] }} for the first time with LDAP authentication, an existing LDAP user must be given admin permissions by visiting the `/launchpad` page (or [via CLI](https://github.com/overleaf/overleaf/wiki/Creating-and-managing-users#creating-the-first-admin-user), but in this case ignoring password confirmation). 

LDAP users will appear in Overleaf Admin Panel once they log in first time with their initial credentials.

!!! note

    To preserve backward compatibility with older configuration files, if `EXTERNAL_AUTH` is not set, but `SHARELATEX_LDAP_URL` is set, then the LDAP
    module will be activated. We still recommend setting `EXTERNAL_AUTH` explicitly.

## Example configuration ##

At Overleaf, we test the LDAP integration against a [test openldap server](https://github.com/rroemhild/docker-test-openldap). The following is an example of a working configuration:

```
# added to variables.env

EXTERNAL_AUTH=ldap
SHARELATEX_LDAP_URL=ldap://ldap:389
SHARELATEX_LDAP_SEARCH_BASE=ou=people,dc=planetexpress,dc=com
SHARELATEX_LDAP_SEARCH_FILTER=(uid={{username}})
SHARELATEX_LDAP_BIND_DN=cn=admin,dc=planetexpress,dc=com
SHARELATEX_LDAP_BIND_CREDENTIALS=GoodNewsEveryone
SHARELATEX_LDAP_EMAIL_ATT=mail
SHARELATEX_LDAP_NAME_ATT=cn
SHARELATEX_LDAP_LAST_NAME_ATT=sn
SHARELATEX_LDAP_UPDATE_USER_DETAILS_ON_LOGIN=true
```

The `openldap` needs to run in the same network as the `sharelatex` container (which by default would be `overleaf_default`), so we'll proceed with the following steps:

- Run `docker network create overleaf_default` (will possibly fail due to a `network with name overleaf_default already exists` error, that's ok).
- Start `openldap` container with `docker run --network=overleaf_default --name=ldap rroemhild/test-openldap:1.1`
- Edit `variables.env` to add the LDAP Environment Variables as listed above.
- Restart {{ versions['server-pro-short'] }} 
You should be able to login using `fry` as both username and password.

## Debugging ##

As LDAP is heavily configurable and flexible by nature it can be a good starting point to have a working example with `ldapsearch` or even used by another application.

The following command will connect to the LDAP server at `ldap` on port `389`. It will then bind to the server using the distinguished name (DN) `admin@planetexpress.com` and password `password123`. The base DN for the search will be `ou=people,dc=planetexpress,dc=com`. The search filter is set to return entries where the Common Name (CN) contains `fry`, and it will return the `mail` attribute of these entries. 

When running this search command against your own service please ensure that you replace `fry`, `password123`, and `planetexpress.com` with your actual username, password, and domain. Also, make sure that the LDAP server is accessible and the provided details are correct.

```
ldapsearch -H ldap://ldap:389 -x -D admin@planetexpress.com -w password123  -b ou=people,dc=planetexpress,dc=com "CN=\*fry\*" mail
```