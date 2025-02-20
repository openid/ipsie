# IPSIE Levels

- *SL* - Session Lifecycle
- *IL* - Identity Lifecycle

Each level includes the previous level (_e.g._ SL3 includes the requirements of SL1 and SL2). SL* and IL* are independent of each other.

| IPSIE<br>LEVEL|   Application (aka RP)                                                 |  Identity Service                                                                                             |
|---------------|----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| SL1           |   - MUST meet NIST800-63rev3 FAL2 compliance <br>- Session lifetime MUST match assertion lifetime | - MUST meet NIST800-63rev3 FAL2 Compliance <br> - MUST enforce MFA and communicate an authentication class to the Application |
| SL2           |  - MUST terminate sessions at the request of the Identity Service| - MUST enforce authentication method requests from Application |
| SL3           |  - MUST communicate session state changes to Identity Service | - MUST communicate user, session, and device state changes to the Application |
||||
| IL1           | - Local Create, Update, and Delete of users and the Identity Servive provided profiles SHALL NOT be allowed <br>| - MUST synchronize provisioned users and their profile data to the Application|
| IL2           |  - MUST support mapping group claims to application roles and capabilities | - MUST synchronize user group membership claims to Application |
| IL3           |  - MUST expose application roles to the Identity Service | - MUST consume Application roles and map to users<br> - MUST synchronize user role claims to Application |

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

### IPSIE Identity Lifecycle Level IL1 - User and Profile Synchronization

IPSIE Lifecycle Level P1 requires the Identity Service to synchonize with the Application the users that have access to the and their profile data. The Application SHALL NOT independently create, update, or delete users or the provided profile data.

### IPSIE Identity Lifecycle Level IL2 - User Group Membership 

Level P2 adds the ability for the Identity Service to organize users into groups and to synchonize group memberhip with the Application. The Application MUST use group membership to determine the roles and capabilities of the user.

### IPSIE Identity Lifecycle Level IL3 - User Roles

Level P3 adds the ability for the Application to publish the roles that a user may have at the Application, and for the Identity Service to synchronize with the Application which roles each user has.



