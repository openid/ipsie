# IPSIE Levels

- *SL* - Session Lifecycle
- *IL* - Identity Lifecycle

Each level includes the previous level (_e.g._ SL3 includes the requirements of SL1 and SL2). SL* and IL* are independent of each other.

| IPSIE<br>LEVEL|   Application (aka RP)                                                 |  Identity Service                                                                                             |
|---------------|----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| SL1           |   - MUST meet NIST 800-63-4 FAL2 compliance* <br>- Application-specific session lifetime MUST be set from the assertion | - MUST meet NIST 800-63-4 FAL2 Compliance* <br> - MUST enforce MFA and communicate an authentication class to the Application |
| SL2           |  - MUST terminate sessions at the request of the Identity Service <br> - MUST not accept unsolicited federation assertions| - MUST enforce authentication method requests from Application |
| SL3           |  - MUST communicate session state changes to Identity Service | - MUST communicate user, session, and device state changes to the Application |
||||
| IL1           | - MUST support Create, Update and Delete of users by the Identity Service prior to sign-in <br>- Local Create, Update, and Delete of users and the Identity Service provided profiles SHALL NOT be allowed <br>| - MUST synchronize provisioned users and their profile data to the Application|
| IL2           |  - MUST support mapping group claims to application roles and capabilities | - MUST synchronize user group membership claims to Application |
| IL3           |  - MUST expose application roles to the Identity Service | - MUST consume Application roles and map to users<br> - MUST synchronize user role claims to Application |

-----
### IPSIE Session Lifecycle SL1 - Single Sign-On & Session Lifetime Controls

Level SL1 enables basic single sign-on from applications to the identity provider, communicating identity statements about the user. Single sign-on in Level SL1 meets the technical requirements of [FAL2 in NIST 800-63-4](https://pages.nist.gov/800-63-4/sp800-63c/fal/). 

***Note:** IPSIE does not include all of the controls specified in NIST 800-63-4 at FAL2.  IPSIE SL1 requires the technical controls from FAL2 which impact the security of the federation protocol(s).  Business agreements, such as data handling policies, are out of scope for IPSIE. 

The Application respects the session lifetime as communicated by the Identity Service in the assertion, and re-validates the session with the Identity Service after the expiration. Re-validation can occur with a new single sign-on flow, or using refresh tokens. It is likely that the session lifetime communicated by the Identity Service is shorter than the session at the Identity Service. The goal is to let the Identity Service set the interval in which the RP checks back at the Identity Service.

The Identity Service MUST communicate information about the user's authentication method at the Identity Service in the SSO assertion.

### IPSIE Session Lifecycle SL2 - MFA, Logout, & Session Termination

Level SL2 adds the ability for the Application to request specific authentication methods when the user logs in at the Identity Service.

Applications MUST NOT accept unsolicited federation requests from the identity service (e.g. SAML IdP initiated federation).

The Identity Services MUST be able to communicate a session termination event.  The Application MUST act upon session termination requests from the Identity Services.

### IPSIE Session Lifecycle SL3 - Continuous Access

Level SL3 adds continuous access to the authentication between Identity Service and Application.

The Application communicates session changes to the Identity Service such as IP address change, enabling the Identity Service to be aware of more context around what is happening to users' sessions after the initial sign-in.

The Identity Service communicates changes in the account and device posture to the application, enabling the application to take actions it determines are necessary based on its own policies about these changes.  Neither application nor identity services are obliged to act upon any state changes, the policies for responding to state changes are not in scope for SL3.

### IPSIE Identity Lifecycle Level IL1 - User and Profile Synchronization

IPSIE Lifecycle Level P1 requires the Identity Service to synchonize with the Application the users that have access and their profile data. The Application SHALL NOT independently create, update, or delete users, or the provided profile data, of users managed by Identity Services. While an Application may also support support Just In Time (JIT) for account creation using claims in an SSO token, JIT support is NOT a requirement of IPSIE.

### IPSIE Identity Lifecycle Level IL2 - User Group Membership 

Level P2 adds the ability for the Identity Service to organize users into groups and to synchonize group memberhip with the Application. The Application MUST use group membership to determine the roles and capabilities of the user.

### IPSIE Identity Lifecycle Level IL3 - User Roles

Level P3 adds the ability for the Application to publish the roles that exist in the Application to the Identity Service, and for the Identity Service to map these roles to users and synchronize with the Application which roles each user has.



