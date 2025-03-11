
# IPSIE SL1 SAML 2.0 Profile

## Abstract

The IPSIE SL1 SAML 2.0 Profile defines a profile of SAML 2.0 intended to meet the security and interoperability requirements of enterprise integrations using SAML. This profile establishes requirements that satisfy the NIST Special Publication 800-63-4 Federation Assurance Level 2 (FAL2) for both Identity Providers and Service Providers to ensure secure, standardized, and compliant implementations.

## About This Document

This note is to be removed before publishing as an RFC.

The latest revision of this draft can be found at <TBD>. Status information for this document may be found at <TBD>.

Discussion of this document takes place on the IPSIE Working Group mailing list (mailto:ipsie@lists.openid.net), which is archived at https://openid.net/wg/ipsie/.

Source for this draft and an issue tracker can be found at <TBD>.

## Status of This Memo

This Internet-Draft is submitted in full conformance with the provisions of BCP 78 and BCP 79.

Internet-Drafts are working documents of the Internet Engineering Task Force (IETF). Note that other groups may also distribute working documents as Internet-Drafts. The list of current Internet-Drafts is at https://datatracker.ietf.org/drafts/current/.

Internet-Drafts are draft documents valid for a maximum of six months and may be updated, replaced, or obsoleted by other documents at any time. It is inappropriate to use Internet-Drafts as reference material or to cite them other than as "work in progress."

This Internet-Draft will expire on 11 September 2025.

## Copyright Notice

Copyright (c) 2025 IETF Trust and the persons identified as the document authors. All rights reserved.

