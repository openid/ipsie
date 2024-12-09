## Framing

As a developer building a new business application, which will be used by many organizations (also known as a B2B SaaS application), I need to:

* set up user and group provisioning and deprovisioning between a customer's workforce IdP and my application
* set up user authentication via federated relationship with a customer's workforce IdP
* ensure end users only have access to what they need in my application at any given point in time
* be able to convey to the customer's IdP that I require a certain authentication level, require step-up authentication, or require re-authentication (e.g. due to policy enforcement at the RP)
* know whether that authentication level was met at the IdP during a sign-in
* be notified when tokens have been revoked
* be notified when sessions have been invalidated
* receive real-time signals about changes in account posture or integrity
* receive real-time signals about changes in the device posture or integrity.

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

---

### **1. Authentication**
1. **As a B2B SaaS developer, I want to set up user authentication via a federated relationship with a customer's IdP, so that end users can log in using their enterprise credentials.**
2. **As a B2B SaaS developer, I want to enable each enterprise tenant to configure their own IdP(s) for authentication, so that multi-tenant customers can manage their users independently.**
3. **As a B2B SaaS developer, I want to convey to the customer's IdP that the application requires a specific authentication level, so that access policies like MFA can be enforced.**
4. **As a B2B SaaS developer, I want to know whether the specified authentication level was met at the IdP during sign-in, so that I can ensure compliance with security requirements.**
5. **As a B2B SaaS developer, I need to bind the customer's uuid and IdP entity to an internal UUID, so that accidental user impersonation cannot happen when multiple customer IdPs are configured.**
   
---

### **2. Authorization**
1. **As a B2B SaaS developer, I want to ensure end users only have access to what they need in my application at any given point in time, so that I maintain the principle of least privilege.**
2. **As a B2B SaaS developer, I want to allow enterprises to dynamically assign roles based on user attributes, so that access permissions stay aligned with organizational changes.**
3. **As a B2B SaaS developer, I want to issue tokens scoped to specific permissions so that users or services have only the access they need.**
4. **As a B2B SaaS developer, I want to allow enterprises to dynamically assign policies to specific permissions, so that enterprises have flexibility in assignment of access.**

---

### **3. User and Group Lifecycle Management**
1. **As a B2B SaaS developer, I want to set up user provisioning and de-provisioning between a customer's workforce IdP and my application, so that user accounts are automatically managed.**
2. **As a B2B SaaS developer, I want to set up group provisioning and de-provisioning between a customer's workforce IdP and my application, so that user group memberships are synchronized.**
3. **As a B2B SaaS developer, I want to provision user accounts dynamically upon successful authentication, so that onboarding is seamless.**
4. **As a B2B SaaS developer, I want to automatically deactivate user accounts when permissions change or users leave the enterprise, so that access remains secure.**
5. **As a B2B SaaS developer, I want to allow enterprises to define and assign their own access policies, so that they can dynamically allow just-in-time access to my application's permissions.**

---

### **4. Identity Federation**
1. **As a B2B SaaS developer, I want to automate the exchange and update of federation metadata with enterprise IdPs, so that customers experience seamless integrations.**
2. **As a B2B SaaS developer, I want to support multi-IdP configurations per tenant, so that enterprises with complex structures can connect multiple directories.**
3. **As a B2B SaaS developer, I want to provide tools to debug and monitor federation flows, so that enterprise clients can troubleshoot issues effectively.**
4. **As a B2B SaaS developer, I want to advertise what entity categories (or domains and subject areas) of attributes I support, so that enterprise clients can more easily integrate their standard attributes and groups into my application.**
5. **As a B2B SaaS developer, I want to define a data catalog with element names, allowed values, functional descriptions, data types, data use, and versioning, so that enterprise clients can more easily integrate their standards into the system.**

---

### **5. Risk Signal Sharing and Posture Updates**
1. **As a B2B SaaS developer, I want to receive real-time signals about changes in account posture or integrity, so that I can take action to protect the application.**
2. **As a B2B SaaS developer, I want to support conditional access policies based on real-time risk signals, so that enterprises can enforce dynamic access controls.**
3. **As a B2B SaaS developer, I want to send real-time signals about changes in account posture or integrity, so that my enterprise customers can take action to protect their data.**

---

### **6. Session Management**
1. **As a B2B SaaS developer, I want to be notified when tokens have been revoked, so that I can terminate access for affected sessions.**
2. **As a B2B SaaS developer, I want to be notified when sessions have been invalidated, so that I can ensure the application enforces session termination.**

---

### **7. Protocols and Standards**
1. **As a B2B SaaS developer, I want to identify which protocols (e.g., SAML, SCIM, OIDC, OAuth 2.0) I should use, so that I meet enterprise customer requirements.**
2. **As a B2B SaaS developer, I want to securely implement and deploy these protocols at scale through community-driven SDKs, so that my application remains reliable and secure without becoming an identity expert.**
3. **As a B2B SaaS developer, I want to implement protocols in an interoperable manner, so that my application works with diverse enterprise IdP configurations.**

---

### **8. Security and Compliance**
1. **As a B2B SaaS developer, I want to provide APIs to revoke tokens and sessions, so that enterprises can respond to incidents immediately.**
2. **As a B2B SaaS developer, I want to log and report IAM-related activities, so that enterprise customers can meet their compliance requirements.**
3. **As a B2B SaaS developer, I want to allow my enterprise customers to pull logs from my application into their enterprise SIEM, so that the can meet their compliance requirements.**
4. **As a B2B SaaS developer, I want to ensure my application and enterprise customers can support multiple signing and encryption keys, so we can independently rotate them without major operational impacts.**

---

### **9. Tenant Management and Customization**
1. **As a B2B SaaS developer, I want to ensure data and configurations are securely isolated between tenants, so that enterprise customers can operate securely.**
2. **As a B2B SaaS developer, I want to protect data and configurations with customer-managed keys so that enterprise customers can operate securely.**
3. **As a B2B SaaS developer, I want to enable enterprise clients to customize the application interface and login page with their branding, so that the application aligns with their identity.**
4. **As a B2B SaaS developer, I want to allow enterprises to define and enforce unique IAM policies for their users, so that they can meet their security requirements.**

---

### **10. Developer and Client Support**
1. **As a B2B SaaS developer, I want to consume SDKs and APIs to simplify IAM integrations, so that enterprise clients can integrate with my application quickly.**
2. **As a B2B SaaS developer, I want to offer a sandbox environment for testing IAM configurations, so that clients can validate their setups before going live.**
3. **As a B2B SaaS developer, I want to consume sandbox environments for testing IAM configurations, so that I can validate their setups before going live.**
4. **As a B2B SaaS developer, I want to provide clear error messages and debugging tools, so that clients can troubleshoot integration issues quickly.**

---
