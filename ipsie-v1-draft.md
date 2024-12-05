## Framing

As a developer building a new business application, which will be used by many organizations (also known as a B2B SaaS application), I need to:

* set up user and group provisioning and deprovisioning between a customer's workforce IdP and my application
* set up user authentication via federated relationship with a customer's workforce IdP
* ensure end users only have access to what they need in my application at any given point in time
* be able to convey to the customer's IdP that I require a certain authentication level, require step-up authentication, or require re-authentication (e.g. due to policy enforcement at the RP)
* know whether that authentication level was met at the IdP during a sign-in
* be able to convey to the customer's IDP that I require a certain identity verification level and/or require additional identity attributes (e.g. due to policy enforcement at the RP)
* know whether the requested identity verification was met at the IDP
* be notified when tokens have been revoked
* be notified when sessions have been invalidated
* receive real-time signals about changes in account posture or integrity
* receive real-time signals about changes in the device posture or integrity.
* receive enterprise required policy that the new business application must enforce (e.g. identities authenticated by the enterprise IDP MUST NOT access the RP via a VPN)
* only enforce the enterprise defined policy for users authenticated by the enterprise IDP

To make that happen, I need to know:

* which protocols I should use
* how to securely implement and deploy those protocols at scale
* how to implement those protocols in an interoperable manner

---

## User Authentication via Workforce IdP

> set up user authentication via federated relationship with a customer's workforce IdP

## User Provisioning and Deprovisioning

> set up user provisioning and deprovisioning between a customer's workforce IdP and my application

## Group Provisioning and Deprovisioning

> set up group provisioning and deprovisioning between a customer's workforce IdP and my application

## Ensure Least Privileged Access

> ensure end users only have access to what they need in my application at any given point in time

## Convey Required Authentication Level to the IdP

> be able to convey to the customer's IdP that I require a certain authentication level, request step-up, or request re-authentication of the user via the customer's IdP

## Know whether Required Authentication Level was Met at the IdP

> know whether that authentication level was met at the IdP during a sign-in, step-up, or re-authentication event

## Receive Notifications of Revoked Tokens

> be notified when tokens have been revoked

## Receive Notifications of Invalidated Sessions

> be notified when sessions have been invalidated

## Receive Signals about Account Posture or Integrity Changes

> receive real-time signals about changes in account posture or integrity

## Receive Signals about Device Posture or Integrity Changes

> receive real-time signals about changes in the device posture or integrity (e.g. from device management services, EDR, etc.)
