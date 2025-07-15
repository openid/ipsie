# Abstract
The IPSIE IL1 SCIM 2.0 Profile defines a profile of SCIM 2.0 intended to meet the security and interoperability requirements of enterprise integrations using SCIM. It establishes a clear baseline for provisioning and lifecycle management of enterprise users and the roles those users are assigned. The IL1 specification ensures that CRUD operations initiate from the Identity Service and that local modifications within the Application do not occur.


# 1.0 Introduction
This document defines the IPSIE Identity Lifecycle 1 (SL1) Profile for SCIM 2.0. It provides a clear reference for SCIM deployments that require a well-defined security baseline meeting best practices for interoperable enterprise identity management.

The profile addresses critical aspects of secure identity management, with particular emphasis on:
* Client Authentication
* Creation, Update, and Deletion of accounts
* Synchronization of data from the Identity Service to the Application

By adhering to this profile, organizations can implement SCIM-based integrations that meet stringent security requirements while ensuring interoperability across different implementations.

# 2.0 Conventions and Definitions
The keywords "shall", "shall not", "should", "should not", "may", and "can" in this document are to be interpreted as described in ISO Directive Part 2 [ISODIR2]. These keywords are not used as dictionary terms such that any occurrence of them shall be interpreted as keywords and are not to be interpreted with their natural language meanings.


## 2.1 Terminology
The terms "SCIM", "SCIM Client", and "SCIM Service" refer to the terminology defined in [RFC 7644].

## 2.2 Roles
* **Identity Service**: Acts as the SCIM Client, initiating all provisioning operations.
* **Application**: Acts as the SCIM Service, hosting SCIM endpoints and processing all provisioning requests.


# 3.0 Profile

## 3.1 OAuth 2.0 Requirements
Identity Services shall integrate with the Application's SCIM endpoint using OAuth 2.0 for secure authorization and token-based authentication. The following requirements ensure consistent and secure handling of access tokens and authorization server configuration:
* OAuth 2.0 interactions must comply with JWT Client Authentication as defined in RFC 7523.
* The token shall exchange a signed JWT for an access token and present that token in the {Authorization: Bearer} header on all subsequent SCIM requests.
* The token must be scoped to "scim" and not grant broader permissions.
* The token must contain a "token_endpoint" value which is the URL of the Application Provider's OAuth 2.0 token endpoint.
* All Authorization Server parameters should be discovered from OAuth Authorization Server metadata as defined in RFC 8414.
* The Identity Service should expose a jwks_uri to allow the Application to perform signature verification

## 3.2 SCIM Interoperability Requirements

### 3.2.1 General Requirements
* The Identity Service shall implement the required functionality of a SCIM Client as defined in RFC 7643 and RFC 7644.
* The Application shall implement the required functionality of a SCIM Client as defined in RFC 7643 and RFC 7644.
* All SCIM operations shall be authenticated via OAuth 2.0; local modifications are prohibited.

### 3.2.2 User Provisioning Operations
The Application MUST provide support all User provisioning operations defined in this section.

#### 3.2.3 Create User (POST /Users)
User creation is performed by the SCIM operation POST /Users

In addition to the user attributes required by RFC 7643, the following attributes are required to be part of the account schema:
* An attribute which contains a unique identifier used by the enterprise to distinguish the owner of the account, such as "externalId."
* An attribute which contains the primary email address of the user, such as "email"

#### 3.2.4 Update User (PATCH /Users/{id})
User updates are performed via the SCIM operation: PATCH /Users/{id}

#### 3.2.5 Deactivate or Reactivate User (PATCH /Users/{id})
Changes to the user activation status are performed via the SCIM operation: PATCH /Users/{id}

The Identity Service SHOULD propagate user deactivation events to the Application within 5 minutes of the user being deactivated.

The Application SHOULD respond to user deactivation events by revoking the ability for the user to continue accessing the Application, including the revocation of currently active sessions. The revocation mechanism is an implementation detail outside the scope of this specification. Revocation SHOULD occur within 5 minutes of receiving the deactivation.

When a user account is deactivated, all access mechanisms and authorizations associated with that account must also be deactivated. This includes, but is not limited to:
* Web sessions
* API tokens
* Refresh tokens
* Personal access tokens
* SSH keys associated with the user
* Device-based authentication credentials

