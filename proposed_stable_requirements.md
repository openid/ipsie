Stable requirements are highlighted in _italics_ and may be used to develop conformance tests as the draft continued to be developed.  The sections below remain intact from the original requirements found [here](https://github.com/openid/ipsie-openid-sl1/blob/d322ccf62970e2eb23a507d96c7119cc7a518df2/draft-openid-ipsie-sl1-profile.md).

### Requirements for OpenID Providers

OpenID Providers:

* _MUST distribute discovery metadata (such as the authorization endpoint) via the metadata document as specified in [OpenID.Discovery];_
* _MUST reject requests using the resource owner password credentials grant;_
* _MUST support public clients as defined in [RFC6749];_
* _MUST NOT expose open redirectors {{Section 4.11 of RFC9700}};_
* _MUST only accept its issuer identifier value (as defined in [RFC8414]) as a string in the `aud` claim received in client authentication assertions;_
* _MUST issue authorization codes with a maximum lifetime of 60 seconds;_
* _MUST require clients to be preregistered, and MUST NOT support unauthenticated Dynamic Client Registration requests (see Note 1);_
* _MUST require clients to pre-register their redirect URIs;_

Access Tokens issued by OpenID Providers:

* _MUST only be used by the RP to retrieve identity claims at the OpenID Provider;_
* _SHOULD only issue sender-constrained access tokens using DPoP [RFC9449];_

ID Tokens issued by OpenID Providers:

* _MUST contain the OAuth Client ID of the RP as a single audience value as a string (see Note 2);_
* MUST contain the `acr` claim as a string that identifies the Authentication Context Class that the authentication performed satisfied, as described in Section 2 of [OpenID];
* MUST contain the `amr` claim as an array of strings indicating identifiers for authentication methods used in the authentication from those registered in the IANA Authentication Method Reference Values registry, as described in Section 2 of [OpenID];
* MUST contain the `auth_time` claim to describe when end user authentication last occurred (see Note 4);
* MUST indicate the expected expiration time of the RP session in the `session_expiry` claim as a JSON integer that represents the Unix timestamp (seconds since epoch). (see Note 3);

Note 1: The requirement for preregistered clients corresponds to Section 3.4 "Trust Agreements" of [NIST.FAL].

Note 2: The audience value must be a single string to meet the audience restriction of [NIST.FAL].

Note 3: This claim is currently being defined in the AB Connect WG.  See the latest draft at https://openid.github.io/connect-enterprise-extensions/main.html.

Note 4: This claim is required to satisfy the requirements in Section 4.7 of [NIST.FAL].


For the authorization code flow, OpenID Providers:

* _MUST require the value of `response_type` described in [RFC6749] to be `code`;_
* _MUST require PKCE [RFC7636] with S256 as the code challenge method (see Note 1 below);_
* _MUST require an exact match of a registered redirect URI as described in {{Section 2.1 of RFC9700}};_
* _MUST issue authorization codes with a maximum lifetime of 60 seconds;_
* _MUST return an `iss` parameter in the authorization response according to [RFC9207];_
* _MUST NOT transmit authorization responses over unencrypted network connections, and, to this end, MUST NOT allow redirect URIs that use the `http` scheme;_
* _MUST reject an authorization code (Section 1.3.1 of [RFC6749]) if it has been previously used;_
* _MUST NOT use the HTTP 307 status code when redirecting a request that contains user credentials to avoid forwarding the credentials to a third party accidentally (see {{Section 4.12 of RFC9700}});_
* _SHOULD use the HTTP 303 status code when redirecting the user agent using status codes;_
* _MUST support `nonce` parameter values up to 64 characters in length, and MAY reject `nonce` values longer than 64 characters._
* _MUST support the `max_age` parameter with a values representing the maximum number of seconds allowable since the user was authenticated by the OP. If the elapsed time since authentication is less than this value, the OP MAY choose to actively reauthenticate the user.  If the elapsed time since authentication is greater than this value, the OP MUST actively reauthenticate the user._

Note 1: while both nonce and PKCE can provide protection from authorization code injection, nonce relies on the client (RP) to implement and enforce the check, and the IdP is unable to verify that it has been implemented correctly, and only stops the attack after tokens have already been issued. Instead, PKCE is enforced by the IdP and stops the attack before tokens are issued.



### Requirements for OpenID Relying Parties

OpenID Relying Parties:

* _MUST support third-party initiated login as defined in Section 4 of [OpenID];_
* _MUST use the authorization server's issuer identifier value (as defined in [RFC8414]) in the `aud` claim in client authentication assertions. The issuer identifier value shall be sent as a string not as an item in an array;_
* _MUST NOT expose open redirectors (see {{Section 4.11 of RFC9700}});_
* _MUST only use authorization server metadata (such as the authorization endpoint) retrieved from the metadata document as specified in [OpenID.Discovery] and [RFC8414];_
* _MUST ensure that the issuer URL used as the basis for retrieving the authorization server metadata is obtained from an authoritative source and using a secure channel, such that it cannot be modified by an attacker;_
* _MUST ensure that this issuer URL and the issuer value in the obtained metadata match;_

OpenID Relying Parties making resource requests to the OpenID Provider:

* _MUST support sender-constrined access tokens using DPoP as described in [RFC9449];_
* _MUST support the server-provided nonce mechanism (as defined in {{Section 8 of RFC9449}});_
* _MUST send access tokens in the HTTP header as described in {{Section 7.1 of RFC9449}};_

For the authorization code flow, Relying Parties:

* _MUST use the authorization code grant described in [RFC6749];_
* _MUST use PKCE [RFC7636] with S256 as the code challenge method;_
* _MUST generate the PKCE challenge specifically for each authorization request and securely bind the challenge to the client and the user agent in which the flow was started;
_* _MUST check the `iss` parameter in the authorization response according to [RFC9207] to prevent mix-up attacks;_
* _SHOULD NOT use `nonce` parameter values longer than 64 characters;_
* _SHOULD use the `max_age` parameter in the authentication request to specify the maximum allowable authentication age to the OP in seconds.  The value of the `max_age` parameter MAY be determined based upon the business rules of the RP._

In addition to the _ID Token validation requirements described in Section 3.1.37 of [OpenID]_, Relying Parties:

* _MUST validate that the `aud` claim is a single string and matches the OAuth Client ID of the RP;_
* MUST re-authenticate the user through the OpenID Provider after the time indicated in the `session_expiry` claim, by either initiating a new authorization code flow, or by requesting a new ID token using a previously obtained refresh token (see Note 1);

Note 1: This claim is currently being defined in the AB Connect WG.  See the latest draft at https://openid.github.io/connect-enterprise-extensions/main.html.
