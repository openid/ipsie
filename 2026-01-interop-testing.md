# Interop Testing Checklist

January 2026


## IDPs testing RPs

The below checklist is for you if you are coming to the interop event representing an IDP. Use the checklist below to test whether each RP interoperates with your IDP according to the IPSIE SL1 requirements.


### OpenID IPSIE SL1

https://openid.github.io/ipsie-openid-sl1/draft-openid-ipsie-sl1-profile.html#section-3.2.2

Configuration and Discovery

* [ ] The RP supports third-party initiated login 
* [ ] The RP uses only HTTPS URLs as the redirect URI values ([link](https://openid.github.io/ipsie-common-requirements-profile/draft-ipsie-common-requirements-profile.html#section-3.2.1))

Client Authentication and Requests

* [ ] The RP validates the TLS certificate of the IDP when connecting (authorization server metadata, token endpoint, etc)
* [ ] The RP includes the authorization server issuer identifier in client authentication JWTs
* [ ] The RP uses recommended ciphers ([link](https://openid.github.io/ipsie-common-requirements-profile/draft-ipsie-common-requirements-profile.html#section-3.2.2))
* [ ] The RP requests user info from the OP using DPoP
* [ ] The RP supports the server-provided nonce mechanism of DPoP
* [ ] The RP does not send access tokens in the POST body or GET query string (See OIDC conformance tests §3.1, "Accesses UserInfo Endpoint with header method".)

Authorization Code Request

* [ ] The RP requests `response_type=code` 
* [ ] The RP includes a PKCE `code_challenge` with S256
* [ ] The RP does not hard-code the PKCE `code_challenge`, and uses a unique value per request

Authorization Code Response

* [ ] The RP rejects a request to the redirect URI with an invalid `iss` in the response (See OIDC conformance tests §3.2, "Reject ID Token with invalid iss claim".)

ID Tokens

* [ ] The RP rejects an ID token with an incorrect `aud` claim that is not the `client_id` ((See OIDC conformance tests §3.2, "Reject ID Token with invalid iss claim".  TODO: Verify how this is tested.)
* [ ] The RP re-validates the session after the `session_expiry` claim (the user is redirected to the IDP after this time, or the RP uses a refresh token to get a new ID token)


## RPs testing IDPs

The below checklist is for you if you are coming to the interop event representing an RP. Use the checklist below to test whether each IDP interoperates with your RP according to the IPSIE SL1 requirements.


### OpenID IPSIE SL1

https://openid.github.io/ipsie-openid-sl1/draft-openid-ipsie-sl1-profile.html#section-3.2.1

Configuration and Discovery

* [ ] The IDP uses only HTTPS URLs using TLS 1.2 or later ([link](https://openid.github.io/ipsie-common-requirements-profile/draft-ipsie-common-requirements-profile.html#section-3.2.1))
* [ ] The IDP uses recommended ciphers ([link](https://openid.github.io/ipsie-common-requirements-profile/draft-ipsie-common-requirements-profile.html#section-3.2.2))
* [ ] The IDP has enabled DNSSEC
* [ ] The IDP publishes OpenID Server Metadata
* [ ] The IDP rejects the Resource Owner Password Credentials grant for this RP
* [ ] The IDP allows this RP to be configured as a public client
* [ ] The IDP only accepts its issuer value as the `aud` in client authentication assertions (does not accept its token endpoint as a value)
* [ ] The IDP does not support unauthenticated Dynamic Client Registration
* [ ] The IDP has a method to preregister the client including its complete redirect URI
* [ ] The IDP does not accept wildcard redirect URIs
* [ ] The IDP does not accept `http` redirect URIs

Authorization Code Request

* [ ] The IDP ensures connections to the authorization endpoint cannot be downgraded ([link](https://openid.github.io/ipsie-common-requirements-profile/draft-ipsie-common-requirements-profile.html#section-3.2.3))
* [ ] The IDP supports `response_type=code`(See OIDC conformance tests §3.1, "Support code response_type".)
* [ ] The IDP rejects other `response_type` values (`token`, `id_token`, `code id_token`)
* [ ] The IDP rejects an authorization request without PKCE
* [ ] The IDP rejects unrecognized `redirect_uri` values
* [ ] The IDP accepts a nonce value with 64 characters
* [ ] The IDP accepts the `max_age` parameter and reauthenticates the user appropriately (See OIDC conformance tests §3.1, "Support max_age request
parameter", "Support max_age request parameter when max age reached", "Support max_age request parameter when max age not reached.")
* [ ] The IDP does not allow CORS-enabled requests to the authorization endpoint

Authorization Code Response

* [ ] The IDP returns an `iss` value in the authorization response (See OIDC conformance tests §3.1, "ID Token has iss claim".)
* [ ] The IDP does not redirect the browser to the redirect URI with HTTP 307

Token Request

* [ ] The IDP rejects authorization codes older than 60 seconds
* [ ] The IDP rejects an authorization code that has already been used (See OIDC conformance tests §3.1, "Reject second use of Authorization Code".)

ID Tokens

* [ ] The IDP signs ID tokens with a recommended algorithm (PS256, ES256, or EdDSA (using the Ed25519 variant) algorithms) ([link](https://openid.github.io/ipsie-common-requirements-profile/draft-ipsie-common-requirements-profile.html#section-3.3))
* [ ] The ID token contains the OAuth Client ID as a single `aud` value as a string 
* [ ] The ID token contains the `acr` claim as a string (See OIDC conformance tests §3.1, "Support acr_values request parameter".)
* [ ] The ID token contains the `amr` claim as an array of strings 
* [ ] The ID token contains the `auth_time` claim 
* [ ] The ID token contains the `session_expiry` claim 

**References**
[OIDC Conformance Profiles](https://openid.net/wordpress-content/uploads/2018/06/OpenID-Connect-Conformance-Profiles.pdf)



