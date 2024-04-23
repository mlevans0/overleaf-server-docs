---
tags:
  - Server Pro
---

{{ versions['server-pro-short'] }} provides SAML 2.0 server integration for user authentication. For {{ versions['toolkit-short'] }} deployments, the SAML 2.0 integration is configured via the [`variables.env`](/configuration/overleaf-toolkit#the-variablesenv-file) file.

You can see a full list of available configuration options for [SAML 2.0](environment-variables.md#saml-20) over on the [Environments variables](environment-variables.md) page. 

## Configuration ##

Internally, the Overleaf SAML 2.0 integration uses the [passport-SAML](https://github.com/vesse/passport-saml) library. Most of these configuration options are passed through to the `server` configuration object which is used to configure `passport-SAML`. 

If you are having issues configuring SAML 2.0, it is worth reading the README for `passport-SAML` to get a feel for the configuration it expects.

To enable the SAML 2.0 module, the `EXTERNAL_AUTH` variable must be set to `saml`:

```
EXTERNAL_AUTH=saml
```

!!! note

    To preserve backward compatibility with older configuration files, if `EXTERNAL_AUTH` is not set, but `OVERLEAF_SAML_ENTRYPOINT` is set, then the SAML 2.0
    module will be activated. We still recommend setting `EXTERNAL_AUTH` explicitly

### Passing keys and certificates ###

As of Server Pro `2.7.0`:

- The value of the `OVERLEAF_SAML_CERT` environment variable cannot be empty if SAML 2.0 is enabled (with `EXTERNAL_AUTH=saml`, or if `OVERLEAF_SAML_ENTRYPOINT` is set).

As of Server Pro `2.5.0`:

- The value of the `OVERLEAF_SAML_CERT` environment variable must be passed in single-line format (without the begin and end lines from the PEM format; see below for more information).
- The value of the `OVERLEAF_SAML_PRIVATE_CERT` environment variable should be a full path to a file which contains the private key in PEM format.
- The value of the `OVERLEAF_SAML_DECRYPTION_PVK` environment variable must be passed in PEM format (multi-line). (But single-line may be [supported soon](https://github.com/node-saml/passport-saml/issues/524).)

To pass a key or certificate in single-line format, you can just specify it as a string (don't include the begin or end lines, any internal whitespace, or any newline escapes, e.g. `\n`, also do not add quotes):

```env
OVERLEAF_SAML_CERT=MIIEowIBAAKCAQEAxmJWY0eJcuV2uBtLnQ4004fuknbODo5xIyRhkYNkls5n9OrBq4Lok6cjv7G2Q8mxAdlIUmzhTSyuNkrMMKZrPaMsAkNKE/aNpeWuSLXqcMs8T/8gYCDcEmC5KYEJakNtKb3ZX2FKwT4yHHpsNomLDzJD5DyJKbRpNBm2no7ggIy7TQRJ2H00mogQIQu8/fUANXVeGPshvLJU8MXEy/eiXkHJIT3DDA4VSr/C/tfP0tGJSNTM874urc4zej+4INuTuMPtesZS47J0AsPxQuxengS4M76cVt5cH+Iqd1nKe5UqiSKvLCXacPYg/T/Kdx0tBnwHIjKo/cbzZ+r+XynsCwIDAQABAoIBAFPWWwu5v6x+rJ1Ba8MDre93Eqty6cHdEJL5XQJRtMDGmcg3LYF94SwFBmaMg6pCIjvVx2qN+OjUaQsosQIeUlPKEV8jcLrfBx2E4xJ3Tow8V1C3UMdPG7Hojler4H633/oz8RkN1Lm1vxep5PFnTw0tAOQDcTPeulb6RuLbHqU0FEnf/jVOMhtPLcMAwJ3fkAJQ+ljFW2VKCQ83d+ci1p+NHY/dbGLSR4lK58mVghcRMO3zhe5scrbECHJMfT6fCb2TXdjaueFUGC6+fqUXvDj8HRfUilzTegNq8ZhwgMSw1HeX/PuiczSKc3aHYSsohMBugTErnkW+qF4ZkE+kxgECgYEA/sm7umcyFuZME+RWYL8Gsp8agH1OGEgsmIiMi1z6RTlTmdR8fN18ItzXyW+363VZln/1b5wCaPdLIxgASxybLAaxnKAXfmL7QvyVAaMwxj7N0ogvMQoNx2VuSGZSam2+LFVIMWHq1C+3fvVnCDLm6oHvIMK/zvEsPBBtz+L6rlECgYEAx1PrKogaGHCi1XgsrNv9aFaayRvmhzZbmiigF0iWKAd3KKww94BdyyGSVfMfyL23LAbMQDCrDNGpYAnpNZo/cL+OcGPYzlPsWDBrJub1HOA/H3WQlP4oEcfdbmJZhIkEwTGFHaCHynEu4ekiCrWz9+XVNCquTyqnmaVDEzAfEZsCgYA8jQbfUt0Vkh+sboyUq3FVC/jJZn4jyStICNOV3z/fKbOTkGsRZbW1t1RVHAbSn23uFXTn1GTCO1sQ+QhA0YiTGvgk5+sNb0qVbd+fpv/VbWGO0iyc8+24YIOoEyEtB+21LYNdsQ6U5M4wDvQwf6BfRQfmekIJVUmU8LaYPDIlMQKBgDSRiT/aTSeM7STnYMDl89sEnCXV2eJnD5mEhVQerJs5/M8ZOoDLtfDQlctdJ1DF1/0gfdWgADyNPuI5OuwMFhciLequKoufzoEjo97KonJPIdamJs9kiCTIVTm7bmhpyns5GCZMJAPb/cVOus+gRCpozuXHK9ltIm5/C0WQN2FpAoGBAOss6RN2krieqbn1mG8e2v5mMUd0CJkiJu2y5MnF3dYHXSQ3/ePAh/YgJOthpgYgBh+mV0DLqJhx/1DLS/xiqcoHDlndQDmYbtvvY7RlMo00+nGzkRVOfrqyhC+1KsYHGPbSQixNQXtvFbAAVMSo+RRBkVGINYGDFnlQUpkppYRk
```

To pass a key or certificate in multi-line format, wrap the entire value in double quotes and use new line characters (`\n`) as usual:

```env
OVERLEAF_SAML_DECRYPTION_PVK="-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAxmJWY0eJcuV2uBtLnQ4004fuknbODo5xIyRhkYNkls5n9OrB
q4Lok6cjv7G2Q8mxAdlIUmzhTSyuNkrMMKZrPaMsAkNKE/aNpeWuSLXqcMs8T/8g
YCDcEmC5KYEJakNtKb3ZX2FKwT4yHHpsNomLDzJD5DyJKbRpNBm2no7ggIy7TQRJ
2H00mogQIQu8/fUANXVeGPshvLJU8MXEy/eiXkHJIT3DDA4VSr/C/tfP0tGJSNTM
874urc4zej+4INuTuMPtesZS47J0AsPxQuxengS4M76cVt5cH+Iqd1nKe5UqiSKv
LCXacPYg/T/Kdx0tBnwHIjKo/cbzZ+r+XynsCwIDAQABAoIBAFPWWwu5v6x+rJ1B
a8MDre93Eqty6cHdEJL5XQJRtMDGmcg3LYF94SwFBmaMg6pCIjvVx2qN+OjUaQso
sQIeUlPKEV8jcLrfBx2E4xJ3Tow8V1C3UMdPG7Hojler4H633/oz8RkN1Lm1vxep
5PFnTw0tAOQDcTPeulb6RuLbHqU0FEnf/jVOMhtPLcMAwJ3fkAJQ+ljFW2VKCQ83
d+ci1p+NHY/dbGLSR4lK58mVghcRMO3zhe5scrbECHJMfT6fCb2TXdjaueFUGC6+
fqUXvDj8HRfUilzTegNq8ZhwgMSw1HeX/PuiczSKc3aHYSsohMBugTErnkW+qF4Z
kE+kxgECgYEA/sm7umcyFuZME+RWYL8Gsp8agH1OGEgsmIiMi1z6RTlTmdR8fN18
ItzXyW+363VZln/1b5wCaPdLIxgASxybLAaxnKAXfmL7QvyVAaMwxj7N0ogvMQoN
x2VuSGZSam2+LFVIMWHq1C+3fvVnCDLm6oHvIMK/zvEsPBBtz+L6rlECgYEAx1Pr
KogaGHCi1XgsrNv9aFaayRvmhzZbmiigF0iWKAd3KKww94BdyyGSVfMfyL23LAbM
QDCrDNGpYAnpNZo/cL+OcGPYzlPsWDBrJub1HOA/H3WQlP4oEcfdbmJZhIkEwTGF
HaCHynEu4ekiCrWz9+XVNCquTyqnmaVDEzAfEZsCgYA8jQbfUt0Vkh+sboyUq3FV
C/jJZn4jyStICNOV3z/fKbOTkGsRZbW1t1RVHAbSn23uFXTn1GTCO1sQ+QhA0YiT
Gvgk5+sNb0qVbd+fpv/VbWGO0iyc8+24YIOoEyEtB+21LYNdsQ6U5M4wDvQwf6Bf
RQfmekIJVUmU8LaYPDIlMQKBgDSRiT/aTSeM7STnYMDl89sEnCXV2eJnD5mEhVQe
rJs5/M8ZOoDLtfDQlctdJ1DF1/0gfdWgADyNPuI5OuwMFhciLequKoufzoEjo97K
onJPIdamJs9kiCTIVTm7bmhpyns5GCZMJAPb/cVOus+gRCpozuXHK9ltIm5/C0WQ
N2FpAoGBAOss6RN2krieqbn1mG8e2v5mMUd0CJkiJu2y5MnF3dYHXSQ3/ePAh/Yg
JOthpgYgBh+mV0DLqJhx/1DLS/xiqcoHDlndQDmYbtvvY7RlMo00+nGzkRVOfrqy
hC+1KsYHGPbSQixNQXtvFbAAVMSo+RRBkVGINYGDFnlQUpkppYRk
-----END RSA PRIVATE KEY-----"
```

!!! warning

    The above private key is an example key from the [`xml-encryption`](https://github.com/auth0/node-xml-encryption/blob/435b1d0f01b1f218c51f07b01fb90df4a4e108de/test/test-auth0.key) library's test suite. **Do not use this key.**

### Metadata for the identity provider ###

Your Identity Provider (IdP) will need to be configured to recognize your {{versions['server-pro-short']}} instance as a Service Provider (SP). How this is done will vary between different IdP's - we therefore recommend that you consult the documentation for your SAML 2.0 server for instructions on how to do this.

As of version `2.6.0`, {{versions['server-pro-short']}} includes a metadata endpoint which can be used to retrieve Service Provider Metadata from, for example `http://my-overleaf-instance.com/saml/meta`

Here is an example of appropriate Service Provider (SP) metadata, note the `AssertionConsumerService.Location`, `EntityDescriptor.entityID` and `EntityDescriptor.ID` properties, and set as appropriate.

```
<?xml version="1.0"?>
<EntityDescriptor xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
                  xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
                  entityID="sharelatex-saml"
                  ID="OVERLEAF_saml">
  <SPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
    <NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress</NameIDFormat>
    <AssertionConsumerService index="1"
                              isDefault="true"
                              Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                              Location="https://sharelatex.example.com/saml/callback" />
  </SPSSODescriptor>
</EntityDescriptor>
```

## Example configuration ##

At Overleaf, we test the SAML 2.0 integration against a SAML 2.0 test server. The following is an example of a working configuration:

```
# added to variables.env

EXTERNAL_AUTH=saml
OVERLEAF_SAML_ENTRYPOINT=http://localhost:8081/simplesaml/saml2/idp/SSOService.php
OVERLEAF_SAML_CALLBACK_URL=http://saml/saml/callback
OVERLEAF_SAML_ISSUER=sharelatex-test-saml
OVERLEAF_SAML_IDENTITY_SERVICE_NAME=SAML Test Server
OVERLEAF_SAML_EMAIL_FIELD=email
OVERLEAF_SAML_FIRST_NAME_FIELD=givenName
OVERLEAF_SAML_LAST_NAME_FIELD=sn
OVERLEAF_SAML_UPDATE_USER_DETAILS_ON_LOGIN=true
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

- Edit `variables.env` to add the SAML 2.0 Environment Variables as listed above.
- Restart Server Pro.

You should be able to login using `sally` as username and `sally123` as password.