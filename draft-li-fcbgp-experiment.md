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
  FC-BGP Protocol Specification: draft-wang-sidrops-fcbgp-protocol-03
  
informative:
  RFC4272: # BGP Security Vulnerabilities Analysis
  RFC5065: # Autonomous System Confederations for BGP
  RFC5492: # Capabilities Advertisement with BGP-4
  RFC6472: # Recommendation for Not Using AS_SET and AS_CONFED_SET in BGP
  RFC6811: # BGP Prefix Origin Validation
  RFC7132: # Threat Model for BGP Path Security
  RFC7607: # Codification of AS 0 Processing
  RFC7908: # Route Leaks Definition and Classification
  Deprecation-AS_SET-AS_CONFED_SET: I-D.ietf-idr-deprecate-as-set-confed-set # Deprecation of AS_SET and AS_CONFED_SET in BGP
  FC-ARXIV:
    title: "Secure Inter-domain Routing and Forwarding via Verifiable Forwarding Commitments"
    date: Sep. 23, 2023
    target: https://arxiv.org/abs/2309.13271

--- abstract

Forwarding Commitment BGP（FC-BGP） is capable of establishing a secure inter-domain system that can simultaneously authenticate AS_PATH attribute in BGP-UPDATE and validate network forwarding on the dataplane. In an effort to enhance the validation of FC-BGP, a prototype implementation of the FC-BGP was developed and an evaluation was conducted on the FITI high-performance IPv6 backbone network. This document reports on the prototype implementation and the results of the evaluation.


--- middle

# Introduction




# Conventions and Definitions

{::boilerplate bcp14-tagged}


# FC-BGP Overview

~~~~~~
AS(65536) --> AS(65537) --> AS(65538)
                      \
                       \--> AS(65539)
~~~~~~
{: #fig-atta-ex title="An FC-BGP UPDATE propagation example."}

It is important to note that this example only introduces important steps here and see xxxxx for details. What's more,  here ASes are global AS and not RS or AS Confederation. No ASPP is taken into consideration.

For the sake of discussion, we assume that AS 65537 receives an FC-BGP UPDATE message for prefix 192.0.2.0/24 from AS 65536 and will send the route to AS 65538 and AS 65539 as {{fig-atta-ex}} shows. An FC-BGP speaker SHOULD propagate an FC-BGP UPDATE message to downstream ASes only after completing the validation and best route path selection.

When the FC-BGP speaker in AS 65536 plans to propagate routes to its downstream AS 65537, it fills the FC path attribute first. The PASN is 0/NULL, the CASN is 65536, and the NASN is 65537. Then it calculates the signature, fills the FC, and forms the FC path attribute and FC-BGP UPDATE message. Then, it sends the UPDATE message out.

When receiving an UPDATE message from AS 65536, the FC-BGP speaker in AS 65537 retrieves the FC path attribute and extracts the FC list. It then finds the FC with `CASN == 65536` as the AS_PATH has only one AS(65536) and checks whether PASN is 0 as well as NASN is 65537. If so, it uses the SKI field to find the public key and calculates the signature using the algorithm specified in the Algorithm ID. If the calculated signature matches the signature in the FC segment, then the AS-Path hop associated with the AS 65536 is verified. This process repeats for all FCs and AS-Paths in the FC list if it has other ASes in AS_PATH. However, if AS 65537 does not support FC-BGP, the BGP speaker of AS 65537 simply forwards the BGP UPDATE to its neighbors when propagating this FC-BGP route without validating the FC path attribute.

FC-BGP speakers need to generate different UPDATE messages for different neighbors. Each UPDATE announcement contains only one route prefix and cannot be aggregated. This is because different route prefixes may have different announcement paths due to different routing policies. Multiple aggregated route prefixes may cause FC generation and verification errors. When multiple route prefixes need to be announced, the FC-BGP speaker needs to generate different UPDATE messages for each route prefix. Thus, the FC-BGP speaker of AS 65537 generates different UPDATE messages for AS 65538 and AS 65539 separately. The biggest difference is that the NASN is 65538 for AS 65538 and 65539 for AS 65539 in the FC segment generated by AS 65537.

Take AS 65538 as the next hop. The FC-BGP speaker in AS 65537 will encapsulate each prefix to be sent to AS 65538 in a single UPDATE message, add the FC path attribute, and sign the path content using its private key to fill a new FC segment. The FC path attribute and the FC segment use the message format shown in xxxx and xxxxx separately. When signing, the FC-BGP speaker computes the SHA256 hash in the order of (PASN ( 0 if absent), CASN, NASN, IP Prefix Address, and IP Prefix Length) first. Next, the FC-BGP speaker should calculate the digest information Digest, sign the Digest with ECDSA, and then fill in the Signature field and other FC fields. After that, AS 65537 prepends its own FC on top of the FC List. At this point, the processing of FC path attributes by the FC-BGP speaker is complete. The subsequent processing of BGP messages follows the standard BGP process.

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
