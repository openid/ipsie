# IPSIE Levels

- *SL* - Session Lifecycle
- *IL* - Identity Lifecycle

Each level includes the previous level (_e.g._ SL3 includes the requirements of SL1 and SL2). Each set of levels is _independent_ from other levels (e.g. an application may achieve IL3 while all other sets are at Level 1).

| IPSIE<br>LEVEL|   Applications<br>(aka RP)                                                 |  Identity Services                                                                                             |
|---------------|----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| SL1           |   - MUST meet NIST800-63rev3 FAL2 compliance <br>- Session lifetime MUST match assertion lifetime | - MUST meet NIST800-63rev3 FAL2 Compliance <br> - MUST enforce MFA and communicate an authentication class to the application |
| SL2           |  - MUST terminate sessions at the request of the Identity Service| - MUST enforce authentication method requests from Applications |
| SL3           |  - MUST communicate session state changes to Identity Services | - MUST communicate user, session, and device state changes to the Application |
||||
| IL1           |  - MUST support JIT provisioning of users via SSO <br> - MUST accept user attributes during provisioning <br> - Out of band provisioning/self provisioning users to the organization SHALL NOT be allowed | |
| IL2           |  - MUST support pre-provisioning of users by the Identity Services prior to signin<br> - MUST support deprovisioning of users by Identity Services <br> - MUST support mapping group claims to application roles | - MUST send selected group claims to Applications |
| IL3           |  - MUST expose application roles to the Identity Service | - MUST consume application roles and map to users<br> - MUST include roles as user attributes in JIT and async provisioning |

-----
### IPSIE Session Lifecycle SL1 - Single Sign-On & Session Lifetime Controls

Level SL1 enables basic single sign-on from applications to the identity provider, communicating identity statements about the user. Single sign-on in Level SL1 meets the requirements of [FAL2](https://pages.nist.gov/800-63-4/sp800-63c/fal/).

The Application respects the session lifetime as communicated by the Identity Service in the assertion, and reauthenticates the user through the Identity Service after the expiration.

### IPSIE Session Lifecycle SL2 - MFA, Logout, & Session Termination
Level SL2 adds the ability to communicate information about the user's authentication method between Identity Service and Application. The Identity Service includes claims about the authentication level in the assertion to the Application. The Application can request a specific authentication level of the Identity Service.

The Identity Services must be able to communicate a session termination event.  The Application must act upon session termination requests from the Identity Services.

### IPSIE Session Lifecycle SL3 - Continuous Access

Level SL3 adds continuous access to the authentication between Identity Service and application.

The app communicates session changes to the Identity Service such as IP address change, enabling the Identity Service to be aware of more context around what is happening to users' sessions after the initial sign-in.

The Identity Service communicates changes in the account and device posture to the application, enabling the application to take actions it determines are necessary based on its own policies about these changes.  Neither application nor identity services are obliged to act upon any state changes, the policies for responding to state changes are not in scope for SL3.

### IPSIE Identity Lifecycle Level IL1 - JIT User Provisioning Control

IPSIE Provisioning Level IL1 requires the Identity Service to provision users in the application when they log in via SSO. Users must not exist in the application prior to the user logging in for the first time, eliminating alternative pathways for user provisioning (e.g. self-provisioning).

### IPSIE Identity Lifecycle Level IL2 - User Pre-Provisioning and Deprovisioning Control 

Level IL2 adds the ability to provision and deprovision users in the application before they have logged in. Prior to level IL2, users were only JIT-provisioned in the application as part of SSO. An application at IL2 MUST support pre-provisioning and deprovisioning of users.  Identity Services and Apps at IL2 MAY support JIT provisioning for downward compatability with an Identity Service / Application operating at Level IL1.

Level IL2 also adds group provisioning and deprovisioning into applications. Applications MUST support group and group membership provisioning from the Identity Service, and use the groups to map to application roles.

### IPSIE Identity Lifecycle Level IL3 - Group and Group Membership Pre-Provisioning and Deprovisioning Control

Level IL3 enables the enterprise to manage application roles at the Identity Service. Applications MUST support exposing application roles to the Identity Service, so that the enterprise can assign application roles to users at the Identity Service. The Identity Service MUST include the application roles assigned to users in the user attributes during JIT and async provisioning. 




