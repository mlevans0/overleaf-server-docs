<style>
table th:first-of-type {
    width: 40%;
    word-break: break-all;
}
table th:nth-of-type(2) {
    width: 60%;
}
</style>

## All versions ##

These environment variables are compatible with {{ versions['community-edition-short'] }} and {{ versions['server-pro-short'] }} providing you with an easy migration path between these two on-premise versions. They can also be used with both {{ versions['toolkit-short'] }} and Docker Compose deployments.

| Name | Description |
|------|-------------|
| `SHARELATEX_SITE_URL` | Where your instance of Overleaf is publicly available. This is used in public links, and when connecting over websockets, so must be configured correctly! |
| `SHARELATEX_ADMIN_EMAIL` | The email address where users can reach the person who runs the site. |
| `SHARELATEX_APP_NAME` | The name to display when talking about the running application. Defaults to 'Overleaf (Community Edition)'. |
| `SHARELATEX_MONGO_URL` | The URL of the Mongo database to use |
| `SHARELATEX_REDIS_HOST` and `REDIS_HOST` | The host name of the Redis instance to use. Both are required (see [release notes](https://github.com/ overleaf/overleaf/wiki/Release-Notes-2.0#changes-to-the-docker-compose-file-format)) |
| `SHARELATEX_REDIS_PORT` and `REDIS_PORT` | The port of the Redis instance to use. Both are required (see [release notes](https://github.com/overleaf/overleaf/wiki/Release-Notes-2.0#changes-to-the-docker-compose-file-format)) |
| `SHARELATEX_REDIS_PASS` | The password to use when connecting to Redis (if applicable) |
| `SHARELATEX_NAV_TITLE` | Set the tab title of the application |
| `SHARELATEX_SESSION_SECRET` | A random string which is used to secure tokens, if load balancing this needs to be set to the same toke across boxes. If only 1 instance is being run it does not need to be set by the user. |
| `SHARELATEX_BEHIND_PROXY` | Set to true if running behind a proxy like nginx/apache allowing it to correctly detect the forwarded IP address |
| `SHARELATEX_SECURE_COOKIE` | Set this to something non-zero to use a secure cookie. Only use this if your Overleaf instance is running behind a reverse proxy with SSL configured. |
| `SHARELATEX_RESTRICT_INVITES_TO_EXISTING_ACCOUNTS` | If set to `true`, will restrict project invites to email addresses which correspond with existing user accounts. |
| `SHARELATEX_ALLOW_PUBLIC_ACCESS` | If set to `true`, will allow non-authenticated users to view the site. The default is `false`, which means  non-authenticated users will be unconditionally redirected to the login page when they try to view any part of the site. Note, setting this option does not disable authentication or security in any way. This option is necessary if your users intend to make their projects public and have non-authenticated users view those projects. |
| `SHARELATEX_ALLOW_ANONYMOUS_READ_AND_WRITE_SHARING` | If set to `true`, will allow anonymous users to view and edit projects shared via the new  |[link-sharing](https |//www.sharelatex.com/blog/2017/11/27/integration-update-link-sharing.html) feature. |
| `EMAIL_CONFIRMATION_DISABLED` | When set to `true` the banner requesting email confirmation won't be displayed. |
| `ADDITIONAL_TEXT_EXTENSIONS` | an array of strings to configure additional extensions for editable files |`ADDITIONAL_TEXT_EXTENSIONS='["abc", "xyz"]'` |
| `SHARELATEX_STATUS_PAGE_URL` | Custom status page URL (since 3.4.0), e.g. `status.example.com` |
| `SHARELATEX_FPH_INITIALIZE_NEW_PROJECTS` | set to `'false'` to prevent new projects from being initialised with Full Project History (since 3.5.0) |
| `SHARELATEX_FPH_DISPLAY_NEW_PROJECTS` | set to `'false'` to prevent new projects from displaying Full Project History instead of the legacy history (since 3.5.0) |
| `ENABLE_CRON_RESOURCE_DELETION` | Set this environment variable to `true` to enable the automatic clean-up of deleted projects and users after 90 days. |

## Password restrictions ##

It is possible to enforce password restrictions on users when using the Overleaf login system (local accounts), not an SSO option such as LDAP. For SSO accounts, password policies will be enforced by your identity provider or directory service, additionally allowing support for multi-factor authentication. 

| Name | Description |
|------|-------------|
| `SHARELATEX_PASSWORD_VALIDATION_MIN_LENGTH` | The minimum length required |
| `SHARELATEX_PASSWORD_VALIDATION_MAX_LENGTH` | The Maximum length allowed |
| `SHARELATEX_PASSWORD_VALIDATION_PATTERN` | is used to validate password strength<br /><br /><strong>Example</strong><br />- `abc123` – password requires 3 letters and 3 numbers and be at least 6 characters long<br />- `aA` – password requires lower and uppercase letters and be 2 characters long<br />- `ab$3` – it must contain letters, digits and symbols and be 4 characters long<br /> <strong>Note:</strong> There are 4 groups of characters: letters, UPPERcase letters, digits, symbols. Everything that is neigher letter, nor digit is considered to be a symbol.

## {{ versions['server-pro-short'] }} only ##

The environment variables listed below are only compatible with {{ versions['server-pro-short'] }} and can be used with both {{ versions['toolkit-short'] }} and Docker Compose deployments.

### LDAP/AD ###

| Name | Description |
|------|-------------|
| `SHARELATEX_LDAP_URL` | **Required**, The URL of the LDAP server, E.g. `ldaps://ldap.example.com:636` |
| `SHARELATEX_LDAP_EMAIL_ATT` | The email attribute the LDAP server will return, defaults to `mail` |
| `SHARELATEX_LDAP_NAME_ATT` | The property name holding the name of the user which is used in the application |
| `SHARELATEX_LDAP_LAST_NAME_ATT` | If your LDAP server has a first and last name then this can be used in conjunction with `SHARELATEX_LDAP_NAME_ATT` |
| `SHARELATEX_LDAP_PLACEHOLDER` | The placeholder for the login form, defaults to **Username** |
| `SHARELATEX_LDAP_UPDATE_USER_DETAILS_ON_LOGIN` | If set to `true`, will update the user **first_name** and **last_name** field on each login, and turn off the user-details form on /user/settings page. Otherwise, details will be fetched only on first login. |
| `SHARELATEX_LDAP_BIND_DN` | **Optional**, e.g. `uid=admin,ou=people,o=planetexpress.com`. |
| `SHARELATEX_LDAP_BIND_CREDENTIALS` | Password for `bindDn`. |
| `SHARELATEX_LDAP_BIND_PROPERTY` | **Optional**, default `dn`. Property of user to bind against client e.g. **name**, **email** |
| `SHARELATEX_LDAP_SEARCH_BASE` | The base DN from which to search for users by username. E.g. `ou=people,o=planetexpress.com` |
| `SHARELATEX_LDAP_SEARCH_FILTER` | LDAP search filter with which to find a user by username, e.g. `(uid={{username}})`. Use the literal `{{username}}` to have the given username be interpolated in for the LDAP search. If you are using Active Directory then the search filter `(sAMAccountName={{username}})` may be more appropriate. |
| `SHARELATEX_LDAP_SEARCH_SCOPE` | **Optional**, default `sub`. Scope of the search, one of `base`, `one`, or `sub`. |
| `SHARELATEX_LDAP_SEARCH_ATTRIBUTES` | **Optional**, default all. Json array of attributes to fetch from LDAP server. |
| `SHARELATEX_LDAP_GROUP_DN_PROPERTY` | **Optional**, default `dn`. The property of user object to use in `{{dn}}` interpolation of `groupSearchFilter`. |
| `SHARELATEX_LDAP_GROUP_SEARCH_BASE` | **Optional**. The base DN from which to search for groups. If defined, also `groupSearchFilter` must be defined for the search to work. |
| `SHARELATEX_LDAP_GROUP_SEARCH_SCOPE` |  **Optional**, default `sub`. |
| `SHARELATEX_LDAP_GROUP_SEARCH_FILTER` | **Optional**. LDAP search filter for groups. The following literals are interpolated from the found user object: `{{dn}}` the property configured with `groupDnProperty`. Optionally you can also assign a function instead, which passes a user object, from this a dynamic `groupSearchFilter` can be retrieved. |
| `SHARELATEX_LDAP_GROUP_SEARCH_ATTRIBUTES` | **Optional**, default all. Json array of attributes to fetch from LDAP server. |
| `SHARELATEX_LDAP_CACHE` | **Optional**, default `false`. If `true`, then up to 100 credentials at a time will be cached for 5 minutes. |
| `SHARELATEX_LDAP_TIMEOUT` | **Optional**, default Infinity. How long the client should let operations live for before timing out. |
| `SHARELATEX_LDAP_CONNECT_TIMEOUT` | **Optional**, default is up to the OS. How long the client should wait before timing out on TCP connections. |
| `SHARELATEX_LDAP_TLS_OPTS_CA_PATH` | A JSON array of paths to the CA file for TLS, must be accessible to the Docker container. E.g. `-env SHARELATEX_LDAP_TLS_OPTS_CA_PATH='["/var/one.pem", "/var/two.pem"]'` |
| `SHARELATEX_LDAP_TLS_OPTS_REJECT_UNAUTH` | If `true`, the server certificate is verified against the list of supplied CAs.|

### SAML 2.0 ###

| Name | Description |
|------|-------------|
| `SHARELATEX_SAML_IDENTITY_SERVICE_NAME` | Display name for the Identity Provider, used on the login page. |
| `SHARELATEX_SAML_EMAIL_FIELD` | Name of the **Email** field in user profile, defaults to `nameID`. **Alias**: `SHARELATEX_SAML_EMAIL_FIELD_NAME` |
| `SHARELATEX_SAML_FIRST_NAME_FIELD` | Name of the **firstName** field in user profile, defaults to `givenName` |
| `SHARELATEX_SAML_LAST_NAME_FIELD` | Name of the **lastName** field in user profile, defaults to `lastName` |
| `SHARELATEX_SAML_UPDATE_USER_DETAILS_ON_LOGIN` | If set to `true`, will update the users **firstName** and **lastName** fields on each login, and turn off the user-details form on `/user/settings` page. |
| `SHARELATEX_SAML_ENTRYPOINT` | Entrypoint URL for the SAML Identity Service <br />**Example**: `https://idp.example.com/simplesaml/saml2/idp/SSOService.php`<br />**Azure**: `https://login.microsoftonline.com/8b26b46a-6dd3-45c7-a104-f883f4db1f6b/saml2 ` |
| `SHARELATEX_SAML_CALLBACK_URL` | Callback URL for Overleaf service. Should be the full URL of the `/saml/callback` path. <br />**Example**: `https://sharelatex.example.com/saml/callback` |
| `SHARELATEX_SAML_ISSUER` | The issuer name |
| `SHARELATEX_SAML_CERT` | (required since `2.7.0`) Identity Provider's public signing certificate, used to validate incoming SAML messages, in single-line format. <br /> **Example**: `MIICizCCAfQCCQCY8tKaMc0BMjANBgkqh...W==` <br /><br /> See [more information about passing keys and certificates](#passing-keys-and-certificates). <br />- See [full documentation](https://github.com/node-saml/passport-saml/blob/master/README.md#security-and-signatures) for more information. |
| `SHARELATEX_SAML_PRIVATE_CERT` | **Optional**, path to a file containing a PEM-formatted private key used to sign auth requests sent by passport-saml.<br />**Note**: This would be better called `PRIVATE_KEY_FILE`, but `PRIVATE_CERT` is the current name.<br /><br />See [more information about passing keys and certificates](#passing-keys-and-certificates)<br />- See [full documentation](https://github.com/node-saml/passport-saml/blob/master/README.md#security-and-signatures) for more information. |
| `SHARELATEX_SAML_DECRYPTION_CERT` | **Optional**, public certificate matching the `SHARELATEX_SAML_DECRYPTION_PVK`, used for the [metadata endpoint](#metadata-for-the-identity-provider).<br /><br />- See [more information about passing keys and certificates](#passing-keys-and-certificates) for how to pass the certificate.<br />- See [full documentation](https://github.com/node-saml/passport-saml/blob/master/README.md#security-and-signatures) for more information. |
| `SHARELATEX_SAML_SIGNING_CERT ` | **Optional**, public certificate matching `SHARELATEX_SAML_PRIVATE_CERT`. It's required when setting up the [metadata endpoint](#metadata-for-the-identity-provider) if the strategy is configured with a `SHARELATEX_SAML_PRIVATE_CERT`.<br /><br />- An array of certificates can be provided to support certificate rotation. When supplying an array of certificates, the first entry in the array should match the current `SHARELATEX_SAML_PRIVATE_CERT`.<br />- See [more information about passing keys and certificates](#passing-keys-and-certificates) for how to pass the certificate.<br />- See [full documentation](https://github.com/node-saml/passport-saml/blob/master/README.md#security-and-signatures) for more information. |
| `SHARELATEX_SAML_DECRYPTION_PVK` | **Optional**, private key that will be used to attempt to decrypt any encrypted assertions that are received, in PEM (multi-line) format.<br /><br />- See [more information about passing keys and certificates](#passing-keys-and-certificates) for how to pass the key in PEM format.<br />- See [full documentation](https://github.com/node-saml/passport-saml/blob/master/README.md#security-and-signatures) for more information.
| `SHARELATEX_SAML_SIGNATURE_ALGORITHM` | Optionally set the signature algorithm for signing requests, valid values are `sha1` (default) or `sha256` |
| `SHARELATEX_SAML_ADDITIONAL_PARAMS` | JSON dictionary of additional query params to add to all requests |
| `SHARELATEX_SAML_ADDITIONAL_AUTHORIZE_PARAMS` | JSON dictionary of additional query params to add to 'authorize' requests. <br /><br />**Example**: ` {"some_key": "some_value"} ` |
| `SHARELATEX_SAML_IDENTIFIER_FORMAT` | If present, name identifier format to request from identity provider (**Default**: `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`) |
| `SHARELATEX_SAML_ACCEPTED_CLOCK_SKEW_MS` | Time in milliseconds of skew that is acceptable between client and server when checking OnBefore and  NotOnOrAfter assertion condition validity timestamps. Setting to `-1` will disable checking these conditions entirely. **Default** is `0`. |
| `SHARELATEX_SAML_ATTRIBUTE_CONSUMING_SERVICE_INDEX` | **Optional**, `AttributeConsumingServiceIndex` attribute to add to AuthnRequest to instruct the IdP which attribute set to attach to the response ([link](http://blog.aniljohn.com/2014/01/data-minimization-front-channel-saml-attribute-requests.html)) |
| `SHARELATEX_SAML_AUTHN_CONTEXT` | If present, name identifier format to request auth context (**Default**: `urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport`) |
| `SHARELATEX_SAML_FORCE_AUTHN ` | If `true`, the initial SAML request from the service provider specifies that the IdP should force re-authentication of the user, even if they possess a valid session. |
| `SHARELATEX_SAML_DISABLE_REQUESTED_AUTHN_CONTEXT` | If `true`, do not request a specific auth context. For example, you can this this to `true` to allow additional contexts such as password-less logins (`urn:oasis:names:tc:SAML:2.0:ac:classes:X509`). Support for additional contexts is dependant on your IdP. |
| `SHARELATEX_SAML_SKIP_REQUEST_COMPRESSION ` | If set to `true`, the SAML request from the service provider won't be compressed. |
| `SHARELATEX_SAML_AUTHN_REQUEST_BINDING` | If set to `HTTP-POST`, will request authentication from IDP via HTTP POST binding, otherwise defaults to HTTP Redirect<br /><br /><strong>Note:</strong> If `SHARELATEX_SAML_AUTHN_REQUEST_BINDING` is set to `HTTP-POST`, then `SHARELATEX_SAML_SKIP_REQUEST_COMPRESSION` must also be set to `true`.|
| `SHARELATEX_SAML_VALIDATE_IN_RESPONSE_TO` | If truthy, then `InResponseTo` will be validated from incoming SAML responses |
| `SHARELATEX_SAML_REQUEST_ID_EXPIRATION_PERIOD_MS` | Defines the expiration time when a Request ID generated for a SAML request will not be valid if seen in a SAML response in the `InResponseTo` field.  **Default** is `8` hours. |
| `SHARELATEX_SAML_CACHE_PROVIDER` | Defines the implementation for a cache provider used to store request Ids generated in SAML requests as part of `InResponseTo` validation. **Default** is a built-in in-memory cache provider. <br /><br />See [link](https://github.com/node-saml/passport-saml/blob/master/README.md) for more information. |
| `SHARELATEX_SAML_LOGOUT_URL` | Base address to call with logout requests (**Default**: `entryPoint`) |
| `SHARELATEX_SAML_LOGOUT_CALLBACK_URL` | The value with which to populate the `Location` attribute in the `SingleLogoutService` elements in the generated service provider metadata. |
| `SHARELATEX_SAML_ADDITIONAL_LOGOUT_PARAMS` | JSON dictionary of additional query params to add to 'logout' requests |