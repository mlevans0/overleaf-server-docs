---
tags:
  - Server Pro
---

{{ versions['server-pro-short'] }} provides SAML2 server integration for user authentication. For {{ versions['toolkit-short'] }} deployments, the SAML2 integration is configured via the [`variables.env`](/configuration/overleaf-toolkit#the-variablesenv-file) file.

## Configuration

Internally, the Overleaf SAML integration uses the [passport-SAML](https://github.com/vesse/passport-saml) library. Most of these configuration options are passed through to the `server` config object which is used to configure `passport-SAML`. If you are having issues configuring SAML, it is worth reading the README for `passport-SAML` to get a feel for the configuration it expects.

The `EXTERNAL_AUTH` variable must be set to `saml`, to enable the SAML module:

```
EXTERNAL_AUTH=saml
```

You can see a full list of available configuration options over on the [Environments variables](environment-variables.md#saml2) page. 

!!! note

    To preserve backward compatibility with older configuration files, if `EXTERNAL_AUTH` is not set, but `SHARELATEX_SAML_ENTRYPOINT` is set, then the SAML
    module will be activated. We still recommend setting `EXTERNAL_AUTH` explicitly

## Example configuration ##

At Overleaf, we test the SAML integration against a SAML test server. The following is an example of a working configuration:

```
# added to variables.env

EXTERNAL_AUTH=saml
SHARELATEX_SAML_ENTRYPOINT=http://localhost:8081/simplesaml/saml2/idp/SSOService.php
SHARELATEX_SAML_CALLBACK_URL=http://saml/saml/callback
SHARELATEX_SAML_ISSUER=sharelatex-test-saml
SHARELATEX_SAML_IDENTITY_SERVICE_NAME=SAML Test Server
SHARELATEX_SAML_EMAIL_FIELD=email
SHARELATEX_SAML_FIRST_NAME_FIELD=givenName
SHARELATEX_SAML_LAST_NAME_FIELD=sn
SHARELATEX_SAML_UPDATE_USER_DETAILS_ON_LOGIN=true
```

The `sharelatex/saml-test` image needs to run in the same network as the `sharelatex` container (which by default would be `overleaf_default`), so we'll proceed with the following steps:

- Run `docker network create overleaf_default` (will possibly fail due to a `network with name overleaf_default already exists` error, that's ok).
- Start `saml-test` container with some environment parameters:

```
docker run --network=overleaf_default --name=saml                 \
    --publish='8081:80'                                           \
    --env SAML_BASE_URL_PATH='http://localhost:8081/simplesaml/'  \
    --env SAML_TEST_SP_ENTITY_ID='sharelatex-test-saml'           \
    --env SAML_TEST_SP_LOCATION='http://localhost/saml/callback'  \
    sharelatex/saml-test 
```

- Edit `variables.env` to add the SAML Environment Variables as listed above.
- Restart Server Pro.

You should be able to login using `sally` as username and `sall123` as password.