This document is subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF Documents (https://trustee.ietf.org/license-info) in effect on the date of publication of this document. Please review these documents carefully, as they describe your rights and restrictions with respect to this document.
## Table of Contents

1. [Introduction](#1-introduction)
2. [Conventions and Definitions](#2-conventions-and-definitions)
   1. [Roles](#21-roles)
3. [Profile](#3-profile)
   1. [Network Layer Requirements](#31-network-layer-requirements)
      1. [Requirements for all endpoints](#311-requirements-for-all-endpoints)
      2. [Requirements for endpoints not used by web browsers](#312-requirements-for-endpoints-not-used-by-web-browsers)
      3. [Requirements for endpoints used by web browsers](#313-requirements-for-endpoints-used-by-web-browsers)
   2. [Cryptography and Signatures](#32-cryptography-and-signatures)
   3. [SAML Requirements](#33-saml-requirements)
      1. [Requirements for Identity Providers](#331-requirements-for-identity-providers)
      2. [Requirements for Service Providers](#332-requirements-for-service-providers)
   4. [FAL2 Specific Requirements](#34-fal2-specific-requirements)
      1. [Assertion Protection](#341-assertion-protection)
      2. [Holder-of-Key Requirements](#342-holder-of-key-requirements)
      3. [Authentication Requirements](#343-authentication-requirements)
      4. [Federation Metadata Management](#344-federation-metadata-management)
      5. [Privacy Requirements](#345-privacy-requirements)
      6. [FAL2 Requirements for Service Providers](#346-fal2-requirements-for-service-providers)
      7. [FAL2 Requirements for Identity Providers](#347-fal2-requirements-for-identity-providers)
4. [Security Considerations](#4-security-considerations)
5. [Compliance Statement](#5-compliance-statement)
6. [Normative References](#6-normative-references)
7. [Acknowledgments](#7-acknowledgments)
8. [Author's Address](#8-authors-address)

## 1. Introduction

This document defines the IPSIE Security Level 1 (SL1) Profile for SAML 2.0, which specifies the requirements for enterprise SAML implementations to achieve NIST 800-63-4 Federation Assurance Level 2 (FAL2) compliance. It provides a clear reference for SAML deployments that require a well-defined security baseline meeting federal guidelines for identity federation.

The profile addresses critical aspects of secure federation, with particular emphasis on:
* Multi-factor authentication enforcement
* Session lifecycle management
* NIST 800-63-4 FAL2 compliance requirements
* Secure communication of authentication context information
* Robust cryptographic protections

By adhering to this profile, organizations can implement SAML-based federations that meet stringent security requirements while ensuring interoperability across different implementations.

## 2. Conventions and Definitions

The keywords "shall", "shall not", "should", "should not", "may", and "can" in this document are to be interpreted as described in ISO Directive Part 2 [ISODIR2]. These keywords are not used as dictionary terms such that any occurrence of them shall be interpreted as keywords and are not to be interpreted with their natural language meanings.

### 2.1. Roles

This document uses the following terminology:
- "Identity Provider" (IdP) refers to the SAML Authority that issues assertions.
- "Service Provider" (SP) refers to the SAML Relying Party that consumes assertions.
- "Subscriber" refers to the end user being authenticated.

## 3. Profile

### 3.1. Network Layer Requirements

#### 3.1.1. Requirements for all endpoints

To protect against network attacks, Identity Providers and Service Providers:

- shall only offer TLS protected endpoints and shall establish connections to other servers using TLS;
- shall set up TLS connections using TLS version 1.2 or later;
- shall follow the recommendations for Secure Use of Transport Layer Security in [BCP195];
- shall use DNSSEC to protect against DNS spoofing attacks that can lead to the issuance of rogue domain-validated TLS certificates; and
- shall perform a TLS server certificate check, as per [RFC9525].

#### 3.1.2. Requirements for endpoints not used by web browsers

For server-to-server communication endpoints that are not used by web browsers, the following requirements apply:

- When using TLS 1.2, servers shall only permit the cipher suites recommended in [BCP195];
- When using TLS 1.2, clients shall only permit the cipher suites recommended in [BCP195].

#### 3.1.3. Requirements for endpoints used by web browsers

For endpoints that are used by web browsers, the following additional requirements apply:

- Servers shall use methods to ensure that connections cannot be downgraded using TLS stripping attacks. A preloaded HTTP Strict Transport Security policy [RFC6797] shall be used for this purpose.
- When using TLS 1.2, servers shall only use cipher suites allowed in [BCP195].
- Servers shall not support CORS for the SSO endpoints, as clients must perform an HTTP redirect rather than access these endpoints directly.

### 3.2. Cryptography and Signatures

The following requirements apply to cryptographic operations and signatures:

- Identity Providers and Service Providers when creating or processing SAML assertions shall:
  - adhere to XML Signature best practices [XMLDSig];
  - use RSA-SHA256, ECDSA-SHA256, or EdDSA (using the Ed25519 variant) algorithms for XML signatures;
  - not use unencrypted or unsigned assertions under any circumstances.

- RSA keys shall have a minimum length of 2048 bits.
- Elliptic curve keys shall have a minimum length of 256 bits.
- Credentials not intended for handling by end-users shall be created with at least 128 bits of entropy such that an attacker correctly guessing the value is computationally infeasible.
- All key material shall be generated using FIPS 140-2 Level 1 validated random bit generators or higher.
- Assertion signing keys shall be distinct from keys used for other purposes.

### 3.3. SAML Requirements

In the following, a profile of the following technologies is defined:

- SAML 2.0 Core [SAML2Core]
- SAML 2.0 Profiles [SAML2Profiles]
- SAML 2.0 Metadata [SAML2Metadata]
- SAML 2.0 Bindings [SAML2Bindings]

#### 3.3.1. Requirements for Identity Providers

Identity Providers:

- shall distribute metadata as specified in [SAML2Metadata] through a protected channel with integrity protection;
- shall only support pre-registered Service Providers with established trust relationships;
- shall authenticate Service Providers using X.509 certificates or another strong authentication method;
- shall sign all assertions using XML Signature [XMLDSig];
- shall encrypt all assertions containing personally identifiable information (PII) or sensitive attributes using XML Encryption [XMLEnc] with AES-GCM with a minimum key length of 128 bits;
- shall not expose open redirectors;
- shall only accept entity IDs registered in metadata as audience values;
- shall issue assertions with a maximum lifetime of 5 minutes;
- shall require Service Providers to be preregistered, and shall not support dynamic registration of Service Providers;
- shall support assertion revocation or status verification mechanisms (such as OCSP for certificate checking);
- shall implement key rotation procedures with a maximum key lifetime of 2 years for signing keys.

SAML Assertions issued by Identity Providers:

- shall contain the Service Provider's entity ID as the audience value;
- shall contain an AuthnContext element that identifies the authentication context class that the authentication performed satisfied;
- shall include an AuthnContextClassRef indicating the authentication method used, which must reflect multi-factor authentication per NIST 800-63-4 AAL2 requirements;
- shall indicate the expected lifetime of the Service Provider session in the SessionNotOnOrAfter attribute;
- shall include NotBefore and NotOnOrAfter conditions with a maximum validity period of 5 minutes;
- shall include a unique identifier for the assertion that is never reused;
- shall include a SubjectConfirmation element that implements holder-of-key binding when assertions contain sensitive information or authorize high-value transactions;
- shall include attribute statements that use pairwise pseudonymous identifiers for privacy protection when appropriate;
- shall minimize disclosure of personal information by only including required attributes in assertions.

For the Web Browser SSO Profile, Identity Providers:

- shall support HTTP-Redirect and HTTP-POST bindings for the SSO service;
- shall generate a unique ID for each assertion;
- shall include a ResponseID attribute that is unique for each response;
- shall not transmit SAML responses over unencrypted network connections;
- shall encrypt assertions using XML Encryption [XMLEnc] with AES-GCM with a minimum key length of 128 bits;
- shall reject authentication requests that have been previously processed (replay detection);
- shall not use the HTTP 307 status code when redirecting a request that contains user credentials;
- shall use the HTTP 303 status code when redirecting the user agent using status codes;
- shall support RelayState values up to 80 bytes in length, may reject RelayState values longer than 80 bytes;
- shall require multi-factor authentication for all authentication events and encode this fact in the AuthnContextClassRef element;
- shall maintain records of federation events for a minimum of 7 years or as required by applicable regulations;
- shall implement a federation termination mechanism that allows for immediate revocation of federation relationships.

#### 3.3.2. Requirements for Service Providers

Service Providers:

- shall support the SAML Web Browser SSO Profile as defined in [SAML2Profiles];
- shall support verification of signatures using X.509 certificates;
- shall support decryption of encrypted assertions using appropriate algorithms;
- shall not expose open redirectors;
- shall only use Identity Provider metadata retrieved from authoritative sources;
- shall ensure that the metadata URL used is obtained from an authoritative source and using a secure channel, such that it cannot be modified by an attacker;
- shall ensure that the entity ID used and the entity ID in the obtained metadata match;
- shall implement federation termination procedures that can be executed promptly when required;
- shall maintain session records that establish the link between the local session and the corresponding SAML assertion.

Service Providers processing SAML assertions:

- shall validate signatures on assertions and responses;
- shall verify that the assertion was issued to them by checking the audience restriction;
- shall check the validity period of the assertion;
- shall implement replay detection to prevent assertion reuse;
- shall validate the issuer of the assertion against trusted metadata;
- shall verify holder-of-key bindings when present in the assertion;
- shall enforce the session lifetime as specified in the assertion's SessionNotOnOrAfter attribute without exception;
- shall terminate user sessions immediately when the session lifetime specified in the assertion expires;
- shall not establish sessions with durations longer than specified in the SessionNotOnOrAfter attribute under any circumstances;
- shall respect authentication context requirements and shall not grant access to protected resources if the authentication context does not meet requirements;
- shall verify that multi-factor authentication was performed by checking the AuthnContextClassRef value;
- shall implement appropriate security controls around any PII received in assertions;
- shall implement appropriate subject identifier management that prevents correlation of subject identifiers across relying parties without user consent.

For the Web Browser SSO Profile, Service Providers:

- shall use signed authentication requests;
- shall verify the InResponseTo attribute matches the ID of the request that was sent;
- shall check the Issuer element in the SAML response to prevent mix-up attacks;
- shall implement the SAML Enhanced Client or Proxy (ECP) Profile for non-browser clients;
- shall safely store and manage RelayState data to prevent tampering;
- shall maintain logs of authentication events for a minimum of 7 years or as required by applicable regulations;
- shall implement appropriate access controls and encryption for stored assertion data;
- shall terminate local sessions when federation relationships are terminated.

### 3.4. FAL2 Specific Requirements

#### 3.4.1. Assertion Protection

- All assertions shall be cryptographically signed by the Identity Provider.
- All assertions containing PII or sensitive attributes shall be encrypted when transmitted through the front channel (via the browser).
- Encryption shall use approved algorithms and key sizes as specified in NIST SP 800-131A.
- Assertions shall include mechanisms to detect unauthorized modification or substitution.

#### 3.4.2. Holder-of-Key Requirements

- Identity Providers shall support holder-of-key bindings for high-assurance use cases.
- Service Providers shall validate holder-of-key bindings when present in assertions.
- Key material used for holder-of-key bindings shall be generated using FIPS 140-2 Level 1 validated random bit generators or higher.
- Key material shall be properly protected in storage and transit.

#### 3.4.3. Authentication Requirements

- Identity Providers shall require multi-factor authentication (MFA) in accordance with NIST 800-63-4 AAL2.
- Identity Providers shall communicate the authentication method used to the Service Provider through the AuthnContextClassRef element.
- Service Providers shall enforce MFA requirements by checking the AuthnContextClassRef value.
- Authentication events shall be logged and stored securely.

#### 3.4.4. Federation Metadata Management

- Both Identity Providers and Service Providers shall protect the integrity of federation metadata.
- Federation metadata shall be cryptographically signed.
- Federation metadata shall be refreshed at least daily.
- Key rotation procedures shall be implemented with a maximum key lifetime of 2 years.

#### 3.4.5. Privacy Requirements

- Identity Providers shall support pairwise pseudonymous identifiers when appropriate.
- Service Providers shall not correlate identifiers across relying parties without user consent.
- Both Identity Providers and Service Providers shall minimize the collection, storage, and transmission of PII.
- Both Identity Providers and Service Providers shall implement appropriate retention policies for PII.

#### 3.4.6. FAL2 Requirements for Service Providers

To meet NIST 800-63-4 FAL2 compliance, Service Providers:

- shall enforce session management based on the session lifetime parameters received in the assertion;
- shall treat the SessionNotOnOrAfter attribute as the authoritative source for session expiration;
- shall implement session monitoring that actively enforces the session duration limits specified in assertions;
- shall maintain verifiable technical controls that prevent the extension of sessions beyond their authorized lifetime;
- shall document how session lifetime enforcement is implemented for audit and compliance purposes;
- shall implement all cryptographic requirements specified in NIST 800-63-4 FAL2;
- shall implement assertion validation procedures consistent with FAL2 requirements;
- shall maintain logs of session creation, management, and termination that reference the controlling assertion identifiers.

#### 3.4.7. FAL2 Requirements for Identity Providers

To meet NIST 800-63-4 FAL2 compliance, Identity Providers:

- shall require multi-factor authentication (MFA) for all authentication events without exception;
- shall reject authentication attempts that do not satisfy MFA requirements;
- shall communicate the authentication methods used through the AuthnContextClassRef element in all assertions;
- shall use standardized authentication class references as specified in NIST 800-63-4;
- shall include sufficient contextual information in assertions to allow Service Providers to make informed authorization decisions;
- shall maintain verifiable technical controls that prevent the issuance of assertions without MFA completion;
- shall document MFA enforcement mechanisms for audit and compliance purposes;
- shall implement all cryptographic requirements specified in NIST 800-63-4 FAL2;
- shall protect all assertions according to FAL2 requirements.

## 4. Security Considerations

This profile aims to ensure compliance with NIST 800-63-4 FAL2 requirements. Implementers should be aware of additional security concerns specific to their deployments, including:

- XML signature wrapping attacks
- XML external entity (XXE) attacks
- XML Encryption vulnerabilities
- Cross-site scripting in the content of SAML messages
- Session management vulnerabilities
- Replay attacks on assertions
- Phishing and social engineering attacks
- Insider threats and privileged user management
- Continuous monitoring and incident response capabilities

Implementers shall conduct regular security assessments and penetration testing of their SAML implementations to ensure ongoing compliance with FAL2 requirements.

The following specific considerations apply to implementations of this profile:

1. **Session Management**: Service Providers must implement robust session management controls to enforce the session lifetime as specified in assertions. This includes mechanisms to detect and prevent session hijacking, fixation, and other session-based attacks.

2. **Authentication Context**: Service Providers must validate the authentication context provided by Identity Providers and ensure it meets the requirements for the requested resource or service.

3. **Assertion Validation**: Service Providers must implement comprehensive assertion validation, including signature verification, audience restriction checking, and replay detection.

4. **Key Management**: Both Identity Providers and Service Providers must implement secure key management practices, including secure key generation, storage, and rotation.

5. **Logging and Monitoring**: Both Identity Providers and Service Providers must implement comprehensive logging and monitoring of federation events to detect and respond to security incidents.

## 5. Compliance Statement

Implementation of all mandatory requirements in this profile will result in a SAML deployment that satisfies NIST Special Publication 800-63-4 Federation Assurance Level 2 (FAL2) requirements. The profile specifically ensures:

1. Service Providers strictly enforce session lifetimes as specified in the assertions they receive.
2. Identity Providers enforce multi-factor authentication and communicate authentication class information to Service Providers.
3. Both parties implement all cryptographic, privacy, and security controls required for FAL2 compliance.

Organizations implementing this profile should conduct a formal assessment to verify compliance with all applicable requirements.

This profile addresses the specific NIST 800-63-4 FAL2 requirements through the following controls:

1. **Authenticated Protected Channel**: All communication is protected using TLS 1.2 or higher with approved cipher suites.
2. **Signed Assertions**: All assertions are cryptographically signed by the Identity Provider.
3. **Encrypted Assertions**: All assertions containing PII or sensitive attributes are encrypted.
4. **Holder-of-Key**: Support for holder-of-key bindings for high-assurance use cases.
5. **Assertion Lifetimes**: Strict enforcement of assertion and session lifetimes.
6. **Multi-Factor Authentication**: Requirement for multi-factor authentication and communication of authentication context information.

## 6. Normative References

[SAML2Core]
Cantor, S., Kemp, J., Philpott, R., and E. Maler, "Assertions and Protocol for the OASIS Security Assertion Markup Language (SAML) V2.0", OASIS Standard, March 2005.

[SAML2Profiles]
Hughes, J., Cantor, S., Hodges, J., Hirsch, F., Mishra, P., Philpott, R., and E. Maler, "Profiles for the OASIS Security Assertion Markup Language (SAML) V2.0", OASIS Standard, March 2005.

[SAML2Metadata]
Cantor, S., Moreh, J., Philpott, R., and E. Maler, "Metadata for the OASIS Security Assertion Markup Language (SAML) V2.0", OASIS Standard, March 2005.

[SAML2Bindings]
Cantor, S., Hirsch, F., Kemp, J., Philpott, R., and E. Maler, "Bindings for the OASIS Security Assertion Markup Language (SAML) V2.0", OASIS Standard, March 2005.

[BCP195]
Best Current Practice 195.
At the time of writing, this BCP comprises the following:
Moriarty, K. and S. Farrell, "Deprecating TLS 1.0 and TLS 1.1", BCP 195, RFC 8996, March 2021.
Sheffer, Y., Saint-Andre, P., and T. Fossati, "Recommendations for Secure Use of Transport Layer Security (TLS) and Datagram Transport Layer Security (DTLS)", BCP 195, RFC 9325, November 2022.

[RFC6797]
Hodges, J., Jackson, C., and A. Barth, "HTTP Strict Transport Security (HSTS)", RFC 6797, November 2012.

[RFC9525]
Saint-Andre, P. and R. Salz, "Service Identity in TLS", RFC 9525, November 2023.

[XMLDSig]
Eastlake, D., Reagle, J., Solo, D., Hirsch, F., and T. Roessler, "XML Signature Syntax and Processing Version 1.1", W3C Recommendation, April 2013.

[XMLEnc]
Eastlake, D., Reagle, J., Hirsch, F., and T. Roessler, "XML Encryption Syntax and Processing Version 1.1", W3C Recommendation, April 2013.

[NIST800-63-4]
"Digital Identity Guidelines", NIST Special Publication 800-63 (2nd Public Draft), Rev. 4, August 2024.

[NIST800-131A]
Barker, E. and A. Roginsky, "Transitioning the Use of Cryptographic Algorithms and Key Lengths", NIST Special Publication 800-131A, Rev. 2, March 2019.

[ISODIR2]
International Organization for Standardization, "ISO/IEC Directives, Part 2: Principles and rules for the structure and drafting of ISO and IEC documents", 2021.

## 7. Acknowledgments

This profile builds upon the work of the IPSIE SL1 OpenID Connect Profile and adapts those requirements to the SAML context. The structure and requirements have been designed to align with those in the OpenID Connect Profile while respecting the differences in the protocols and ensuring compliance with NIST 800-63-4 FAL2 requirements.

## 8. Author's Address

Matt Topper
UberEther
Email: matt@uberether.com
