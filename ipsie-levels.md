# IPSIE Levels

- *SL* - Session Lifecycle
- *AL* - Account Lifecycle

Each level includes the previous level (_e.g._ SL3 includes the requirements of SL1 and SL2). SL* and AL* are independent of each other.

| IPSIE<br>LEVEL|   Application (aka RP)                                                 |  Identity Service                                                                                             |
|---------------|----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| SL1           |   - MUST meet NIST 800-63-4 FAL2 compliance* <br>- Application-specific session lifetime MUST be set from the assertion | - MUST meet NIST 800-63-4 FAL2 Compliance* <br> - MUST enforce MFA and communicate an authentication class to the Application |
| SL2           |  - When requested from the Identity Service, MUST require the user to re-authenticate at the Identity Service before continuing any action. <br> MUST revoke any active sessions and tokens at the request of the Identity Service <br> - MUST not accept unsolicited federation assertions| - MUST enforce authentication method requests from Application |
| SL3           |  - MUST communicate session state changes to Identity Service | - MUST communicate user, session, and device state changes to the Application |
||||
| AL1           | - MUST support suspend, archive, or delete of users by the Identity Service | - MUST deprovision accounts from the Application|
| AL2           |  - MUST support create and update of users by the Identity Service prior to sign-in <br>- Local create, Update, and Delete of users and the Identity Service provided profiles SHALL NOT be allowed <br>- MUST support mapping group claims to application roles and capabilities |- MUST synchronize provisioned users and their profile data to the Application <br> - MUST synchronize user group membership claims to Application |
| AL3           |  - MUST expose application roles to the Identity Service | - MUST consume Application roles and map to users<br> - MUST synchronize user role claims to Application |
-----

### IPSIE Session Lifecycle SL1 - Single Sign-On & Session Lifetime Controls

Level SL1 enables basic single sign-on from applications to the identity provider, communicating identity statements about the user. Single sign-on in Level SL1 meets the technical requirements of [FAL2 in NIST 800-63-4](https://pages.nist.gov/800-63-4/sp800-63c/fal/). 

***Note:** IPSIE does not include all of the controls specified in NIST 800-63-4 at FAL2.  IPSIE SL1 requires the technical controls from FAL2 which impact the security of the federation protocol(s).  Business agreements, such as data handling policies, are out of scope for IPSIE. 

The Application respects the session lifetime as communicated by the Identity Service in the assertion, and re-validates the session with the Identity Service after the expiration. Re-validation can occur with a new single sign-on flow, or using refresh tokens. It is likely that the session lifetime communicated by the Identity Service is shorter than the session at the Identity Service. The goal is to let the Identity Service set the interval in which the RP checks back at the Identity Service.

The Identity Service MUST communicate information about the user's authentication method at the Identity Service in the SSO assertion.

### IPSIE Session Lifecycle SL2 - MFA, Logout, & Session Termination

Level SL2 adds the ability for the Application to request specific authentication methods when the user logs in at the Identity Service.

Applications MUST NOT accept unsolicited federation requests from the identity service (e.g. SAML IdP initiated federation).

When requested by the Identity Service, Applications MUST require the specified user re-authenticate at the Identity Service before continuing any action within the Application. This is effectively a "force re-authenticate" command, or an "expire session" command. This command does not place any requirements on what the Application does with artifacts like session identifiers or tokens. Identity Services are not obligated to send this command, but Applications are required to support receiving it.

When requested by the Identity Service, Applications MUST terminate all active sessions and any access tokens and refresh tokens they may have created for the specified user. Identity Services are not obligated to send this command, but Applications are required to support receiving it.

### IPSIE Session Lifecycle SL3 - Continuous Access

Level SL3 adds continuous access to the authentication between Identity Service and Application.

The Application communicates session changes to the Identity Service such as IP address change, enabling the Identity Service to be aware of more context around what is happening to users' sessions after the initial sign-in.

The Identity Service communicates changes in the account and device posture to the application, enabling the application to take actions it determines are necessary based on its own policies about these changes.  Neither application nor identity services are obliged to act upon any state changes, the policies for responding to state changes are not in scope for SL3.

### IPSIE Account Lifecycle Level AL1 - User Deprovisioning

IPSIE Account Lifecycle Level AL1 requires the Application to deprovision users at the request of the Identity Service.

### IPSIE Account Lifecycle Level AL2 - User and Group Management

Level AL2 adds the ability for the Identity Service to synchronize with the Application the users that have access and their profile data. The Application SHALL NOT independently create, update, or delete users, or the provided profile data, of users managed by Identity Services. While an Application may also support support Just In Time (JIT) for account creation using claims in an SSO token, JIT support is NOT a requirement of IPSIE. It also allows the Identity Service to organize users into groups and to synchonize group membership with the Application. The Application MUST use group membership to determine the roles and capabilities of the user.

### IPSIE Account Lifecycle Level AL3 - User Role Management

Level AL3 adds the ability for the Application to publish the roles that exist in the Application to the Identity Service, and for the Identity Service to map these roles to users and synchronize with the Application which roles each user has.


## IPSIE Security Controls 

The list below captures the security features described in the levels above. It is meant to summarize the security controls and capabilities provided at the different levels, rather than focus on protocol features.

### SL1

* Identity Service controls session lifetime in the Application (`session_expiry` claim in ID token)
* Identity Service communicates user's authentication methods in the ID token (`amr` and `auth_time` claims)
* Identity Service allows Application to specify the maximum acceptable time since the last time the user interactively authenticated at the Identity Service (`max_age`)
* Identity Service rejects password grant
* Identity Service requires authorization code flow with PKCE

### SL2

* Identity Service allows Application to request specific minimum authentication methods
* Application prohibits unsolicited federation assertions from the Identity Service
* Identity Service can terminate sessions and tokens at the Application
* Identity Service can require the user re-authenticate at the Identity Service before they can continue to interact with the Application

### SL3

* Application communicates session state changes to the Identity Service (actual state changes to be defined, such as IP address change)
* Identity Service communicates changes in account and device posture to the Application (to be defined)

### AL1

* Identity Service can suspend/archive/delete/deprovision accounts at the Application

### AL2

* Identity Service can create and update accounts at the Application
* Application does not allow local accounts to be created in the tenant managed by the Identity Service
* Application supports mapping Identity Service groups to local Application roles and capabilities

### AL3

* Application exposes available roles to Identity Service so they can be managed and mapped to accounts