The Application MUST allow reactivation of a deactivated user. 

#### 3.2.6 Delete User (DELETE /Users/{id})
User deletions are performed via the SCIM operation: DELETE /Users/{id}

After a user is deleted, Application MUST allow the creation of a new user with the same username. 

#### 3.2.7 Get User By ID (GET /Users/{id})
User searches by id are performed via the SCIM operation: GET /Users/{id}

#### 3.2.8 List Users By Alternate Identifier (GET /Users?)
User searches by alternate identifier are performed via the SCIM operation: GET /Users?filter={filterExpression}

Application Providers MUST support the following filter expressions:
* username eq {username}
* externalId eq {externalId}
* emails[value eq {email}]

### 3.3 Group (Role) Provisioning Operations
The Application MUST provide support all Group provisioning operations defined in this section.

**Note**: Within the IPSIE standard, Application permissions are referred to as "Roles." Within SCIM, Application permissions are referred to as "Groups." The term "Role" in IPSIE is functionally equivalent to the term "Group" in SCIM.

#### 3.3.1 Get Group By ID (GET /Group/{id})
Group searches by id are performed via the SCIM operation: GET /Group/{id}?excludedAttributes=members

#### 3.3.2 List Groups By Alternate Identifier (GET /Groups?)
User lookups by alternate identifier are performed via the SCIM operation: GET /Groups?filter={filterExpression}&excludedAttributes=members

#### 3.3.3 Add or Remove Group Members (PATCH /Group/{id})
Members are added or removed from Groups via the SCIM operation: PATCH /Groups/{id}

For each Operation in the PATCH:

The op attribute MUST contain either "add" or "remove".
* When the op is "add":
    * The path attribute MUST be "members".
    * The value attribute MUST be an array of Group member elements, as defined in Section 4.2 of the SCIM Core Schema [RFC7643]. Each member MUST contain a value subattribute with the id of the resource being added to the group.

* When the op is "remove":
    * The path attribute MUST be either:
        * "members" (to remove all members)
        * "members[value eq \"{id}\"]" (to remove a single member)
    * The value attribute MUST be unspecified.

### 3.4 Metadata Endpoints

#### 3.4.1 /ResourceTypes
Application MUST host a /ResourceTypes endpoint, as defined in Section 4 of RFC 7644.

The supported ResourceTypes MUST include Users and Groups. 

#### 3.4.2 /ServiceProviderConfig
Application Providers MUST host a /ServiceProviderConfig endpoint to describe the operations they support, as defined in Section 4 of RFC 7644.

The operations MUST include, at minimum, the set of SCIM capabilities required for compatibility with this IPSIE profile.

#### 3.4.3 /Schemas
Application Providers MUST host a /Schemas endpoint to describe the supported schemas, as defined in Section 4 of RFC 7644. 

# 4.0 Security Considerations
For SCIM security considerations, see RFC 7643 and RFC 7644. 

Additionally, the following security considerations must be included:
* **Transport Security**: All endpoints shall enforce TLS 1.2 or later with strong cipher suites and certificate validation.
* **Error Handling**: Error responses shall use the SCIM error format and shall not leak internal details.
* **Replay Resistance**: Access tokens shall expire and nonces shall be validated to prevent replay.
* **Auditing**: All provisioning actions and responses shall be logged for audit and troubleshooting.

# 5.0 Compliance Statement
Implementation of all mandatory requirements in this profile will result in a SCIM 2.0 deployment that satisfies IPSIE Identity Lifecycle Level 1 (IL1). Specifically:

* **Identity Service (SCIM Client)**
    * Shall initiate all CRUD operations for Users and Groups.

* **Application (SCIM Service)**
    * Shall host all SCIM endpoints with full support for User and Group provisioning.
    * Shall prevent local modifications outside of SCIM.
    * Shall enforce OAuth 2.0 JWT Profile for authentication and adhere to the security considerations above.

By conforming to this profile, implementations will achieve a consistent, secure, and interoperable baseline for enterprise identity lifecycle management.

## 6.0 Normative References
TODO

## 7.0 Acknowledgements
TODO

## 8.0 Author's Address
TODO
