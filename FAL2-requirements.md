The information below was copied from the [SP800-63C rev4 DRAFT](https://pages.nist.gov/800-63-4/sp800-63c.html) available as of May 1, 2025.  This derivation of the original document is designed to highlight the controls required for IPSIE profiles to meet the guidelines at FAL2.  There are some controls that may not be appropriate for IPSIE SL1, these should be identified and discussed by the WG.

_Sections in italics are commentary either by the author or from the original NIST document.  Author comments are tagged with his initials -dhs, all others are verbatim from the original document._

_N.B. I deleted sections that are not required by IPSIE, e.g. Section 2.4 FAL3. -dhs_

**SECTION 2 Federation Assurance Level (FAL)**

- In order to fulfill the requirements for a a given FAL, the federation transaction SHALL meet or exceed all requirements listed for that FAL.

**SECTION 2.1 Common FAL Requirements**
- At all FALs, all federation transactions SHALL comply with the requirements in Sec. 3 to deliver an assertion to the RP and create an authenticated session at the RP. 
  - **OIDC SL1:** To be verified below.
- At all FALs, the RP needs to trust the IdP to provide valid assertions representing the subscriber’s authentication event and SHALL validate the assertion.
  - **OIDC SL1:** To be verified below.
- IdPs and RPs SHALL employ appropriately tailored security controls from the moderate baseline security controls defined in [SP800-53] or an equivalent federal (e.g., [FEDRAMP]) or industry standard that the organization has determined for the information systems, applications, and online services that these guidelines are used to protect.
  - **OIDC SL1:** Not enforced through SL1. _IPSIE profiles should not enforce this control._
- IdPs and RPs SHALL ensure that the minimum assurance-related controls for the appropriate systems, or equivalent, are satisfied. Additional security controls are discussed in Sec. 3.10.
  - **OIDC SL1:** Not enforced through SL1. _IPSIE profiles should not enforce this control._

**SECTION 2.2 Federation Assurance Level 1 (FAL1)**
- At FAL1, the IdP SHALL sign the assertion using approved cryptography.
  - **OIDC SL1:** OIDC SL1 Draft §3.2 meets the requirement.   
- The RP SHALL validate the signature using the key associated with the expected IdP. The signature protects the integrity of the assertion contents and allows for the IdP to be verified as the source of the assertion.
  - **OIDC SL1:** Requires validation per OIDC §3.1.3.7 https://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation.
- All assertions at FAL1 SHALL be audience-restricted to a specific RP or set of RPs, and the RP SHALL validate that it is one of the targeted RPs for the given assertion.
  - **OIDC SL1:** OIDC SL1 Draft §3.3.1 meets the requirement.
- At FAL1, the trust agreement MAY be established by the subscriber during the federation transaction.
  - **OIDC SL1:** OIDC SL1 Draft §3.3.1 meets the requirement by requiring pre-registered clients/no dynamic registration.
- At FAL1, the federation protocol SHOULD apply injection protection as discussed in Sec. 3.10.1.
  - **OIDC SL1:** OIDC SL1 Draft uses the back channel as discussed in NIST800-63C 3.10.1 and 4.11.1 as sufficient protection.
- The federation transaction SHOULD be initiated by the RP.
  - **OIDC SL1:** OIDC SL1 Draft §3.3.2 requires support of third party initiated login (§4 of OIDC Core). 

**SECTION 2.3 Federation Assurance Level 2 (FAL2)**
- At FAL2, the assertion SHALL be strongly protected from injection attacks, as discussed in Sec. 3.10.1.
  - **OIDC SL1:** OIDC SL1 Draft uses the back channel as discussed in NIST800-63C 3.10.1 and 4.11.1 as sufficient protection.
- The federation transaction SHALL be initiated by the RP.
  - **OIDC SL1:** OIDC SL1 Draft §3.3.2 requires support of third party initiated login (§4 of OIDC Core). 
- At FAL2, the assertion SHALL audience restricted to a single RP.
  - **OIDC SL1:** OIDC SL1 Draft §3.3.1 meets the requirement.
- At FAL2, an a priori trust agreement SHALL be established prior to the federation transaction taking place.
  - **OIDC SL1:** OIDC SL1 Draft §3.3.1 meets the requirement by requiring pre-registered clients/no dynamic registration. 

**SECTION 2.4 Federation Assurance Level 3 (FAL3)**
_Skipped as irrelevant to IPSIE SL1 -dhs_

**SECTION 2.5 Requesting and Processing xALs**
- IdPs SHALL support a mechanism for RPs to specify a set of minimum acceptable xALs as part of the trust agreement and SHOULD support the RP specifying a more strict minimum set at runtime as part of the federation transaction.
  - **OIDC SL1:** This is implicit in IPSIE for AAL and FAL. IPSIE SL1 requires multifactor authentication, meeting AAL2.  IPSIE compliant services must meet FAL2.  IAL is not specified in IPSIE and no minimum is required.
- When an RP requests a particular xAL, the IdP SHOULD fulfill that request, if possible, and SHALL indicate the resulting xAL in the assertion.
  - **OIDC SL1:** IPSIE does not require IdPs to meet this requirement.
- The IdP SHALL inform the RP of the following information for each federation transaction:
  - The IAL of the subscriber account being presented to the RP, or an indication that no IAL claim is being made
    - **OIDC SL1:** TBD
  - The AAL of the currently active session of the subscriber at the IdP, or an indication that no AAL claim is being made
    - **OIDC SL1:** IPSIE does not require the AAL to be transmitted directly, however, it can be inferred as AAL2 when using IPSIE compliant services.  The AAL may be derived from the `amr` and `acr` claims, as well.
  - The FAL of the federation transaction
    - **OIDC SL1:** IPSIE compliant services are FAL2 compliant by default. This may be inferred from the trust agreement.
- The RP gets this xAL information from a combination of the terms of the trust agreement as described in Sec. 3.4 and information included in the assertion as described in Sec. 4.9 and Sec. 5.8. If the xAL is unchanging for all messages between the IdP and RP, the xAL information SHALL be included in the terms of the trust agreement between the IdP and RP.
  - **OIDC SL1:** Trust agreements, beyond preregistration, are unlikely to be enforced by IPSIE profiles.
- If the xAL could be within a range of possible values specified by the trust agreement, the xAL information SHALL be included as part of the assertion contents.
  - **OIDC SL1:** This is unlikely to be enforced by IPSIE.
- The IdP MAY indicate that no claim is made to the IAL or AAL for a given federation transaction. In such cases, no default value is assigned to the resulting xAL by the RP. That is to say, a federation transaction without an IAL declaration in either the trust agreement or the assertion is functionally considered to have “no IAL” and the RP cannot assume the account meets “IAL1”, the lowest numbered IAL described in this suite.
  - **OIDC SL1:** This is unlikely to be enforced by IPSIE.
- The RP SHALL determine the minimum IAL, AAL, and FAL it is willing to accept for access to any offered functionality.
  - **OIDC SL1:** This is unlikely to be enforced by IPSIE.
- An RP MAY vary its functionality based on the IAL, AAL, and FAL of a specific federated authentication.
  - **OIDC SL1:** IPSIE does not constrain the RP's functionality.
- The RP SHALL ensure that it meets its obligations in the federation transaction for the FAL declared in the assertion. For example, the RP needs to ensure the presentation method meets the injection protection requirements at FAL2 and above, and that the appropriate bound authenticator is presented at FAL3.
  - **OIDC SL1:** If all components meet IPSIE SL1 or above, this requirement is met.

**Section 3 Common Federation Requirements**

**Section 3.2 Proxied Federation**
_TBD - I don't believe we have discussed this pattern in IPSIE SL1. -dhs_

- A federation proxy acts as an intermediary between the IdP and RP for all communication in the federation protocol. The proxy functions as an RP on the upstream side and an IdP on the downstream side [...] the upstream IdP and downstream RP communicate with the proxy using a standard federation protocol, and the subscriber takes part in two separate federation transactions. As a consequence, all normative requirements that apply to IdPs and RPs SHALL apply to proxies in their respective roles on each side. 
- The federated identifier (see Sec. 3.3) of an assertion from a proxy SHALL indicate the proxy as the issuer of the assertion. The downstream RP receives and validates the assertion generated by the proxy, as it would an assertion from any other IdP. This assertion is based on the assertion the proxy receives from the upstream IdP. 
- The FAL of the connection between the proxy and the downstream RP is considered as the lowest FAL along the entire path, and the proxy SHALL accurately represent this to the downstream RP.
  
**Section 3.3 Federated Identifiers**
- The subscriber SHALL be identified in the federation transaction using a federated identifier unique to that subscriber. A federated identifier is the logical combination of a subject identifier, representing a subscriber account, and an issuer identifier, representing the IdP. The subject identifier is assigned by the IdP, and the issuer identifier is assigned to the IdP usually through configuration.
  - **OIDC SL1:** `sub` claim of the ID token meets this requirement.
-Federated identifiers SHALL contain no plaintext personally-identifiable information (PII), such as usernames, email addresses, or employee numbers, etc.
  - **OIDC SL1:** `sub` claim of the ID token meets this requirement.

**Section 3.3.1 Pairwise Pseudonymous Identifiers (PPI)**

_TBD - I don't believe we have discussed this pattern in IPSIE SL1.  Given the context of IPSIE usage in an enterprise context, I do not think this should be included in our IPSIE requirements. -dhs_
_ Added an [issue](https://github.com/openid/ipsie/issues/75) to force discussion._


- When using pairwise pseudonymous identifiers within the assertions generated by the IdP for the RP, the IdP SHALL generate a different federated identifier for each RP as described in Sec. 3.3.1.2 or set of RPs as described in Sec. 3.3.1.3.
- Where PPIs are used alongside identifying attributes, privacy policies SHALL be established to prevent correlation of subscriber data consistent with applicable legal and regulatory requirements.
- Note that in a proxied federation model (see Sec. 3.2.3), the upstream IdP may be unable to generate a PPI for the downstream RP, since the proxy could blind the IdP from knowing which RP is being accessed by the subscriber. In such situations, the PPI is generally established between the IdP and the federation proxy. The proxy, acting as an IdP, can provide a PPI to the downstream RP. Depending on the protocol, the federation proxy may need to map the PPI back to the associated identifiers from upstream IdPs in order to allow the identity protocol to function. In such cases, the proxy will be able to track and determine which PPIs represent the same subscriber at different RPs. The proxy SHALL NOT disclose the mapping between the PPI and any other identifiers to a third party or use the information for any purpose other than those allowed for transmission of subscriber information defined in Sec. 3.9.1.
- The PPI SHALL contain no identifying information about the subscriber (e.g., username, email address, employee number, etc.).
- The PPI SHALL be difficult to guess by a party having access to information about the subscriber, having at least 112 bits of entropy as stated in [SP800-131A]. PPIs can be generated randomly and assigned to subscribers by the IdP or could be derived from other subscriber information if the derivation is done in an irreversible, unguessable manner (e.g., using a keyed hash function with a secret key as discussed in [SP800-131A]).
- Unless the PPI is designated as shared by the trust agreement, the PPI SHALL be disclosed to only a single RP.
- The same shared PPI SHALL be used for a specific set of RPs if all the following criteria are met:
  - The trust agreement stipulates a shared PPI for a specific set of RPs;
  - The authorized party consents to and is notified of the use of a shared PPI;
  - Those RPs have a demonstrable relationship that justifies an operational need for the correlation, such as a shared security domain or shared legal ownership; and
  - All RPs in the set of a shared PPI consent to being correlated in such a manner (i.e., one RP cannot request to have another RP’s PPI without that other RP’s knowledge and consent).
- The RPs SHALL conduct a privacy risk assessment to consider the privacy risks associated with requesting a shared PPI. See Sec. 7.2 for further privacy considerations.
- The IdP SHALL ensure that only intended RPs are included in the set; otherwise, a rogue RP could learn of the shared PPI for a set of RPs by fraudulently posing as part of that set.

**Section 3.4 Trust Agreements**

_ The current SL1 profile explictly requires pre-registration, does not support dynamic registration.  Enforcing the use of a trust agreement beyond preregistration should be out of scope for IPSIE.  If we decide this is out of scope, all of section 3.4 can be ignored. -dhs_
_Opened a new [issue](https://github.com/openid/ipsie/issues/76) to further the discussion_


- All federation transactions SHALL be defined by one or more trust agreements between the applicable parties.
- The trust agreement SHALL establish a trust relationship between the RP and:
  - The CSP responsible for provisioning and managing the subscriber account,
  - The IdP responsible for providing assertions and attributes, or
  - Both the CSP and IdP.
- Trust agreements establish the terms for federation transactions between the parties they affect, including things like the allowed xALs and the intended purposes of identity attributes exchanged in the federation transaction. The trust agreement SHALL establish usability and equity requirements for the federation transaction.
- The trust agreement SHALL disclose details of the proofing process used at the CSP, including any compensating controls and exception handling processes.
- All trust agreements SHALL define a specific population of subscriber accounts that the agreement is applicable to. The exact means of defining this population are out of scope of this document. In many cases, the population is defined as the full set of subscriber accounts that the CSP manages and makes available through an IdP. In other cases, the population is a demarcated subset of accounts available through an IdP. It is also possible for an RP to have a distinct trust agreement established with an IdP for a single subscriber account, such as in a subscriber-driven trust agreement.
- During the course of a single federation transaction, it is important for the policies and expectations of all parties be unambiguous for all parties involved. Therefore, there SHOULD be only one set of trust agreements in effect for a given transaction. This will usually be determined by the unique combination of CSP, IdP, and RP participating in the transaction. However, these agreements could vary in other ways, such as different populations of subscribers being governed by different trust agreements.
- Trust agreements SHALL establish terms regarding expected and acceptable IALs and AALs in connection with the federated relationship.
- Trust agreements SHALL define necessary mechanisms and materials to coordinate redress and issues between the different participants in the federation, as discussed in Sec. 3.4.3.
- (_N.B. not a requirement, however, this appears relevant to IPSIE -dhs_) Establishment of a trust agreement is required for all federation transactions, even those in which the roles and applications exist within a single security domain or shared legal ownership. In such cases, the establishment of the trust agreement can be an internal process and does not need to involve a formal agreement. Even in such cases, it is still required for the IdP to document and disclose the trust agreement to the subscriber upon request.

**Section 3.4.2 Multilateral Trust Agreements**
- The trust agreement SHALL enumerate the required practices for vetting all parties, and SHALL indicate the party or parties responsible for performing the vetting process.
  - Vetting of CSPs, IdPs, and RPs SHALL establish, as a minimum, that:
    - CSPs are performing identity proofing of subscriber accounts in accordance with [SP800-63A]
    - CSPs onboard subscriber accounts to IdPs in a secure fashion in adherence to the requirements in Sec. 4.1 or Sec. 5.4 as applicable
    - Authenticators used for authenticating the subscriber at the IdP or onboarding a subscriber-controlled wallet are used in accordance with [SP800-63B]
    - Assertions generated by IdPs adhere to the requirements in Sec. 4.9 or Sec. 5.8.
    - RPs adhere to requirements for handling subscriber attribute data, such as retention, aggregation, and disclosure to third parties.
    - RP and IdP systems use approved profiles of federation protocols.
- The federation authority MAY provide a programmatic means for parties under the trust agreement to verify membership of other parties under the trust agreement. For example, a federation authority could provide a discovery API that provides the vetted capabilities of an IdP for providing identities to RPs within the system, or it could provide a signed attestation for RPs to present to IdPs during a registration step.
- Federation authorities SHALL periodically re-evaluate members for compliance, in terms disclosed in the trust agreement.
- When information needs to be shared between CSPs, such as during suspicion of fraud on a subscriber account, the federation authority can define the policies that apply for the transfer of this information. While sharing information in this way can be used to mitigate fraud, there are also substantial privacy concerns. The federation authority SHALL include all information sharing between parties other than for identity purposes in its privacy risk assessment.
- A federation authority MAY incorporate other multilateral trust agreements managed by other federation authorities in its trust agreement, creating an interfederation agreement. For example, IdP1 has been vetted under a multilateral agreement with FA1, and RP2 has been vetted under a multilateral agreement with FA2. In order to facilitate connection between IdP1 and RP2, a new federation authority FA3 can provide a multilateral agreement that accepts IdPs from FA1 and RPs from FA2. If IdP1 and RP2 accept the authority of FA3, the federation connection can continue under the auspices of this interfederation agreement.

**Section 3.4.3 Redress Requirements**
- As the recipient of a subscriber’s identity attributes, the RP is the subscriber’s primary view into the federated system, and in some instances the subscriber may be unaware that an IdP is involved with their use of the RP. Therefore it falls to the RP to provide the subscriber with a clear and accessible method of contacting the RP to request redress. For matters that involve the RP subscriber account (including any attributes stored in the account), RP functionality, bound authenticators, RP allowlists, and other items under the RP’s control, the RP SHALL provide clear and accessible means of redress to the subscriber.
- For matters that involve the IdP or CSP, the RP SHALL provide the subscriber with a means of initiating the redress process with the IdP or CSP, as appropriate.
- For matters involving the use of the subscriber account in federation transactions, including attribute values and derived attribute values made available over federation transactions, IdP functionality, holder-of-key authenticators, IdP allowlists, and other items in the IdP’s control, the IdP SHALL provide clear and accessible means of redress to the subscriber.
- For matters that also involve a particular RP, the IdP SHALL provide the subscriber with a means of initiating the redress process with the RP.
- For matters involving the subscriber account that has been made available to the IdP, the IdP SHALL provide the subscriber with a means of initiating the redress process with the CSP.
- For matters involving the subscriber account, including identity attributes and authenticators in the subscriber account, the CSP SHALL provide the subscriber with a clear and accessible means of redress.

**Section 3.5 Identifiers and Cryptographic Key Management for CSPs, IdPs, and RPs**
- The discovery and registration processes SHALL be established in a secure fashion as defined by the trust agreement governing the transaction. Protocols requiring the transfer of cryptographic key information SHALL use an authenticated protected channel to exchange cryptographic key information needed to operate the federated relationship, including any shared secrets or public keys.
  - **OIDC SL1:** In the absence of trust agreements, IPSIE should enforce this requirement.  This is not enforced today in the OIDC SL1 profile and should be added to the section on cryptography and secrets §3.2.
- Any symmetric keys used in this relationship SHALL be unique to a pair of federation participants.
  - **OIDC SL1:** 
- CSPs, IdPs (including subscriber-controlled wallets), and RPs MAY have multiple cryptographic keys and identifiers to serve different purposes within a trust agreement, or to serve different trust agreements. For example, an IdP could use one set of assertion signing keys for all FAL1 and FAL2 transactions, but use a separately managed set of cryptographic keys for FAL3 transactions, stored in a higher security container.
  - **OIDC SL1:** Unclear if this adds value to IPSIE SL1. Since this is optional, it has no impact on IPSIE.
- When domain names, URIs, or other structured identifiers are used to identify parties, wildcards SHALL NOT be used. For example, if an RP is deployed at “www.example.com”, “service.example.com”, and “gateway.example.com”, then each of these identifiers would have to be registered for the RP. A wildcard of “*.example.com” cannot be used, as it would unintentionally allow access to “user.example.com” and “unknown.example.com” under the same RP identifier.
  - **OIDC SL1:** TBD

** Section 3.5.1 Cryptographic Key Rotation**
- Over time, it can be desirable or necessary to update the cryptographic key associated with a CSP, IdP, or RP. The allowable update process for any cryptographic keys and identifiers SHALL be defined by the trust agreement and SHALL be executed using an authenticated protected channel, as in the initial cryptographic key establishment.
  - **OIDC SL1:** In the absence of trust agreements, IPSIE should enforce a requirement for secure key establishment in the section on cryptography and secrets §3.2.

**Section 3.5.2 Cryptographic Key Storage**
- CSPs, IdPs (including subscriber-controlled wallets), and RPs SHALL store all private and shared keys used for signing, encryption, and any other cryptographic operations in a secure fashion. Key storage is subject to applicable [FIPS140] requirements of the FAL at which the key is being used, including applicable tamper resistance requirements.
  - **OIDC SL1:** It is unlikely that IPSIE can eforce such a requirement.
    
**Section 3.5.3 Software Attestations**
- When attestations are required by the trust agreement or requested as part of the federation protocol, received attestations SHALL be validated by the receiver.
  - See [RFC7591] Sec. 2.3 for more information about software statements, which are a means for OAuth and OpenID Connect RPs to communicate a signed set of software attributes during dynamic client registration.
  - **OIDC SL1:** Dynamic client registration is not allowed in IPSIE SL1.

**Section 3.6 Authentication and Attribute Disclosure** 
_For use in an enterprise context, these controls should not be enforced by IPSIE.  Enterprises and enterprise services should be aware of local laws and regulations that create data handling requirements for the subscriber's attributes. -dhs_
_Opened https://github.com/openid/ipsie/issues/77 for discussion_

- A subscriber’s attributes SHALL be transmitted between IdP and RP only for federation transactions or support functions such as identification of compromised subscriber accounts as discussed in Sec. 3.9. 
- A subscriber’s attributes SHALL NOT be transmitted for any other purposes, even when parties are allowlisted.
- A subscriber’s attributes SHALL NOT be used by the RP for purposes other than those stipulated in the trust agreement.
- A subscribers attributes SHALL be stored and managed in accordance with Sec. 3.10.3.
- The subscriber SHALL be informed of the transmission of attributes to an RP.
- In the case where the authorized party is the organization, the organization SHALL make available to the subscriber the list of approved RPs and the associated sets of attributes sent to those RPs.
- In the case where the authorized party is the subscriber, the subscriber SHALL be prompted prior to release of attributes using a runtime decision at the IdP as described in Sec. 4.6.1.3.
  - **OIDC SL1:** Not applicable.  The profile does not define the use of a wallet to present the attributes. 

**Section 3.7 RP Subscriber Account**
- An RP subscriber account is provisioned when the RP has associated a set of attributes about the subscriber with a data record representing the subscriber account at the RP. The RP subscriber account SHALL be bound to at least one federated identifier, and a given federated identifier is bound to only one RP subscriber account at a given RP. The provisioning can happen prior to authentication or as a result of the federated authentication process, depending on the deployment patterns as discussed in Sec. 4.6.3. Prior to being provisioned, the RP subscriber account does not exist and has no associated data record at the RP.
  - **OIDC SL1:** At SL1 JIT provisioning is allowed if the appropriate reconciliation via the IL* mechanisms is in place to ensure the account is managed.  Although not explicitly called out in SL1, it is not explicitly banned. 
- An RP subscriber account is terminated when the RP removes all access to the account at the RP. Termination SHALL include removal of all federated identifiers and bound authenticators from the RP subscriber account (to prevent future federation transactions) as well as removal of attributes and information associated with the account in accordance with Sec. 3.10.3.
  - **OIDC SL1:** Sec. 3.10.3 allows this to be controlled by policy.  IPSIE makes no statements about data removal from the RP.
- An RP MAY terminate an RP subscriber account independently from the IdP for a variety of reasons, regardless of the current validity of the subscriber account from which it is derived.
  - **OIDC SL1:** IPSIE makes no statements about RP termination of a user account.

**Section 3.7.1 Account Linking** 

_In the enterprise context, this is an antipattern that would allow the user to potentially avoid enterprise control.  For example, if the user has federated from their personal google account and their enterprise IdP, the authentication controls on the google account may not meet those on the enterprise account.  Further, this could create a situation where the user retains access through a personal account after ending employment.  I suggest this is explicitly disallowed within IPSIE. -dhs_
_added https://github.com/openid/ipsie/issues/78 for discussion_

- A single RP subscriber account MAY be associated with more than one federated identifier. This practice is sometimes known as account linking.
- If the RP allows a subscriber to manage multiple accounts in this way, the RP SHALL require an authenticated session with the subscriber account for all management and linking functions.
- This authenticated session SHOULD require one existing federated identifier before linking the new federated identifier to the RP subscriber account.
- An RP MAY offer a means of recovery of an RP subscriber account with no current means of access.
- When a federated identifier is removed from an RP subscriber account, the RP SHALL disallow access to the RP subscriber account from the removed federated identifier.
- The RP SHALL document its practices and policies that it enacts when an RP subscriber account reaches a state of having zero associated federated identifiers, no means of access, and no means of recovery. In such cases, the RP subscriber account SHOULD be terminated and information associated with the account in accordance with Sec. 3.10.3.
- The RP SHALL provide notice to the subscriber when:
  - A new federated identifier is added to an existing RP subscriber account
  - A federated identifier is removed from an RP subscriber account, but the account is not terminated
- The RP MAY associate different access rights to the same account depending on which federated account is used to access the RP. The means by which an RP determines authorization and access is out of scope of these guidelines.

**Section 3.7.2 Account Resolution**

_Account resolution is unlikely to be used in the enterprise context. I recommend that this is not applicable to IPSIE. -dhs_
_ See https://github.com/openid/ipsie/issues/79_

- An RP performing account resolution SHALL ensure that the attributes requested from the IdP are sufficient to uniquely resolve the subscriber within the RP’s system before linking the federated identifier with the RP subscriber account and granting access. The intended use of each attribute by the RP is detailed in the trust agreement, including whether the attribute is used for account resolution in this manner.
- An RP performing account resolution SHALL perform a risk assessment to ensure that the resolution process does not associate an RP subscriber account’s information with a federated identifier not belonging to the subscriber.
- A similar account resolution process is also used when the RP verifies an authenticator used in a holder-of-key assertion for the first time. In this case, the RP SHALL ensure that the attributes carried with the authenticator uniquely resolve to the subscriber.
  - **OIDC SL1:** not applicable, holder-of-key is only relevant at FAL3.

**Section 3.7.3 Alternative Authentication Processes**

_IPSIE has not accounted for any alternative processes, however, this has created an issue with breakglass accounts.  We need further discussion on how to manage break glass accounts for applications in IPSIE. -dhs_
_See https://github.com/openid/ipsie/issues/80_

- The RP MAY allow a subscriber to access their RP subscriber account using direct authentication processes by allowing the subscriber to add and remove authenticators in the RP subscriber account.
- The RP SHALL follow the requirements in [SP800-63B] in managing all alternative authenticators.
- If the RP allows this kind of access, the RP SHALL disclose in the trust agreement:
  - The process for adding and removing alternative authenticators in the RP subscriber account
  - Any restrictions on authenticators the subscriber can use to access the RP
  - The AAL required for access to the subscriber account without a federation transaction
  - The circumstances under which the RP will require the subscriber to authenticate with their IdP, such as a period of time since last federation transaction

**Section 3.8 Authenticated Sessions at the RP** 
- The end goal of a federation transaction is creating an authenticated session between the subscriber and the RP, backed by a verified assertion from the IdP. This authenticated session can be used to allow the subscriber access to functions at the RP (i.e., logging in), identifying the subscriber to the RP, or processing attributes about the subscriber carried in the federation transaction. An authenticated session SHALL be created by the RP only when the following conditions are true:
  - The RP has processed and verified a valid assertion
  - The assertion is from the expected IdP for a transaction
  - The IdP that issued the assertion is the IdP identified in the federated identifier of the assertion
  - The assertion is associated with an RP subscriber account (which may be ephemeral)
  - The RP subscriber account has been provisioned at the RP through the method specified in the trust agreement
- The authenticated session MAY be ended by the RP at any time.
  - **OIDC SL1:** Meets all of these requirements except regarding the trust agreement.
  
**Section 3.9 Privacy Requirements**

_These requirements are unlikely to apply in the enterprise context.  See https://github.com/openid/ipsie/issues/81_

- If the same subscriber account is asserted to multiple RPs, and those RPs communicate with each other, the colluding RPs could track a subscriber’s activity across multiple applications and security domains. The IdP SHOULD employ technical measures, such as the use of pairwise pseudonymous identifiers described in Sec. 3.3.1 or privacy-enhancing cryptographic protocols, to provide disassociability and discourage subscriber activity tracking and profiling between RPs.
- If the RP subscriber account lifecycle process gives the RP access to attributes through a provisioning API as discussed in Sec. 4.6.3, additional privacy measures SHALL be implemented to account for the difference in RP subscriber account lifecycle.
  - The IdP SHALL minimize the attributes made available to the RP through the provisioning API.
  - The IdP SHALL limit the population of subscriber accounts available via the provisioning API to the population of subscribers authorized to use the RP by the trust agreement.
  - To prevent RP retention of identity attributes for accounts that have been terminated at the IdP, the IdP SHALL use the provisioning API to de-provision RP subscriber accounts for terminated subscriber accounts.
- Trust agreements SHOULD require identity attributes be shared only when the subscriber opts in, using a runtime decision as discussed in Sec. 4.6.1.3.

**Section 3.9.1 Transmitting Subscriber Information**
- The IdP SHALL limit transmission of subscriber information to only that which is necessary for functioning of the system. These functions include the following:
 - identity proofing, authentication, or attribute assertions (collectively “identity service”); or
 - in the case of a specific subscriber request to transmit the information
- The IdP MAY additionally transmit the subscriber’s information in the following cases, if stipulated and disclosed by the trust agreement:
  - fraud mitigation related to the identity service,
  - to respond to a security incident related to the identity service, or
  - to comply with law or legal process.
- If an IdP discloses information on subscriber activities at an RP to any party, or processes the subscriber’s attributes for any purpose other than these cases, the IdP SHALL implement measures to maintain predictability and manageability commensurate with the privacy risk arising from the additional processing.
  - Measures MAY include providing clear notice, obtaining subscriber consent, or enabling selective use or disclosure of attributes.
  - When an IdP uses consent measures for this purpose, the IdP SHALL NOT make consent for the additional processing a condition of the identity service.
- An RP MAY disclose information on subscriber activities to the associated IdP in the following cases, if stipulated and disclosed by the trust agreement:
 - fraud mitigation related to the identity service,
 - to respond to a security incident related to the identity service, or
 - to comply with law or legal process.

**Section 3.10 Security Controls** 

_IPSIE should not prescribe alignment with SP800-53 or FEDRAMP.  -dhs_
_See https://github.com/openid/ipsie/issues/82_

- The IdP and RP SHALL employ appropriately tailored security controls from the moderate baseline security controls defined in [SP800-53] or equivalent federal (e.g., [FEDRAMP]) or industry standard that the organization has determined for the information systems, applications, and online services that these guidelines are used to protect. 
- The IdP and RP SHALL ensure that the minimum assurance-related controls for the appropriate systems, or equivalent, are satisfied.

**Section 3.10.1 Protection from Injection Attacks**
_N.B. this section does not have directives, however, I have included the following items to track injection protection at FAL2. -dhs_

- Protection from injection attacks is recommended at all FALs, and this protection is required at FAL2 and above. In all cases, the RP needs to take reasonable steps to prevent an attacker from presenting an injected assertion or assertion reference based on the nature of the RP software, the capabilities of the federation protocol in use, and the needs of the overall system. Both [OIDC] and [SAML] provide mechanisms for injection protection including nonces sent from the RP during the request, RP authentication for back-channel communications, and methods for the RP to start the federation transaction and track its state throughout the process. Different mechanisms provide different degrees of protection and are applicable in different circumstances. While the details of specific protections will vary based on the federation protocol and technology in use, common best practices such as the following can be used to limit the attack surface:
  - The use of back channel assertion presentation as discussed in Sec. 4.11.1, which prevents an attacker from presenting the assertion directly to the RP.
  - The use of an unguessable value to tie the unauthenticated session at the RP with the request to the back channel, which prevents an attacker from injecting an assertion reference from one session to another.
  - Requiring the RP to authenticate to the IdP during an assertion request, preventing the attacker from faking a request from the RP to begin a federation process.
  - Prohibition of IdP-initiated federation processes, which prevent the RP from accepting unsolicited assertions and assertion references from the IdP. This prohibition does not include processes in which an external party (such as the IdP or a federation authority) signals the RP to start a federation process with the IdP, allowing the RP to begin the federation transaction and securely await a response within that transaction.
  - The use of a signed front channel response from the IdP with an RP-provided nonce covered by the signature, which prevents the attacker from injecting an assertion reference from one session to another.
  - The use of platform APIs for front-channel communication, as opposed to HTTP redirects.

**Section 3.10.2 Protecting Subscriber Information**
- Communications between the IdP and the RP SHALL be protected in transit using an authenticated protected channel.
  - **OIDC SL1:** Meets this requirement.
- Communications between the subscriber and either the IdP or the RP (usually through a user agent) SHALL be made using an authenticated protected channel.
  - **OIDC SL1:** N/A 
- Additional attributes about the user MAY be included outside of the assertion itself by use of authorized access to an identity API as discussed in Sec. 3.11.3. Splitting user information in this manner can aid in protecting user privacy and can allow for limited disclosure of identifying attributes on top of the essential information in the authentication assertion itself.
  - **OIDC SL1:** Meets this requirement.
- When derived attribute values are available and fulfill the RP’s needs, the RP SHOULD request derived attribute values rather than full attribute values as described in Sec. 7.3.
  - The IdP SHOULD support derived attribute values to the extent the underlying federation protocol allows.
  - **OIDC SL1:** IPSIE does not include any statements regarding this requirement.
    
**Section 3.10.3 Storing Subscriber Information**

_As with the statements above regarding deprovisioning, these should be controlled by the policy of the enterprise. -dhs_
_See https://github.com/openid/ipsie/issues/83_

- The IdP and RP SHALL delete personal identity information in the subscriber account and RP subscriber account (respectively) upon account termination, unless required otherwise by legal action or policy.
- Whenever personal identity information is stored in a subscriber account or RP subscriber account, whether the account is active or not, the IdP and RP SHALL determine and use appropriate controls to ensure secure storage of the personal identity information.
- When the RP uses an ephemeral provisioning mechanism as described in Sec. 4.6.3, the RP SHALL remove all subscriber attributes at the termination of the session, unless required by legal action or policy.

**Section 3.11 Identity Attributes**
- Bundling:
  - Attributes SHALL be either unbundled (presented directly by the IdP) or bundled into a package that is cryptographically signed by the CSP, as described in Sec. 3.11.1.
    - **OIDC SL1:** Meets this requirement.
- Derivation:
  - Attributes SHALL be either attribute values (e.g., a date of birth) or derived attribute values (e.g., an indication of age of majority).
    - **OIDC SL1:** IPSIE makes no statements about derived attributes.
- Presentation:
  - Attributes SHALL be either presented in the assertion (and therefore covered by the assertion’s signature) or made available as part of a protected identity API.
    - **OIDC SL1:** Meets this requirement via the userinfo endpoint.
- Trust agreements SHALL record the validation practices for all attributes made available under the trust agreement (e.g., whether the attribute is from an authoritative or credible source, self-asserted by the subscriber, assigned by the IdP, etc.).
  - **OIDC SL1:** Makes no statement about the validation practices nor are trust agreements likely in scope.

**Section 3.11.1 Attribute Bundles**
_N.B. I do not believe this applies to IPSIE SL1.  I have left this intact until the WG has a chance to discuss. -dhs_
_see https://github.com/openid/ipsie/issues/84_

**_Note: Attribute bundles are often referred to elsewhere as credentials by other protocols and specifications, but usage of this term would be in conflict with its use within these guidelines for a different concept. Consequently, the term attribute bundle is used within these guidelines instead._**

- The presentation of an attribute bundle SHALL be protected by the IdP in the same manner as non-bundled attributes. That is to say, attribute bundles presented in an assertion are covered by the signature of the assertion, and attribute bundles made available by an identity API are protected by the limited access controls to that API.
- Attribute bundles include one or more attribute values and derived attribute values. Attribute bundles are carried in the assertion from the IdP, the subscriber attributes within the bundle need not be fully disclosed to all RPs on every transaction and instead MAY be selectively disclosed to the RP. An attribute bundle using selective disclosure technology can effectively limit which attributes an RP can read from the attribute bundle. The RP can still verify the signature of the attribute bundle as a whole, confirming its source as the CSP, without the IdP having to disclose all of the contents of the attribute bundle to the RP.
- The RP SHALL validate the signature covering the attribute bundle itself as well as the signature of the assertion as a whole.
- The RP SHALL ensure that the attribute bundle is able to be presented by the IdP that created the assertion containing the attribute bundle, such as by verifying that the public key used to sign the assertion is included in the signature of the attribute bundle.

**Section 3.11.3 Identity APIs**
- Attributes about the subscriber, including profile information, MAY be provided to the RP through a protected API known as the identity API. The RP is granted limited access to the identity API during the federation transaction, in concert with the assertion. For example, in OpenID Connect, the UserInfo Endpoint provides a standardized identity API for fetching attributes about the subscriber. This API is protected by an OAuth 2.0 Access Token, which is issued to the RP along with OpenID Connect’s assertion, the ID Token.
  - **OIDC SL1:** Meets this requirement.
- All possible use of identity APIs, including which provisioning models are available through the API, SHALL be recorded and disclosed as part of the trust agreement.
  - **OIDC SL1:** Trust agreements are unlikely to be included in IPSIE.
- Access to the identity API SHALL be time limited by the trust agreement.
- **OIDC SL1:** Trust agreements are unlikely to be included in IPSIE.
- Access to the identity API SHOULD be limited to the duration of the federation transaction plus time necessary for synchronization of attributes, as discussed in Sec. 4.6.4.
  - **OIDC SL1:** Meets this requirement.
- Since the time limitation is separate from the validity time window of the assertion and the lifetime of the authenticated session at the RP, access to an identity API by the RP without an associated valid assertion SHALL NOT be sufficient for the establishment of an authenticated session at the RP.
  - **OIDC SL1:** See https://github.com/openid/ipsie/issues/74.
- A given identity API deployment is expected to be capable of providing attributes for all subscribers for whom the IdP can create assertions. However, when access to the identity API is granted within the context of a federation transaction, the attributes provided by an identity API SHALL be associated with only the single subscriber identified in the associated assertion.
  - **OIDC SL1:** Meets this requirement.
- If the identity API is hosted by the IdP, the returned attributes SHALL include the subject identifier for the subscriber. This allows the RP to positively correlate the assertion’s subject to the returned attributes.
  - **OIDC SL1:** Meets this requirement.
- Note that when access to an identity API is provided as part of pre-provisioning of RP subscriber accounts as discussed in Sec. 4.6.3, the RP is usually granted blanket access to the identity API outside the context of the federation transaction and these requirements do not apply. For pre-provisioning use cases, the privacy considerations SHALL be evaluated and recorded as part of the trust agreement. If the identity API is hosted externally, the requirements in Sec. 3.11.3.1 apply.
  - **OIDC SL1:** Unlikely to have any trust agreements in IPSIE.  In this case, the identity API is via SCIM for provisioning, and not OIDC userinfo endpoint.

**Section 3.11.3.1 External Identity APIs**
- The attributes returned by the attribute provider are assumed to be independent of those returned directly from the IdP, and as such MAY use different identifiers, formats, or schemas.
- Before accepting attributes from an external identity provider and associating them with the RP subscriber account, the RP SHALL verify that the attributes in question are allowed to be provided by the external attribute provider under the auspices of the trust agreement.

**Section 3.12 Assertion Protection**
- Assertions SHALL include a set of protections to prevent attackers from manufacturing valid assertions or reusing captured assertions at disparate RPs. The protections required are dependent on the details of the use case being considered, and specific protections are listed here.

**Section 3.12.1 Assertion Identifier**
- Assertions SHALL be sufficiently unique to permit unique identification by the target RP.
- Assertions MAY accomplish this by use of an embedded nonce, issuance timestamp, assertion identifier, or a combination of these or other techniques.

**Section 3.12.2 Signed Assertion** 
- Assertions SHALL be cryptographically signed by the issuer (IdP).
- The RP SHALL validate the digital signature or MAC of each such assertion based on the issuer’s key.
- This signature SHALL cover the entire assertion, including its identifier, issuer, audience, subject, and expiration.
- The assertion signature SHALL either be a digital signature using asymmetric keys or a MAC using a symmetric key shared between the RP and issuer.
- Shared symmetric keys used for this purpose by the IdP SHALL be independent for each RP to which they send assertions, and are normally established during registration of the RP.
- Public keys for verifying digital signatures SHALL be transferred to the RP in a secure manner, and MAY be fetched by the RP in a secure fashion at runtime, such as through an HTTPS URL hosted by the IdP.
- Approved cryptography SHALL be used.

**Section 3.12.3 Encrypted Assertion**
- The contents of the assertion can be encrypted to protect their exposure to untrusted third parties, such as a user agent. This protection is especially relevant when the assertion contains PII of the subscriber—excluding opaque identifiers such as the subject identifier. Subject identifiers are meaningless outside of their target systems, unlike other possible identifiers such as SSN, email address, or driver’s license number. Therefore, subject identifiers are excluded as a qualifier for encrypting the assertion. A trust agreement MAY require encryption of assertion contents in other situations.
- When encrypting assertions, the IdP SHALL encrypt the contents of the assertion using either the RP’s public key or a shared symmetric key.
  - Shared symmetric keys used for this purpose by the IdP SHALL be independent for each RP to which they send assertions, and are normally established during registration of the RP.
  - Public keys for encryption SHALL be transferred over an authenticated protected channel and MAY be fetched by the IdP at runtime, such as through an HTTPS URL hosted by the RP.
- All encryption of assertions SHALL use approved cryptography applied to the federation technology in use. For example, a SAML assertion can be encrypted using XML-Encryption, or an OpenID Connect ID Token can be encrypted using JSON Web Encryption (JWE). When used with back-channel presentation, an assertion can also be encrypted with a mutually-authenticated TLS connection, so long as there are no intermediaries between the IdP and RP that interrupt the TLS channel.

**Section 3.12.4 Audience Restriction** 
- Assertions SHALL use audience restriction techniques to allow an RP to recognize whether or not it is the intended target of an issued assertion.
- All RPs SHALL check that the audience of an assertion contains an identifier for their RP to prevent the injection and replay of an assertion generated for one RP at another RP.
- In order to limit the places that an assertion could successfully be replayed by an attacker, IdPs SHOULD issue assertions designated for only a single audience. Restriction to a single audience is required at FAL2 and above.


**Section 4 General-Purpose IdPs** 
_When the IdP is hosted on a service and not on the subscriber’s device, or when the IdP represents multiple subscribers, the IdP is known as a general-purpose IdP and the following requirements apply._

**Section 4.1 IdP Account Provisioning**
- In order to make subscriber accounts available through an IdP, the subscriber accounts need to be provisioned at the IdP. The means by which the subscriber account is provisioned to the IdP SHALL be disclosed in the trust agreement.
  
**Section 4.3 Trust Agreements**
- Trust agreements SHALL be established either:
  - as the result of an agreement by the federated parties, prior to the federation transaction, or
  - as the result of decision or action by the subscriber, during the federation transaction.

**Section 4.3.1 Apriori Trust Agreement Establishment**
- When the trust agreement is established by the federated parties prior to the federation transaction, the trust agreement SHALL establish the following terms, which MAY vary per IdP and RP relationship:
  - The set of subscriber attributes the CSP makes available to the IdP as part of the subscriber account
  - The set of subscriber attributes the IdP can make available to the RP
  - The attribute storage policy of the IdP for the subscriber account, including any available means for the subscriber to request deletion
  - Any additional attribute sources that the IdP receives applicable subscriber attributes from
  - What if any identity APIs are made available by the IdP, either directly or through an external provider, and which subscriber attributes are available at these APIs
  - The population of subscriber accounts that the IdP can create assertions for
  - Any additional uses of subscriber information, beyond providing the identity service
  - The set of subscriber attributes that the RP will request (a subset of the attributes made available)
  - The purpose for each attribute requested by the RP
  - The attribute storage policy of the RP for the RP subscriber account, including any available means for the subscriber to request deletion
  - The use of any shared signaling between the IdP and RP
  - The authorized party responsible for decisions regarding the release of subscriber attributes to the RP (e.g., the IdP organization, the subscriber, etc.)
  - The means of informing subscribers about attribute release to the RP
  - The xALs available from the IdP
  - The xALs required by the RP
  - The terms of the trust agreement SHALL be available to the operators of the RP and the IdP upon its establishment.
  - The terms of the trust agreement SHALL be made available to subscribers upon request to the IdP or RP.
- The IdP and RP SHALL each assess their respective redress mechanisms for their efficacy in achieving a resolution of complaints or problems and disclose the results of this assessment as part of the trust agreement. See Sec. 3.4.3 for additional requirements and considerations for redress mechanisms.
- Runtime decisions at the IdP, as described in Sec. 4.6.1.3, MAY be used to further limit which subscriber attributes are sent between parties in the federated authentication process (e.g., a runtime decision could opt to not disclose an email address even though this attribute was included in the terms of the trust agreement).
- The IdP and RP SHALL exchange only the minimum data necessary to achieve the function of the system.
- The trust agreement SHALL be reviewed periodically to ensure it is still fit for purpose, and to avoid unnecessary data exchange and over-collection of subscriber data.

**Section 4.3.2 Subscriber-driven Trust Agreement Establishment** 
- When the trust agreement is established as the result of a subscriber’s decision, such as a subscriber starting a federation transaction between an RP and their IdP who have no established agreement, the trust agreement is anchored by the subscriber. Consequently, the following terms SHALL be disclosed to the subscriber upon request to the IdP and to the RP during the runtime decision at the IdP as described in Sec. 4.6.1.3:
  - The set of subscriber attributes the CSP makes available to the IdP
  - Any additional attribute sources that the IdP receives applicable subscriber attributes from
  - What if any identity APIs are made available by the IdP, either directly or through an external provider, and which subscriber attributes are available at these APIs
  - The set of subscriber attributes the IdP can make available to the RP
  - The attribute storage policy of the IdP for the subscriber account, including any available means for the subscriber to request deletion
  - The use of any shared signaling between the IdP and RP
  - The population of subscriber accounts that the IdP can create assertions for
  - Any additional uses of subscriber information, beyond providing the identity service
  - The xALs available from the IdP
- The IdP SHALL assess its redress mechanisms for their efficacy in achieving a resolution of complaints or problems and disclose the results of this assessment to the subscriber. See Sec. 3.4.3 for additional requirements and considerations for redress mechanisms.
- The release of subscriber attributes SHALL be managed using a runtime decision at the IdP, as described in Sec. 4.6.1.3. The authorized party SHALL be the subscriber.
- The following terms of the trust agreement SHALL be disclosed to the subscriber during the runtime decision:
  - The set of subscriber attributes that the RP will request (a subset of the attributes made available by the IdP)
  - The purpose for each attribute requested by the RP
  - The attribute storage policy of the RP for the RP subscriber account, including any available means for the subscriber to request deletion
  - The xALs required by the RP
  - Note that all information disclosed to the subscriber needs to be conveyed in a manner that is understandable and actionable, as discussed in Sec. 8.

**Section 4.4 Discovery and Registration **
- To perform a federation transaction with a general-purpose IdP, the RP SHALL associate the assertion signing keys and other relevant configuration information with the IdP’s identifier, as stipulated by the trust agreement.
  - If these are retrieved over a network connection, request and retrieval SHALL be made over a secure protected channel from a location associated with the IdP’s identifier by the trust agreement. In many federation protocols, this is accomplished by the RP fetching the public keys and configuration data from a URL known to be controlled by the IdP or offered on the IdP’s behalf. It is also possible for the RP to be configured directly with this information in a static fashion, whereby the RP’s administrator enters the IdP information directly into the RP software.
- Additionally, the RP SHALL register its information either with the IdP or with an authority the IdP trusts, as stipulated by the trust agreement. In many federation protocols, the RP is assigned an identifier during this stage, which the RP will use in subsequent communication with the IdP.
- In all of these requirements, the IdP MAY use a trusted third party to facilitate its discovery and registration processes, so long as that trusted third party is identified in the trust agreement. For example, a consortium could make use of a hosted service that collects the configuration records of IdPs and RPs directly from participants. Instead of going to the IdP directly for its discovery record, an RP would instead go to this service. The IdP would in turn go to this service to find the identifiers and configuration information for RPs that are needed to connect.

**Section 4.4.1 Manual Registration**
_ At all FALs, the cryptographic keys and identifiers of the RP and IdP can be exchanged in a manual process, whereby the administrator of the RP submits the RP’s configuration to the IdP (either directly or through a trusted third party) and receives the identifier to use with that IdP. The RP administrator then configures the RP with this identifier and any additional information needed for the federation transaction to continue.

As this is a manual process, the registration happens prior to the federation transaction beginning._

- This process MAY be facilitated by some level of automated tooling, whereby the manual configuration points the systems in question to a trusted source of information that can be updated over time.
- If such automation is used, the trust agreement SHALL enumerate the allowable terms of the cryptographic key distribution and assignment, including allowable cache lifetimes.

**Section 4.4.2 Dynamic Registration**
- At FAL1 and FAL2, the cryptographic keys and identifiers of the RP can be exchanged in a dynamic process, whereby the RP software presents its configuration to the IdP (either directly or through a trusted third party) and receives the identifier to use with that IdP. This process is specific to the federation protocol in use but requires machine-readable configuration data to be made available over the network. All transmission of configuration information SHALL be made over a secure protected channel to endpoints associated with the IdP’s identifier by the trust agreement.
- IdPs SHOULD consider the risks of information leakage to multiple RP instances and take appropriate countermeasures, such as issuing PPIs to dynamically registered RPs as discussed in Sec. 3.3.1.
- Dynamic registration SHOULD be augmented by attestations about the RP software and device, as discussed in Sec. 3.5.3.

**Section 4.5 Subscriber Authentication at the IdP** 
- The IdP SHALL require the subscriber to have an authenticated session before any of the following events:
  - Approval of attribute release
  - Creation and issuance of an assertion
  - Establishment of a subscriber-driven trust agreement.

**Section 4.6 Authentication and Attribute Disclosure**
- The decision of whether a federation transaction proceeds SHALL be determined by the authorized party stipulated by the trust agreement. The decision can be calculated in a variety of ways, including:
 - an allowlist, which determines the circumstances under which the system can allow the federation transaction to proceed in an automated fashion;
 - a blocklist, which determines the circumstances under which the system will not allow the federation transaction to proceed; and
 - a runtime decision, which allows the authorized party to decide if the transaction can proceed and under what precise terms. Note that a runtime decision can be stored and applied to future transactions.
- The IdP SHALL provide effective mechanisms for redress of subscriber complaints or problems (e.g., subscriber identifies an inaccurate attribute value). See Sec. 3.4.3 for additional requirements and considerations for redress mechanisms.

**Section 4.6.1 IdP-Controlled Decisions**
**Section 4.6.1.1 IdP Allowlists of RPs**
- In an a priori trust agreement, IdPs MAY establish allowlists of RPs authorized to receive authentication and attributes from the IdP without a runtime decision from the subscriber.
- When placing an RP on its allowlist, the IdP SHALL confirm that the RP abides the terms of the trust agreement.
- The IdP SHALL determine which identity attributes are passed to the allowlisted RP upon authentication.
- IdPs SHALL make allowlists available to subscribers as described in Sec. 7.2.
- IdP allowlists SHALL uniquely identify RPs through the means of domain names, cryptographic keys, or other identifiers applicable to the federation protocol in use.
  - Any entities that share an identifier SHALL be considered equivalent for the purposes of the allowlist.
  - Allowlists SHOULD be as specific as possible to avoid unintentional impersonation of an RP.
- IdP allowlist entries for an RP SHALL indicate which attributes are included as part of an allowlisted decision.
  - If additional attributes are requested by the RP, the request SHALL be either:
    - subject to a runtime decision of the authorized party to approve the additional attributes requested,
    - redacted to only the attributes in the allowlist entry, or
    - denied outright by the IdP.
- IdP allowlists MAY include other information, such as the xALs under which the allowlist entry is applied. For example, an IdP could use an allowlist entry to bypass a consent screen for an FAL1 transaction but require confirmation of consent from the subscriber during an FAL3 transaction.

**Section 4.6.1.2 IdP Blocklists of RPs**
- IdPs MAY establish blocklists of RPs not authorized to receive authentication assertions or attributes from the IdP, even if requested to do so by the subscriber.
- If an RP is on an IdP’s blocklist, the IdP SHALL NOT produce an assertion targeting the RP in question under any circumstances.
- IdP blocklists SHALL uniquely identify RPs through the means of domain names, cryptographic keys, or other identifiers applicable to the federation protocol in use.
  - Any entities that share an identifier SHALL be considered equivalent for the purposes of the blocklist. For example, a wildcard domain identifier of “*.example.com” would match the domains “www.example.com”, “service.example.com”, and “unknown.example.com” equally. All three of these sites would blocked by the same blocklist entry.

**Section 4.6.1.3 IdP Runtime Decisions**
- Every RP that is in a trust agreement with an IdP but not on an allowlist with that IdP SHALL be governed by a default policy in which runtime authorization decisions will be made by an authorized party identified by the trust agreement. Since the runtime decision occurs during the federation transaction, the authorized party is generally a person and, in most circumstances, is the subscriber; however, it is possible for another party such as an administrator to be prompted on behalf of the subscriber. Note that in a subscriber-driven trust agreement, a runtime decision with the subscriber is the only allowable means to authorize the release of subscriber attributes.
- When processing a runtime decision, the IdP prompts the authorized party interactively during the federation transaction. The authorized party provides consent to release an authentication assertion and specific attributes to the RP. The IdP SHALL provide the authorized party with explicit notice and prompt them for positive confirmation before any attributes about the subscriber are transmitted to the RP.
  - At a minimum, the notice SHOULD be provided by the party in the position to provide the most effective notice and obtain confirmation, consistent with Sec. 7.2.
  - The IdP SHALL disclose which attributes will be released to the RP if the transaction is approved.
  - If the federation protocol in use allows for optional or selective attribute disclosure at runtime, the authorized party SHALL be given the option to decide whether to transmit specific attributes to the RP without terminating the federation transaction entirely.
- If the authorized party is the subscriber, the IdP SHALL provide mechanisms for the subscriber to view the attribute values and derived attribute values to be sent to the RP.
  - To mitigate the risk of unauthorized exposure of sensitive information (e.g., shoulder surfing), the IdP SHALL, by default, mask sensitive information displayed to the subscriber. For more details on masking, see Sec. 8 on usability considerations.
- An IdP MAY employ mechanisms to remember and re-transmit the same set of attributes to the same RP, remembering the authorized party’s decision. This mechanism is associated with the subscriber account as managed by the IdP.
  - If such a mechanism is provided, the IdP SHALL allow the authorized party to revoke such remembered access at a future time.

**Section 4.6.2 RP-Controlled Decisions**
**Section 4.6.2.1 RP Allowlists of IdPs** 
- RPs MAY establish allowlists of IdPs from which the RP will accept authentication and attributes without a runtime decision from the subscriber to use the IdP. 
- When placing an IdP in its allowlist, the RP SHALL confirm that the IdP abides by the terms of the trust agreement. Note that this confirmation can be facilitated by a federation authority or be undertaken directly by the RP.
- RP allowlists SHALL uniquely identify IdPs through the means of domain names, cryptographic keys, or other identifiers applicable to the federation protocol in use.
- RP allowlist entries MAY be applied based on aspects of the subscriber account (such as the xALs required for the transaction). For example, an RP could use a runtime decision for FAL1 transactions but require an allowlisted IdP for FAL3 transactions.

**Section 4.6.2.2 RP Blocklists of IdPs**
- RPs MAY also establish blocklists of IdPs that the RP will not accept authentication or attributes from, even when requested by the subscriber. A blocklisted IdP can be otherwise in a valid trust agreement with the RP, for example if both are under the same federation authority.
- RP blocklists SHALL uniquely identify IdPs through the means of domain names, cryptographic keys, or other identifiers applicable to the federation protocol in use.

**Section 4.6.2.3 RP Runtime Decisions**
- Every IdP that is in a trust agreement with an RP but not on an allowlist with that RP SHALL be governed by a default policy in which runtime authorization decisions will be made by the authorized party indicated in the trust agreement. In this mode, the authorized party is prompted by the RP to select or enter which IdP to contact for authentication on behalf of the subscriber. This process can be facilitated through the use of a discovery mechanism allowing the subscriber to enter a human-facing identifier such as an email address. This process allows the RP to programmatically select the appropriate IdP for that identifier. Since the runtime decision occurs during the federation transaction, the authorized party is generally a person and, in most circumstances, is the subscriber.
- The RP MAY employ mechanisms to remember the authorized party’s decision to use a given IdP. Since this mechanism is employed prior to authentication at the RP, the manner in which the RP provides this mechanism (e.g., a browser cookie outside the authenticated session) is separate from the RP subscriber account described in Sec. 3.10.1.
  - If such a mechanism is provided, the RP SHALL allow the authorized party to revoke such remembered options at a future time.

**Section 4.6.3 Provisioning Models for RP subscriber accounts**
- The lifecycle of the provisioning process for an RP subscriber account varies depending on factors including the trust agreement discussed in Sec. 3.4 and the deployment pattern of the IdP and RP. However, in all cases, the RP subscriber account SHALL be provisioned at the RP prior to the establishment of an authenticated session at the RP in one of the following ways:

_Just-In-Time Provisioning_
- An RP subscriber account is created automatically the first time the RP receives an assertion with an unknown federated identifier from an IdP. Any identity attributes learned during the federation process, either within the assertion or through an identity API as discussed in Sec. 3.11.3, MAY be associated with the RP subscriber account.
- Accounts provisioned in this way are bound to the federated identifier in the assertion used to provision them.
- This is the most common form of provisioning in federation systems, as it requires the least coordination between the RP and IdP. However, in such systems, the RP SHALL be responsible for managing any cached attributes it might have. 

_Pre-provisioning_
- An RP subscriber account is created by the IdP pushing the attributes to the RP or the RP pulling attributes from the IdP. Pre-provisioning of accounts generally occurs in bulk through a provisioning API as discussed in Sec. 4.6.5, as the provisioning occurs prior to the represented subscribers authenticating through a federation transaction.
- Pre-provisioned accounts SHALL be bound to a federated identifier at the time of provisioning. Any time a particular federated identifier is seen by the RP, the associated account can be logged in as a result. This form of provisioning requires infrastructure and planning on the part of the IdP and RP, but these processes can be facilitated by automated protocols. Additionally, the IdP and RP must keep the set of provisioned accounts synchronized over time as discussed in Sec. 4.6.4.
- In this model, the RP also receives attributes about subscribers who have not yet interacted with the RP (and who may never do so). This is in contrast to other models, where the RP receives information only about the subset of subscribers that use the RP, and then only after the subscriber uses the RP for the first time. The privacy considerations of the RP having access to this information prior to a federation transaction SHALL be accounted for in the trust agreement.

_Ephemeral_
- An RP subscriber account is created when processing the assertion, but then the RP subscriber account is terminated when the authenticated session ends. This process is similar to a just-in-time provisioning, but the RP keeps no long-term record of the account when the session is complete, in accordance with Sec. 3.10.3. This form of provisioning is useful for RPs that fully externalize access rights to the IdP, allowing the RP to be more simplified with less internal state. However, this pattern is not common because even the simplest RPs tend to have a need to track state within the application or at least keep a record of actions associated with the federated identifier.

_Other_
- Other RP subscriber account provisioning models are possible but the details of such models are outside the scope of these guidelines. The details of any alternative provisioning model SHALL be included in the privacy risk assessments of the IdP and RP.

- All organizations SHALL document their provisioning models as part of their trust agreement.

**Section 4.6.4 Attribute Synchronization**

- [T]he RP MAY additionally collect, and optionally verify, other attributes to associate with the RP subscriber account, as discussed in Sec. 4.6.6. Sometimes, these attributes can even override what is asserted by the IdP. For example, if an IdP asserts a full display name for the subscriber, the RP can allow the subscriber to provide an alternative preferred name for use at the RP.
- The IdP SHOULD signal downstream RPs when the attributes of a subscriber account available to the RP have been updated, and the RP MAY respond to this signal by updating the attributes in the RP subscriber account. This synchronization can be accomplished using shared signaling as described in Sec. 4.8, through a provisioning API as described in Sec. 4.6.5, or by providing a signal in the assertion (e.g., a timestamp indicating when relevant attributes were last updated) allowing the RP to determine that its cache is out of date.
- If the RP is granted access to an identity API as in Sec. 3.11.3, the IdP SHOULD allow the RP access to the API for sufficient time to perform synchronization operations after the federation transaction has concluded. For example, if the assertion is valid for five minutes, access to the identity API could be valid for 30 minutes to allow the RP to fetch and update attributes out of band.
- The IdP SHOULD signal downstream RPs when a subscriber account is terminated, or when the subscriber account’s access to an RP is revoked. This can be accomplished using shared signaling as described in Sec. 4.8 or through a provisioning API as described in Sec. 4.6.5.
  - Upon receiving such a signal, the RP SHALL process the RP subscriber account as stipulated in the trust agreement.
  - If the RP subscriber account is terminated, the RP SHALL remove all personal information associated with the RP subscriber account, in accordance with Sec. 3.10.3.
  - If the reason for termination is suspicious or fraudulent activity, the IdP SHALL include this reason in its signal to the RP to allow the RP to review the account’s activity at the RP for suspicious activity, if specified in the trust agreement with that RP.

**Section 4.6.5 Provisioning APIs**
- The attributes in the provisioning API available to a given RP SHALL be limited to only those necessary for the RP to perform its functions, including any audit and security purposes as discussed in Sec. 3.9.1.
- As part of establishing the trust agreement, the IdP SHALL document when an RP is given access to a provisioning API including at least the following:
  - the purpose for the access using the provisioning model;
  - the set of attributes made available to the RP;
  - whether the API functions as a push to the RP, a pull from the RP, or both; and
  - the population of subscribers whose attributes are made available to the RP.
- Access to the provisioning API SHALL occur over a mutually authenticated protected channel. The exact means of authentication varies depending on the specifics of the API and whether it is a push model (where the IdP connects to the RP) or a pull model (where the RP connects to the IdP).
- A provisioning API SHALL NOT be made available under a subscriber-driven trust agreement.
- The IdP SHALL NOT make a provisioning API available to any RP outside of an established trust agreement.
- The IdP SHALL provide access to a provisioning API only as part of a federated identity relationship with an RP to facilitate federation transactions with that RP and related functions such as signaling revocation of the subscriber account.
- The IdP SHALL revoke an RP’s access to the provisioning API once access is no longer required by the RP for its functioning purposes or when the trust agreement is terminated.
- Any provisioning API provided to the RP SHALL be under the control and jurisdiction of the IdP.
  - External attribute providers MAY be used as information sources by the IdP to provide attributes through this provisioning API, but the IdP is responsible for the content and accuracy of the information provided by the referenced attribute providers.
- When a provisioning API is in use, the IdP SHALL signal to the RP when a subscriber account has been terminated.
  - When receiving such a signal, the RP SHALL remove the binding of the federated identifier from the account and SHALL terminate the account if necessary (e.g., there are no other federated identifiers linked to this account or the trust agreement dictates such an action).
  - The RP SHALL remove all personal information sourced from the provisioning API in accordance with Sec. 3.10.3.

**Section 4.6.6 Collection of Additional Attributes by the RP**
- The RP MAY collect and maintain additional attributes from the subscriber beyond those provided by the IdP. For example, the RP could collect a preferred display name directly from the subscriber that is not provided by the IdP. The RP could also have a separate agreement with an attribute provider that gives the RP access to an identity API not associated with the IdP. For example, the RP could receive a state license number from the IdP, but use a separate attribute verification API to check if a particular license number is currently valid. The assertion from the IdP binds the license to the subscriber, but the attribute verification API provides additional information beyond what the IdP can share or be authoritative for.
  - These attributes are governed separately from the trust agreement since they are collected by the RP outside of a federation transaction. All attributes associated with an RP subscriber account, regardless of their source, SHALL be removed when the RP subscriber account is terminated, in accordance with Sec. 3.10.3.
- The RP SHALL disclose to the subscriber the purpose for collection of any additional attributes.
  - These attributes SHALL be used solely for the stated purposes of the RP’s functionality and SHALL NOT have any secondary use, including communication of said attributes to other parties.
- The RP SHALL provide an effective means of redress for the subscriber to update and remove these additionally-collected attributes from the RP subscriber account. See Sec. 3.4.3 for additional requirements and considerations for redress mechanisms.

**Section 4.6.7 Time-based Removal of RP Subscriber Accounts**
- If an RP is using a just-in-time provisioning mechanism, the RP only learns of the existence of a subscriber account when that account is first used at the RP. If the IdP does not inform the RP of terminated subscriber accounts using shared signaling as described in Sec 4.8, an RP could accumulate RP subscriber accounts that are no longer accessible from the IdP. This poses a risk to the RP for holding personal information in the RP subscriber accounts. In such circumstances, the RP MAY employ a time-based mechanism to identify RP subscriber accounts for termination that have not been accessed after a period of time tailored to the usage patterns of the application. For example, an RP that is usually accessed on a weekly basis could set a timeout of 120 days since last access at the RP to mark the RP subscriber account for termination. An RP that expects longer gaps between access, such as a service used annually, should have a much longer time frame, such as five years.
- When processing such an inactive account, the RP SHALL provide sufficient notice to the subscriber, about the pending termination of the account and provide the subscriber with an option to re-activate the account prior to its scheduled termination.
- Upon termination, the RP SHALL remove all personal information associated with the RP subscriber account, in accordance with Sec. 3.10.3.

**Section 4.7 Reauthentication and Session Requirements in Federated Environments**
- The IdP ending the subscriber’s session at the IdP will not necessarily cause any sessions that subscriber might have at downstream RPs to end as well. The RP and IdP MAY communicate end-session events to each other, if supported by the federation protocol or through shared signaling (see Sec. 4.8).
- At the time of a federated transaction request, the subscriber could have a pre-existing authenticated session at the IdP which MAY be used to generate an assertion to the RP.
  - The IdP SHALL communicate to the RP any information the IdP has regarding the time of the subscriber’s latest authentication event at the IdP, and the RP MAY use this information in making authorization and access decisions.
  - Depending on the capabilities of the federation protocol in use, the IdP SHOULD allow the RP to request that the subscriber provide a fresh authentication at the IdP instead of using the existing session at the IdP. For example, suppose the subscriber authenticates at the IdP for one transaction. Then, 30 min later, the subscriber starts a federation transaction at the RP. Depending on xAL requirements, the subscriber’s existing session at the IdP can be used to avoid prompting the subscriber for their authenticators. The resulting assertion to the RP will indicate that the last time the subscriber had authenticated to the RP was 30 min in the past. The RP can then use this information to determine whether this is reasonable for the RP’s needs, and, if possible within the federation protocol, request the IdP to prompt the subscriber for a fresh authentication event instead.
- An RP requiring authentication through a federation protocol SHALL specify the maximum acceptable authentication age to the IdP, either through the federation protocol (if possible) or through the terms of the trust agreement.
  - The authentication age represents the time since the last authentication event in the subscriber’s session at the IdP, and the IdP SHALL reauthenticate the subscriber if they have not been authenticated within that time period.
  - The IdP SHALL communicate the authentication event time to the RP to allow the RP to decide if the assertion is sufficient for authentication at the RP and to determine the time for the next reauthentication event.
- If an RP is granted access to an identity API at the same time the RP receives an assertion, the lifetime of the access to the identity API is independent from the lifetime of the assertion. As a consequence, the RP’s ability to successfully fetch additional attributes through an identity API SHALL NOT be used to establish a session at the RP.
  - Likewise, inability to access an identity API SHOULD NOT be used to end the session at the RP.
- The RP MAY terminate its authenticated session with the subscriber or restrict access to the RP’s functions if the assertion, authentication event, or attributes do not meet the RP’s requirements. For example, if an RP is configured to allow access to certain high-risk functionality only if the federation transaction was at FAL3, but the incoming assertion only meets the requirements for FAL2, the RP could decide to deny access to the high-risk functionality while allowing access to lower-risk functionality, or the RP could choose to terminate the session entirely.
  
**Section 4.8 Shared Signaling**
- In some environments, it is useful for the IdP and RP to send information to each other outside of the federation transaction. These signals can communicate important changes in state between parties that would not be otherwise known. The use of any shared signaling SHALL be documented in the trust agreement between the IdP and RP.
  - Signaling from the IdP to the RP SHALL require an apriori trust agreement.
  - Signaling from the RP to the IdP MAY be used in both apriori and subscriber-driven trust agreements.
- Any use of shared signaling SHALL be documented and made available to the authorized party stipulated by the trust agreement.
  - This documentation SHALL include the events under which a signal is sent, the information included in such a signal (including any attribute information), and any additional parameters sent with the signal.
  - The use of shared signaling SHALL be subject to privacy review under the trust agreement.
- The IdP SHOULD send a signal regarding the following changes to the subscriber account:
  - The account has been terminated.
  - The account is suspected of being compromised.
  - Attributes of the account, including identifiers other than the federated identifier (such as email address or certificate common name), have changed.
  - The possible range of IAL, AAL, or FAL for the account has changed.
  - If the RP receives a signal that an RP subscriber account is suspected of compromise, the RP SHOULD review actions taken by that account at the RP for suspicious activity.
- The RP SHOULD send a signal regarding the following changes to the RP subscriber account:
  - The account has been terminated.
  - The account is suspected of being compromised.
  - A bound authenticator is added by the RP.
  - A bound authenticator is removed by the RP.
- If the IdP receives a signal that a subscriber account is suspected of compromise, the IdP SHALL review actions taken by that account at the IdP for suspicious activity.
- If suspicious activity is confirmed at the IdP, the IdP SHALL signal any additional RPs the subscriber account was used for during the suspected time frame.
- Additional signals from both the IdP and RP MAY be allowed subject to privacy and security review as part of the trust agreement.

**Section 4.9 Assertion Contents**
- Assertions SHALL represent a discrete authentication event of the subscriber at the IdP and SHALL be processed as a discrete authentication event at the RP.
- All assertions SHALL include the following attributes:
  - Subject identifier: An identifier for the party to which the assertion applies (i.e., the subscriber).
  - Issuer identifier: An identifier for the issuer of the assertion (i.e., the IdP).
  - Audience identifier: An identifier for the party intended to consume the assertion (i.e., the RP). An assertion can contain more than one audience identifier at FAL1.
  - Issuance time: A timestamp indicating when the IdP issued the assertion.
  - Validity time window: A period of time outside of which the assertion SHALL NOT be accepted as valid by the RP for the purposes of authenticating the subscriber and starting an authenticated session at the RP. This is usually communicated by means of an expiration timestamp for the assertion in addition to the issuance timestamp.
  - Assertion identifier: A value uniquely identifying this assertion, used to prevent attackers from replaying prior assertions.
  - Authentication time: A timestamp indicating when the IdP last verified the presence of the subscriber at the IdP through a primary authentication event.
  - Nonce: A cryptographic nonce, if one is provided by the RP.
  - Signature: Digital signature or message authentication code (MAC), including key identifier, covering the entire assertion.\
- All assertions SHALL contain sufficient information to determine the following aspects of the federation transaction:
  - The IAL of the subscriber account being represented in the assertion, or an indication that no IAL is asserted.
  - The AAL used when the subscriber authenticated to the IdP, or an indication that no AAL is asserted.
  - The IdP’s intended FAL of the federation process represented by the assertion.

_FAL3 requirements deleted_

- Assertions MAY also include additional items, including the following information:
  - Attribute values and derived attribute values: Information about the subscriber.
  - Attribute bundles: Collections of attributes in a signed bundle from the CSP.
  - Attribute metadata: Additional information about one or more subscriber attributes, such as those described in [NISTIR8112].
  - Authentication event: Additional details about the authentication event, such as the class of authenticator used.
- The RP SHALL validate the assertion by checking that all the following are true:
  - Signature validation: ensuring that the signature of the assertion is valid and corresponds to a key belonging to the IdP sending the assertion.
  - Issuer verification: ensuring that the assertion was issued by the IdP the RP expects it to be from.
  - Time validation: ensuring that the expiration and issue times are within acceptable limits of the current timestamp.
  - Audience restriction: ensuring that this RP is the intended recipient of the assertion.
  - Nonce: ensuring that the cryptographic nonce included in the RP’s request (if applicable) is included in the presentation.
  - Transaction terms: ensuring that the IAL, AAL, and FAL represented by the assertion are allowable under the applicable trust agreement.
- An RP SHALL treat subject identifiers as not inherently globally unique. Instead, the value of the assertion’s subject identifier is usually in a namespace under the assertion issuer’s control, as discussed in Sec. 3.3. This allows an RP to talk to multiple IdPs without incorrectly conflating subjects from different IdPs.
- Assertions MAY include additional attributes about the subscriber. Section 3.9 contains privacy requirements for presenting attributes in assertions.
  - The RP MAY be given limited access to an identity API as discussed in Sec. 3.11.3, either in the same response as the assertion is received or through some other mechanism. 
    - The RP can use this API to fetch additional identity attributes for the subscriber that are not included in the assertion itself.
- The assertion’s validity time window is the time between its issuance and its expiration. This window needs to be large enough to allow the RP to process the assertion and create a local application session for the subscriber, but should not be longer than necessary for such establishment. Long-lived assertions have a greater risk of being stolen or replayed; a short assertion validity time window mitigates this risk. Assertion validity time windows SHALL NOT be used to limit the session at the RP. 

**Section 4.10 Assertion Requests**
- When the federation transaction is initiated by the RP, the RP’s request for an assertion SHALL contain:
  - An identifier for the RP
  - A cryptographic nonce, to be returned in the assertion
- The RP’s request SHOULD additionally contain:
  - The set of identity attributes requested by the RP and their purpose of use at the RP; this is a subset of what is allowed by the trust agreement
  - The requirements for the authentication event at the IdP
    
**Note that federation transactions are always initiated by the RP at FAL2 or higher.**

**Section 4.11 Assertion Presentation**
-Assertions MAY also be proxied to facilitate federation between IdPs and RPs using different presentation methods, as discussed in detail in Sec. 3.2.3.

**Section 4.11.1 Back-Channel Presentation**
- In the back-channel presentation model shown in Fig. 11, the subscriber is given an assertion reference to present to the RP, generally through the front channel. The assertion reference itself contains no information about the subscriber and SHALL be resistant to tampering and fabrication by an attacker. The RP presents the assertion reference to the IdP to fetch the assertion. How this is achieved varies form one protocol to the next. In the authorization code flow and some forms of the hybrid flow of [OIDC] the assertion (the ID Token) is presented in the back channel in exchange for the assertion reference (the authorization code). In the artifact binding profile of [SAML-Bindings], the SAML assertion is presented in the back channel.
- [T]he back-channel presentation model consists of three steps:
  - The IdP sends an assertion reference to the subscriber through the front channel.
  - The subscriber sends the assertion reference to the RP through the front channel.
  - The RP presents the assertion reference and its RP credentials to the IdP through the back channel. The IdP validates the credentials and returns the assertion.
- The assertion reference:
  - SHALL be limited to use by a single RP.
  - SHALL be single-use.
  - SHALL be time limited, and
  - SHOULD have a validity time window of no more than five minutes.
  - SHALL be presented along with authentication of the RP to the IdP.
  - SHALL NOT be predictable or guessable by an attacker.

_In this model, the RP directly requests the assertion from the IdP, minimizing chances of interception and manipulation by a third party (including the subscriber themselves). More network transactions are required in the back-channel method, but the information is limited to only those parties that need it. Since an RP is expecting to get an assertion only from the IdP directly as a result of its request, the attack surface is reduced. Consequently, it is more difficult to inject assertions directly into the RP and this presentation method is recommended for FAL2 and above. Since the IdP and RP are already directly connected, the back-channel presentation method facilitates the use of identity APIs, as described in Sec. 3.11.3._

_Note that while it is technically possible for an assertion reference (which is single-audience) to result in a multi-audience assertion, this situation is unlikely. For this reason, back-channel presentation is practically limited to use with single-audience assertions._

- Conveyance of the assertion reference from the IdP to the subscriber, as well as from the subscriber to the RP, SHALL be made over an authenticated protected channel.
- Conveyance of the assertion reference from the RP to the IdP, as well as the assertion from the IdP to the RP, SHALL be made over an authenticated protected channel.
- The RP SHALL protect itself against injection of manufactured or captured assertion references by the use of cross-site scripting protection, rejecting assertion references outside of the correct stage of a federation transaction, or other accepted techniques discussed in Sec. 3.10.1.
- When assertion references are presented to the IdP, the IdP SHALL verify that the RP presenting the assertion reference is the same RP that made the assertion request resulting in the assertion reference. Examples for this are discussed in Sec 10.12 such as the authorization code flow of [OIDC] with additional security profiles such as [FAPI].

_Note that in a federation proxy described in Sec. 3.2.3, the upstream IdP audience restricts the assertion reference and assertion to the proxy, and the proxy restricts any newly-created assertion references or assertions to the downstream RP._

**Section  4.11.2 Front-Channel Presentation**
_In the front-channel presentation model shown in Fig. 12, the IdP creates an assertion and sends it to the RP by means of a third party, such as the subscriber’s user agent. In the implicit flow and some forms of the hybrid flow of [OIDC], the assertion (the ID Token) is presented in the front channel. In the SAML Web SSO profile defined in [SAML-WebSSO], the SAML assertion is presented in the front channel._

_Front-channel presentation methods expose the assertion to parties other than the IdP and RP, which increases the risk for leakage of PII and other information included in the assertion. Additionally, there is an increased attack surface for the assertion to be captured and replayed by an attacker. As a consequence, it is recommended to not use front-channel presentation when other mechanisms are available._

- The RP SHALL use the assertion identifier ensure that a given assertion is presented at most once during the assertion’s validity time window.
- The RP SHALL protect itself against injection of manufactured or captured assertion by the use of cross-site scripting protection, rejecting assertions outside of the correct stage of a federation transaction, or other accepted techniques discussed in Sec. 3.10.1.
- Conveyance of the assertion from the IdP to the subscriber, as well as from the subscriber to the RP, SHALL be made over an authenticated protected channel.
- With general-purpose IdPs, it is common for front-channel communications to be accomplished using HTTP redirects, where the contents of the assertion are made available as part of an HTTP request URL. Due to the nature of the HTTP ecosystem, these request URLs are sometimes available in unexpected places, such as access logs and browser history. These logs and other artifacts tend to live on long past the federation transaction and are available in other contexts, which increases the attack surface for reading the assertion. As a consequence, an IdP that uses HTTP redirects for front channel presentation of assertions that contain PII SHALL encrypt the assertion as discussed in Sec 3.12.3.

**Section 5 Subscriber-Controlled Wallets**
_N.B. Deleted for IPSIE SL1 -dhs_

