# IPSIE Terminology

## Roles

### Enterprise

Enterprises are generally defined as entities - e.g. corportations, non-profit organizations, partnerships.  Enterprises have a workforce comprised of employees, contractors, volunteers, and others who operate on behalf of the enterprise.  Enterprises deploy applications and services to support their organizational needs.  Government, non-governmental organizations, educational entities, small businesses, and others may consider themselves enterprises.

Ultimately, the goal of the IPSIE standard is to better serve the enterprise organization. Enterprises prefer to centralize the authentication process of all the applications used across the enterprise. Among other benefits, this allows users to have only one set of credentials to manage, and enables the company to manage which users can access which applications in a central location. In this framing, enterprises have identity services, applications, data, and the policies that are used to grant, suspend, and revoke user access.

The enterprise company is also referred to as the "customer", since they may be a customer of the Identity Service provider(s) and multiple Applications.

### Identity Service

The enterprise's "Identity Service" the logical set of services used by the enterprise to manage users within their organization. The Identity Service is often purchased by the enterprise company as a service or set of services, but can also be open source software or developed in-house.  The identity service may be a single component, or multiple components with discrete functionality.

The identity service is where the users' access to applications and other resources is managed and enforced.


### Identity CRUD Implementation Concepts
To understand and explain Identity CRUD implementations we need to define the terminology by defining the following concepts: Identity CRUD Data Models, Identity CRUD Protocol Roles, Identity CRUD Orchestrator Roles, Identity CRUD Triggers, and Identity CRUD Actions.

#### Identity CRUD Data Models
Identities are defined by two types of data entities: Resources and Attributes.
##### Resource Object (RO)
An object representing a user, group, or an extension object like devices, used by CRUD operations. The Resource Object contains attributes defined by schemas.
##### Resource Attribute (RA)
A named element of a Resource Object (RO). It includes characteristics like cardinality (single or multiple values), data types (string, boolean, binary, etc.), and properties (required, unique, etc.).

#### Identity CRUD Orchestrator Roles
Orchestrators are the operating parties that facilitate the exchange of data and ensure it flows correctly. Identity entities can have one or more orchestrator roles, depending on the overall architecture.
##### Resource Creator (RC)
An entity responsible for creating the Resource Object (RO). Typically, this role is found in HR or Resource Management (RM) applications that create resources and their attributes.
##### Resource Updater (RU)
An entity responsible for updating specific Resource Attributes (RA) of a Resource Object (RO) or the RO itself. This role is often used in conjunction with other roles that allow the entity to manage specific Resource Attributes (RA) and/or Resource Objects (RO).
##### Resource Manager (RM)
An entity that aggregates or transforms Resource Objects (RO) from resource creators/updaters (RC/RU) and makes them available for Resource Subscribers (RS) through multiple interactions. An example of this role could be an Identity-as-a-Service (IDaaS) cloud service.
##### Resource Subscriber (RS)
An entity that consumes Resource Objects (RO) and typically doesn't create new objects or attributes. An example would be a SaaS application that delivers a service and needs to create a database of objects, sourcing them from an RM/RC/RU.

#### Identity CRUD Triggers
Triggers are activities that may cause a CRUD action to occur. Triggers can result from business processes like a corporate hiring event, scheduled events such as a Unix bash script running as a cron job, or SSO just-in-time events arriving at a federated relying party that identifies a previously unseen user. Triggers can also be standardized events, such as those in the OpenID Shared Signals Framework. They are used to initiate CRUD (Create, Read, Update, Delete) operations.
##### Periodic Intervals
A periodic interval trigger is a pre-configured agreement where an action occurs at a specific time. This trigger is often recurring and typically initiates an action. An example of a periodic interval trigger could be a UNIX cron job executing a script.
##### Events
Event triggers are activities, contexts, or notifications that could happen at any time. Actions could also be triggered by a Security Event Token (SET) as described in [RFC8417].
##### Application Triggers
Application triggers occur when administrative or end-user interfaces are manipulated. An example of an application trigger might be a user modifying their profile information. Another example could be an Identity Administrator creating a new user in the Identity Management (IdM) system who immediately wants to update a SaaS application.
##### SSO (Single Sign-On)
Single Sign-On triggers occur when a user authenticates via federated protocols such as SAML 2.0 or OpenID Connect. If a federated assertion arrives for a user who has not yet been provisioned into the destination application, the application may be triggered to perform just-in-time (JIT) provisioning. This trigger occurs in scenarios where a Single Sign-On flow happens, but not all the resource attributes for the user object are passed in the federated assertion, necessitating an additional protocol to push or pull the remaining needed attributes.

