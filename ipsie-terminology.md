# IPSIE Terminology

### Enterprise Company

Ultimately, the goal of the IPSIE standard is to better serve the "Enterprise Company". Enterprise companies prefer to centralize the authentication process of all the applications used across the enterprise. Among other benefits, this allows users to have only one set of credentials to manage, and enables the company to manage which users can access which applications in a central location.

The enterprise company is also referred to as the "customer", since they may be a customer of the Identity Provider and multiple Applications.


### Identity Provider

The "Identity Provider", abbreviated IdP, is used by the enterprise to manage users within their organization. The Identity Provider is often purchased by the enterprise company as a service, but can also be open source software or developed in-house.

The identity provider is where the users' access to applications and other resources is managed and enforced.


### Application

The "Application" is ultimately used by people within the enterprise company during their day to day work. Applications have their own resources, and users may be limited in which applications they can access or what they can do within an application. Applications use the Identity Provider to authenticate users through a "single sign-on" process.

Applications may also be referred to as the "Relying Party" (RP) during a single sign-on flow.

Applications may be purchased by the enterprise company as a service from the application vendor, in which case they are commonly referred to "Software as a Service" (SaaS).


### User

The "User" is part of the workforce of the enterprise company, and is able to use the identity provider to log in to applications.


