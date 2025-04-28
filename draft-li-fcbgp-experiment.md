---
title: "Forwarding Commitment BGP Testbed and Deployment Experience"
abbrev: "fcbgp-testbed-and-deployment-experience"
category: exp

docname: draft-li-fcbgp-experiment-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: "Routing"
# workgroup: sidrops
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "FCBGP/FCBGP-experiment"
  latest: "https://FCBGP.github.io/FCBGP-experiment/draft-li-fcbgp-experiment.html"

author:
  -
      fullname: Ke Xu
      org: Tsinghua University
      city: Beijing
      country: China
      email: xuke@tsinghua.edu.cn
  -
      fullname: Xiaoliang Wang
      org: Tsinghua University
      city: Beijing
      country: China
      email: wangxiaoliang0623@foxmail.com
  -
      fullname: Zhuotao Liu
      org: Tsinghua University
      city: Beijing
      country: China
      email: zhuotaoliu@tsinghua.edu.cn
  -
      fullname: Qi Li
      org: Tsinghua University
      city: Beijing
      country: China
      email: qli01@tsinghua.edu.cn
  -
      fullname: Jianping Wu
      org: Tsinghua University
      city: Beijing
      country: China
      email: jianping@cernet.edu.cn
  -
      fullname: Ziwei Li
      org: Zhongguancun Laboratory
      city: Beijing
      country: China
      email: lizw@zgclab.edu.cn
  -
      name: Yangfei Guo
      org: Zhongguancun Laboratory
      city: Beijing
      country: China
      email: guoyangfei@zgclab.edu.cn

normative:
  FC-BGP-Protocol: I-D.wang-sidrops-fcbgp-protocol-03

informative:
  RFC4272: # BGP Security Vulnerabilities Analysis
  RFC5065: # Autonomous System Confederations for BGP
  RFC5492: # Capabilities Advertisement with BGP-4
  RFC6472: # Recommendation for Not Using AS_SET and AS_CONFED_SET in BGP
  RFC6811: # BGP Prefix Origin Validation
  RFC7132: # Threat Model for BGP Path Security
  RFC7607: # Codification of AS 0 Processing
  RFC7908: # Route Leaks Definition and Classification
  FC-ARXIV:
    title: "Secure Inter-domain Routing and Forwarding via Verifiable Forwarding Commitments"
    date: Sep. 23, 2023
    target: https://arxiv.org/abs/2309.13271

--- abstract

Forwarding Commitment BGP（FC-BGP） is capable of establishing a secure inter-domain system that can simultaneously authenticate AS_PATH attribute in BGP-UPDATE and validate network forwarding on the dataplane. In an effort to enhance the validation of FC-BGP, a prototype implementation of the FC-BGP was developed and an evaluation was conducted on the FITI high-performance IPv6 backbone network. This document reports on the prototype implementation and the results of the evaluation.

--- middle

# Introduction

The current inter-domain, hop-by-hop forwarding model lacks end-to-end path validation, exposing a broad attack surface and enabling risks such as traffic interception, side-channel exploits, and man-in-the-middle attacks. FC-BGP is a novel forwarding-path verification framework designed to address these security challenges inherent in today’s inter-domain routing system. By introducing a Forwarding Commitment (FC), FC-BGP offers a solution that integrates seamlessly with the existing routing infrastructure. Through the use of FCs to provide a verifiable view of routing intent, FC-BGP ensures that the actual forwarding path conforms to declared routing policies, while avoiding the challenges of computational complexity, management overhead, and deployment cost associated with data-plane–based cryptographic schemes.

As part of the development of the Forwarding Commitment BGP (FC-BGP) framework, we implemented a FC-BGP prototype and deployed the prototype in the operational networks of 40 Autonomous Systems (ASes) within the Future Internet Technology Infrastructure (FITI). The evaluation demonstrates that FC-BGP achieves significant performance improvements in large-scale network deployments and that even limited deployment yields substantial security benefits. This document first describes a FC-BGP prototype solution, then outlines the experimental environment, and finally presents the experimental results. It is anticipated that this document will provide useful insights for those interested in this subject and serve as preliminary input for future IETF work in this area.



# Conventions and Definitions

{::boilerplate bcp14-tagged}


# A Prototype FC-BGP Implementation

~~~~~~
AS(65536) --> AS(65537) --> AS(65538)
                      \
                       \--> AS(65539)