#### Identity CRUD Actions
The protocols that define interactions between two standardized parties that adhere to HTTP RESTful conventions. It enables CRUD operations by mapping these activities to HTTP verbs such as POST, PUT, GET, DELETE, etc. An identity entity can have multiple roles depending on the objective of the use case being described.

### Application

The "Application" is ultimately used by people within the enterprise company during their day to day work. Applications have their own resources, and users may be limited in which applications they can access or what they can do within an application. Applications use the Identity Service to authenticate users through a "single sign-on" process.  Users and entitlements are provisioned to applications through the identity service.

Applications may be purchased by the enterprise company as a service from the application vendor, in which case they are commonly referred to "Software as a Service" (SaaS).

### User

The "User" is part of the workforce of the enterprise company, and is able to use the Identity Service to log in to applications.  

## Terminology

### Workforce

The people engaged in work for a particular organization. Employees, contractors, and university faculty and staff are common examples of these people within a workforce.

### Tenant

In a multitenant system, a tenant represents an organization or group of consumers, such as partners, customers, business units, departments, projects, or environments (e.g., production or test) within a larger entity. Each tenant has users and resources assigned to them, and only these users can access the resources within their specific tenant. Tenancy can be hierarchical, allowing a parent tenant to have sub-tenants, with resources potentially shared among them.

Isolation between tenants varies along a spectrum based on business needs. At one end, physical resources are shared by trusted parties with minimal disruption, while at the other end, tenants are completely unaware of each other and may have dedicated resources. High-end solutions ensure full privacy between untrusted parties, whereas lower-end solutions allow for reduced isolation among trusted parties sharing a system.

### User Group

A group is a collection of users that typically share a common set of permissions or access rights. Groups are used to simplify access management by allowing administrators to assign permissions to multiple users at once, based on their membership in the group. This makes it easier to manage and maintain consistent access controls across an organization.

### User Provisioning

User provisioning is the process of creating, updating, and managing user accounts.

#### Just-In-Time (JIT) Provisioning

A method of user provisioning that automatically creates and manages user accounts on-demand, typically as users attempt to access resources for the first time. JIT provisioning reduces the need for manual account creation and management, and helps ensure that users have access to the resources they need when they need them.

### User Sign-on

Sign-on is the process by which a user gains access to a computer system, network, or application by providing valid credentials, typically a username and password combination. The sign-on process is a crucial component of Identity and Access Management (IAM), as it helps to verify the user's identity and ensure that only authorized individuals can access protected resources. Sign-on is often the first step in the authentication and authorization process, which is followed by granting or denying access to specific resources based on the user's assigned permissions and roles. Additionally, sign-on mechanisms can be enhanced with various security measures, such as multi-factor authentication (MFA) or single sign-on (SSO), to provide an extra layer of protection against unauthorized access or potential security threats.

### Single sign-on (SSO)

A user authentication process that allows users to access multiple applications, services, or systems with a single set of credentials. Single Sign-On simplifies the user experience by eliminating the need for users to remember and enter multiple sets of credentials for different resources. SSO can be implemented using various protocols, such as SAML and OpenID Connect.

### Multi-Factor Authentication (MFA)

An authentication method that requires users to provide more than one independent factors to verify their identity. MFA typically combines something the user knows (e.g., password), something the user has (e.g., smartphone), and/or something the user is (e.g., fingerprint). MFA enhances security by making it more difficult for unauthorized individuals to access accounts, even if they have obtained the user's password.

