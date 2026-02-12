## January 2026 Interop Proposal

Based on feedback that the sequential approach isn't compelling enough for CISO audiences, we propose a revised interoperability event that demonstrates the full vision of IPSIE while providing practical value to participants.

In addition to testing basic SL1 features to enable federation, participants are asked to bring their existing IdPs and RPs to demonstrate the security capabilities aligned with [IPSIE SL2 and SL3](https://github.com/openid/ipsie/blob/main/ipsie-levels.md). This "capability-first" approach allows vendors to showcase existing implementations while identifying gaps toward full IPSIE compliance.

### Core Objectives

- **Demonstrate Security Improvements**: Show enterprise security leaders what IPSIE can achieve at maturity (SL3) rather than just basic federation. Provide a roadmap to the end-state goals of the IPSIE WG.
- **Identify Market Readiness**: Survey current capability distribution across participating vendors (IdPs & RPs) for advanced capabilities at SL2/SL3.
- **Document Integration Gaps**: Create a list of integration gaps for pairwise tests of IdPs and RPs.  Enable vendors to work toward improved capabilities, spinning the flywheel to drive market demand. 
- **Protocol Convergence**: Let implementation patterns guide IPSIE WG's future work to profile specifications for SL2/3.

### Capability Testing

Participants are encouraged demonstrate specific capabilities beyond SL1 in a pairwise fashion (IdP ↔ RP). The interop rules shall not specify the protocol mechanism used to achieve these capabilities.  For each capability test, the participants will document:

- Which capability was demonstrated (e.g. RP session termination at IdP request)
- Which protocols/standards were used (if any, including draft standards such as OP Commands)
- Name of the IdP
- Name of the RP application(s)
- Integration requirements (e.g. configuration)
- How was the capability tested?
- What were the results of testing?
- Logs, configurations, and video (if applicable) to demonstrate the testing

As the interop approaches, we'll provide additional details on the execution of the capability testing documentation.


| IPSIE<br>LEVEL|   Application (aka RP)                                                 |  Identity Service                                                                                             |
|---------------|----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| SL2           |  - MUST terminate sessions at the request of the Identity Service <br> - MUST not accept unsolicited federation assertions| - MUST enforce authentication method requests from Application |
| SL3           |  - MUST communicate session state changes to Identity Service | - MUST communicate user, session, and device state changes to the Application |

