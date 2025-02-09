---
title: Network Attestation for Secure Routing (NASR) Architecture
abbrev: NASR Architecture
category: info

docname: draft-liu-nasr-architecture-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: AREA
# workgroup: WG Working Group
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "liuchunchi/nasr-architecture"
  latest: "https://liuchunchi.github.io/nasr-architecture/draft-liu-nasr-architecture.html"

author:
 -
    fullname: "Chunchi Liu"
    organization: Your Organization Here
    email: "liuchunchi@huawei.com"

normative:

informative:
  RFC9344:
  I-D.liu-nasr-requirements-01: NASRREQ

--- abstract

This document provides an architecture overview of NASR entities, interactive procedures and messages.


--- middle

# Introduction

Endpoints typically perceives no information of the properties of the paths over which their traffic is carried, especially when the properties are security-related. Therefore, data security (confidentiality, integrity and authenticity) has been insofar primarily protected by traffic signing and encryption mechanisms.

However, clients with high security and privacy requirements are not anymore satisfied with signing and encryption mechanisms only; they now request information of the trustworthiness or security properties of the network paths over which the traffic is carried, preferably to choose the desired properties. For example, some clients may require their data to traverse through trusted devices and trusted links only, in order to avoid data being exposed to insecure devices, causing leakage.

Remote Attestation Procedures (RATS) working group developed procedures to establish a level of confidence in the trustworthiness of a device or a system. By providing objective, verifiable evidence and proof of a device’s trust or security properties, clients can expect the device function correctly and deterministically, as anticipated.

Network Attestation for Secure Routing (NASR) aims to create a robust system that provides objective, verifiable evidence of network path properties that can be used by routing technologies to steer traffic towards. Following the same RATS philosophy and building on top of it, NASR also aim to provide objective, verifiable evidence and proof of trust or security properties, but on a path level. Additionally, NASR also aim to provide verifiable proof that certain packets/flows traveled such paths. With NASR, clients can have the ability to be aware of, request and verify specific path properties, and have confidence that certain packets or flow indeed traversed the requested path.

This document introduces the architecture, entities, interactive procedures, and messages sent between the entities, so to achieve the NASR objective.

# Use Cases {#usecases}

Please refer to the use cases identified in {{-NASRREQ}}

# Architectural Overview
                                                                               
      ┌────────────────────┐                                                              
      │                    │                                                              
      │    Relying Party   │                                                              
      │                    │                                                              
      │                    │                                                              
      └────┬─────────▲─────┘                                                              
    Path   │         │                                                                    
    Request│         │ Report                                                             
           │         │                                                                    
      ┌────▼─────────┴─────┐                                    ┌────────────────────┐    
      │                    │     Path Attestation Result (PAR)  │                    │    
      │    Orchestrator    │                                    │      Verifier      │    
      │                    ◄────────────────────────────────────┤                    │    
      │                    │                                    │                    │    
      └────┬───────────────┘                                    └──────────▲─────────┘    
           │                                                               │              
  Path     │                                                               │  Path        
  Evidence │                                                               │  Evidence    
  (PE)     │                                                               │  (PE)        
      ┌────▼───────────────┐       ┌────────────────────┐       ┌──────────┴─────────┐    
      │                    │       │                    │       │                    │    
      │      Attester      ├───────►     Attester...    ├───────►      Attester      │    
      │                    │       │                    │       │                    │    
      └────────────────────┘       └────────────────────┘       └────────────────────┘    
                        Update PE with                Update PE with                      
                          AR/RE/PoT                     AR/RE/PoT                         

[NASR Architecture](https://docs.google.com/presentation/d/1sT2jn8YfgDE7KXhTuAgPBdlWDbxV2uZ_Z78SkMs0HNk/edit?usp=sharing)

[ASCII Diagram](https://asciiflow.com/#/share/eJzVlb1OwzAQx1%2FF8tRKVSoxAO3WIStE5mPKYrWGRgpJcV1oVFVCFSMDQwR9DkbE0%2BRJcNIo35%2B1g8TJgyNffnfJ3f29gRZ%2BIHAMLyZXCA6giR1C%2BeNGh2sdjkeno4EOHb47OT%2FjO0bWjD%2FoEHDz3B%2FBBURM160wi6JTuXRETMew7oGGKXOk07P2X%2Bgt1sdXF9XXMJtnvkiUm6Aj8rgiS5alI7KwKROmJ4hFe2F6rgKdTKfnvnnui8B6B037NSj2hDFeEswM2%2BKFWK5MBnraBPV9t13x27tchEs6nXMKxcymFYGLUdxuCTXuDELLI%2BRe%2FXwVV0vh4jXKtOW%2FqI7wB%2FeD57o1PeZrT4XDHpRP5HGWJXB6qFTS6BEvoKtPxoxYUyKLHvECek9T%2B%2FGZOD3iJTolrSK%2BVrVWkZAvp%2BUa6Fp1jildq5iViv%2BazKbJtIU%2BB4mM1ansRv5OeSuK0sQ9Ry%2FKoOPvrJv3mrWPw8rqFQkZJRUoZTeLGWYEaCp4NqKZb3gaWCmZFxMNkTrU7OsjTn0y3MLtL2JpUd8%3D))


# Roles {#roles}

The existing roles from RATS Architecture document {{RFC9344}} applies. New role(s) are defined below.

Orchestrator: A role performed by an entity (typically a controller or a special device) that receives a Path Request from the Relying Party, send unfilled Path Evidence (PE) inquiry to Attesters, collects Path Attestation Result (PAR) from the Verifier, and send PAR back to the Relying Party.

    Consumes: Path Request, Path Attestation Result

    Produces: (unfilled) Path Evidence 

Verifier: A role performed by an entity that appraises the validity of filled Path Evidence about a set of Attesters and produces Path Attestation Results to be used by an Orchestrator.

    Consumes: (filled) Path Evidence

    Produces: Path Attestation Results

# Conceptual Messages {#messages}

The existing artifacts from RATS Architecture document {{RFC9344}} applies. New conceptual message(s) are defined here. 

Path Request: A set of claims, describing the properties of a network path that a Relying Party requested.

    Consumed By: Orchestrator

    Produced By: Relying Party

Path Attestation Result: The output generated by a Verifier, including information about a set of Attesters, where the Verifier vouches for the validity of the results. 

    Consumed By: Relying Party

    Produced By: Verifier

Path Evidence: The output generated by the Orchestrator and a set of Attesters, to be appraised by a Verifier. Path Evidence may include each Attester's raw Evidence {{RFC9344}}, Attestation Results, Proof-of-Transit, or other proof suggesting correctness of functioning of each Attester. 

    Consumed By: Verifier

    Created By: Orchestrator

    Updated By: Attester(s)


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