~~~~~~
{: #fig-atta-ex title="An FC-BGP UPDATE propagation example."}

This section describes a prototype implementation of Forwarding Commitment BGP (FC-BGP), using a simplified topology as illustrated in {{fig-atta-ex}}. Only the essential operations are highlighted here; readers are referred to {{FC-BGP-Protocol}} for the complete protocol specification.

In this example, all Autonomous Systems (ASes) are standard ASes; Route Servers (RSes) and AS Confederations are not considered. Additionally, no AS Path Protection (ASPP) mechanisms are applied.

## FC-BGP UPDATE Propagation

For the purposes of discussion, it is assumed that:

1. AS 65537 receives an FC-BGP UPDATE message for the prefix 192.0.2.0/24 from AS 65536.
2. AS 65537 then propagates the route to AS 65538 and AS 65539.

An FC-BGP speaker SHOULD propagate an FC-BGP UPDATE message to its downstream neighbors only after completing validation of the Forwarding Commitments (FCs) and performing best path selection according to the standard BGP decision process.

## FC Generation at the Originating AS

When preparing to propagate a route, the FC-BGP speaker in AS 65536 performs the following steps:

1. Constructs the FC segment:

   Sets the Previous AS Number (PASN) to 0 (NULL).

   Sets the Current AS Number (CASN) to 65536.

   Sets the Next AS Number (NASN) to 65537.

2. Computes the signature over the tuple (PASN, CASN, NASN, IP Prefix Address, IP Prefix Length).
3. Constructs the FC Path attribute by embedding the newly generated FC segment.
4. Encapsulates the route announcement in a BGP UPDATE message and transmits it to AS 65537.

## FC Verification at the Receiving AS

Upon receiving the UPDATE message, the FC-BGP speaker in AS 65537:

1. Extracts the FC Path attribute.
2. Retrieves the list of FC segments.
3. Identifies the FC segment where CASN == 65536.
4. Verifies that:

   PASN == 0.

   NASN == 65537.

5. Retrieves the corresponding public key using the Subject Key Identifier (SKI) field.
6. Verifies the signature using the algorithm indicated by the Algorithm ID field.

If the signature matches, the AS hop from 65536 to 65537 is considered verified. This process repeats for each FC segment corresponding to the AS_PATH entries if applicable.

If AS 65537 does not support FC-BGP, its BGP speaker MUST propagate the UPDATE message without validating the FC Path attribute.

## Per-Neighbor UPDATE Generation

An FC-BGP speaker MUST generate a distinct UPDATE message for each downstream neighbor. Each UPDATE message MUST announce only a single IP prefix and MUST NOT aggregate multiple prefixes.

This restriction is necessary because different prefixes may follow different routing policies, resulting in different Forwarding Commitments. Aggregating prefixes could cause errors in FC generation and validation.

Thus, AS 65537 generates:

An UPDATE message for AS 65538 with NASN set to 65538.

A separate UPDATE message for AS 65539 with NASN set to 65539.

## FC Generation at Intermediate ASes

For each UPDATE to a downstream neighbor (e.g., AS 65538), the FC-BGP speaker in AS 65537:

1. Encapsulates the route prefix into a single UPDATE message.
2. Constructs a new FC segment:

   Computes the SHA-256 digest over (PASN, CASN, NASN, IP Prefix Address, IP Prefix Length).

   Signs the digest using ECDSA.

   Fills in the Signature and other required FC fields.

3. Prepends the newly created FC segment to the existing FC List.
4. Completes the FC Path attribute.
5. Continues with standard BGP UPDATE processing and transmission.

# FC-BGP Testbed


## Implementation Status

We implement the FC-BGP mechanism with FRR version 9.0.1. The implementation includes verifying the FC path attribute upon receiving BGP UPDATE messages and adding and signing the FC path attribute when sending BGP UPDATE messages. The development and testing of this implementation were conducted on Ubuntu 22.04 with OpenSSL 3.X installed.

GitHub repository: https://github.com/fcbgp/fcbgp-impl

## FITI

The prototypes of our solutions for FC-BGP are implemented and tested on FITI (Future Internet Technology Infrastructure). FITI is a major Chinese science and technology infrastructure investment, which is now under construction and operation by 40 universities, including the National Development and Reform Commission, the Ministry of Education and Tsinghua University. FITI holds 4096 Autonomous System Numbers (ASNs) assigned by APNIC, along with a 240a:a000::/20 IPv6 address block. With sites distributed across China, it features a genuine internet environment suitable for large-scale network experimentation. FITI supports a maximum bandwidth of up to 1.2 Tbps.

## FC-BGP Testbed on FITI Infrastructure

~~~~~~
        +------------+           +------------+
        |   Beijing  |           |  Shanghai  |
        | +--------+ |           | +--------+ |
        | | FC-BGP | |    ...    | | FC-BGP | |
        | +--------+ |           | +--------+ |
        +------------+           +------------+
              |                        |
        +-------------------------------------+
        |                                     |
        |                FITI                 |
        |              Backbone               |
        |                                     |
        +-------------------------------------+
              |                        |
        +------------+           +------------+
        |   Nanjing  |           |  Shenzhen  |
        | +--------+ |           | +--------+ |
        | | FC-BGP | |    ...    | | FC-BGP | |
        | +--------+ |           | +--------+ |
        +------------+           +------------+
~~~~~~
{: #fig-rs-x title="FC-BGP Testbed on FITI Infrastructure."}

# Test Experience and Results

Utilizing public datasets from RouteViews, we performed Internet BGP topology reconstruction and BGP announcement replay on the Testbed. Simultaneously, we adjusted the deployment ratio of FC-BGP to calculate the network-wide route hijackability rate under partial deployment scenarios as the deployment rate increases.

Given a deployment rate r, we randomly select the r% ASes to upgrade any one of FC-BGP and BGPsec. We randomly select two ASes, A as the victim and B as the attacker. AS B initiates origin prefix hijacking of AS A to all other ASes in the topology and computes the proportion of ASes that can be hijacked denoted as p. This process is repeated 10,000 times, and the average value of p is computed as the origin hijacking rate at the current deployment rate r. We obtained the experimental result: With a deployment rate of only 10%, over 70% of Internet BGP paths can be protected.

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
