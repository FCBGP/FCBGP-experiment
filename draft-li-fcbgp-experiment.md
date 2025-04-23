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
      name: Yangfei Guo
      org: Zhongguancun Laboratory
      city: Beijing
      country: China
      email: guoyangfei@zgclab.edu.cn     

normative:
  RFC4271: # BGP protocol
  RFC4724: # Graceful Restart Mechanism for BGP
  RFC4760: # Multiprotocol Extensions for BGP-4
  RFC5656: # EC algo for secure shell transport layer
  RFC6480: # RPKI infrastructure
  RFC6482: # ROA profile
  RFC6483: # validation route origin using RPKI and ROA
  RFC6487: # A Profile for X.509 PKIX Resource Certificates
  RFC6793: # BGP: 4-octet AS number
  RFC7606: # Revised Error Handling for BGP UPDATE Messages
  RFC7947: # Internet Exchange BGP Route Server
  RFC8205: # BGPsec protocol
  RFC8208: # BGPsec Algorithms, Key Formats, and Signature Formats
  RFC8209: # BGPsec router certificate, CRL, CSR
  RFC8210: # RTR, version 1
  RFC8635: # Router Keying for BGPsec
  RFC9234: # BGP ROLE Capability and OTC path attr.

informative:
  RFC4272: # BGP Security Vulnerabilities Analysis
  RFC5065: # Autonomous System Confederations for BGP
  RFC5492: # Capabilities Advertisement with BGP-4
  RFC6472: # Recommendation for Not Using AS_SET and AS_CONFED_SET in BGP
  RFC6811: # BGP Prefix Origin Validation
  RFC7132: # Threat Model for BGP Path Security
  RFC7607: # Codification of AS 0 Processing
  RFC7908: # Route Leaks Definition and Classification
  RFC8416: # Simplified Local Internet Number Resource Management with the RPKI (SLURM)
  ASPP: I-D.ietf-grow-as-path-prepending
  ASPA-Profile: I-D.ietf-sidrops-aspa-profile
  ASPA-Verification: I-D.ietf-sidrops-aspa-verification
  Deprecation-AS_SET-AS_CONFED_SET: I-D.ietf-idr-deprecate-as-set-confed-set # Deprecation of AS_SET and AS_CONFED_SET in BGP
  FC-ARXIV:
    title: "Secure Inter-domain Routing and Forwarding via Verifiable Forwarding Commitments"
    date: Sep. 23, 2023
    target: https://arxiv.org/abs/2309.13271


--- abstract

Forwarding Commitment BGP（FC-BGP） is capable of establishing a secure inter-domain system that can simultaneously authenticate AS_PATH attribute in BGP-UPDATE and validate network forwarding on the dataplane. In an effort to enhance the validation of FC-BGP, a prototype implementation of the FC-BGP was developed and an evaluation was conducted on the FITI high-performance IPv6 backbone network, as well as on the network interconnecting Internet Exchange Points（IXPs）. This document reports on the prototype implementation and the results of the evaluation.


--- middle

# Introduction




# Conventions and Definitions

{::boilerplate bcp14-tagged}


# FC-BGP Overview

# FC-BGP Testbed

## FITI

The prototypes of our solutions for FC-BGP are implemented and tested on FITI(Future Internet Technology Infrastructure). FITI is a major Chinese science and technology infrastructure investment, which is now under construction and operation by 40 universities, including the National Development and Reform Commission, the Ministry of Education and Tsinghua University.
FITI holds 4096 ASN assigned by APNIC, along with a /20 IPv6 address block. With sites distributed across China, it features a genuine internet environment suitable for large-scale network experimentation, with a maximum bandwidth of up to 1.2T.

FITI high-performance IPv6 backbone network connects 40 experimental nodes across 35 cities nationwide via 100G bandwidth links. It possesses 4,096 AS numbers and an IPv6 address block of 240a:a000::/20, enabling parallel experiments across 4,096 independent networks. This configuration establishes FITI as the largest Internet test facility in the world.

FITI holds 4096 AS numbers assigned by APNIC, along with a /20 IPv6 address block. With sites distributed across China, it features a genuine internet environment suitable for large-scale network experimentation, with a maximum bandwidth of up to 1.2T.

## FC-BGP Testbed on FITI Infrastructure

# Test Experience and Results

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