MFA mechanisms vary greatly in their security properties.  Enterprises should choose authentication mechanisms, including MFA, which meet their authentication assurance needs.  [NIST SP800-63 B](https://pages.nist.gov/800-63-3/sp800-63b.html) provides a useful resource for enterprises to understand an academic view of authentication assurance.  Under the SP800-63 B, multi-factor authentication is classified as [Authentication Assurance Level 2](https://pages.nist.gov/800-63-3/sp800-63b.html#sec4) (AAL2).  AAL2 is generally considered the minimum bar for authentication to sensitive enterprise resources.  AAL1 may be considered for non-sensitive data such as access to the corporate cafeteria lunch menu.

### Governance

In a general context, refers to the process of establishing and implementing policies, procedures, and frameworks to effectively manage and control an organization or system. In the context of Identity and Access Management (IAM), governance involves overseeing the use of digital identities and their access to resources across an organization. This includes setting standards, guidelines, and best practices for managing identities and access, as well as defining roles and responsibilities for IAM stakeholders, such as administrators, users, and auditors.

### Phishing Resistant Authentication

Phishing resistant authentication mechanisms are those which effectively prevent remote account access through relayed secrets or assertion. These mechanisms rely on verifier impersonation resistance, achieved through techniques like [verifier name binding](FUTURE LINK) or [channel binding](FUTURE LINK), which block malicious services from impersonating legitimate verifiers. Examples of phishing-resistant authentication mechanisms include FIDO2/WebAuthn and mutual TLS.

## Protocols

A protocol is a set of rules and guidelines for communicating and exchanging data between different systems or entities. IAM protocols define standardized methods for user authentication, authorization, and access management, and enable different applications and services to interoperate and share identity information. Some of the commonly used IAM protocols include SAML (Security Assertion Markup Language), OAuth (Open Authorization), OpenID Connect, and SCIM (System for Cross-domain Identity Management). These protocols provide a common language and framework for implementing IAM solutions, and help ensure interoperability and compatibility across different systems and applications.

### Continuous Access Evaluation Protocol (CAEP)

TBD

### OAuth 2.0

OAuth 2.0 is a widely-used authorization (authZ) protocol that enables both first-party and third-party applications to obtain limited access to user resources without exposing user credentials. It is a cornerstone of modern Identity and Access Management (IAM) systems, facilitating secure interactions between users and applications. OAuth 2.0 operates on a token-based mechanism, where an authorization server issues an access token to an application after the user consents. This token allows the application to access specific resources on the user's behalf, such as social media profiles or cloud storage, without requiring the user to share their login details directly with the application. OAuth 2.0 supports a variety of deployment models, making it versatile for different use cases and environments.

### OpenID Connect (OIDC)

A protocol built on top of the OAuth 2.0 framework of specification (IETF RFC 6749 and 6750), used for authentication purposes. OIDC allows for the exchange of identity information between parties, such as an Identity Service and a relying party (e.g., a web application). It is designed to provide single sign-on (SSO) capabilities, as well as support for user authentication using JSON Web Tokens (JWTs). OIDC is widely used in modern IAM solutions and is supported by many popular Identity Services and service providers.

### SAML

Security Assertion Markup Language (SAML) is an XML-based protocol used for securely exchanging authentication and authorization information between different systems. It enables single sign-on (SSO) by allowing users to authenticate once and gain access to multiple applications or services without needing to log in separately to each one. SAML facilitates interoperability between identity providers and service providers, ensuring that user credentials are not directly shared between systems.

### Risk Information Sharing & Collaboration

TBD

### SAML

TBD

### Shared Signals Framework (SSF)

TBD

### System for Cross-domain Identity Management (SCIM)

A standard protocol used for automating the exchange of user identity and access information between different systems or domains. SCIM is designed to simplify the management of digital identities and reduce the need for manual provisioning and deprovisioning of users across multiple systems. It enables organizations to automate user onboarding and offboarding, enforce consistent access policies, and reduce the risk of errors and inconsistencies. SCIM is widely used in IAM solutions, especially in cloud-based environments where multiple applications and services need to share identity information.



