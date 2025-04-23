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

informative:

--- abstract

Forwarding Commitment BGP（FC-BGP） is capable of establishing a secure inter-domain system that can simultaneously authenticate AS_PATH attribute in BGP-UPDATE and validate network forwarding on the dataplane. In an effort to enhance the validation of FC-BGP, a prototype implementation of the FC-BGP was developed and an evaluation was conducted on the FITI high-performance IPv6 backbone network, as well as on the network interconnecting Internet Exchange Points（IXPs）. This document reports on the prototype implementation and the results of the evaluation.


--- middle

# Introduction




# Conventions and Definitions

{::boilerplate bcp14-tagged}


# FC-BGP Overview

# FC-BGP Testbed

## FITI

The prototypes of our solutions for FC-BGP are implemented and tested on FITI (Future Internet Technology Infrastructure). FITI is a major Chinese science and technology infrastructure investment, which is now under construction and operation by 40 universities, including the National Development and Reform Commission, the Ministry of Education and Tsinghua University. FITI holds 4096 Autonomous System Numbers (ASNs) assigned by APNIC, along with a 240a:a000::/20 IPv6 address block. With sites distributed across China, it features a genuine internet environment suitable for large-scale network experimentation. FITI supports a maximum bandwidth of up to 1.2 Tbps.

## FC-BGP Testbed on FITI Infrastructure
~~~~~~
                                    ┌────────────┐
                                    │            │
 ┌─────────┐      ┌──────────┐      │            │      ┌──────────┐       ┌──────────┐
 │ Beijing │──────┤  FC-BGP  │──────┤            ├──────│  FC-BGP  ├───────│ Shanghai │
 └─────────┘      └──────────┘      │            │      └──────────┘       └──────────┘
                                    │            │
                                    │    FITI    │
     ...              ...           │            │          ...                ...     
                                    │  Backbone  │
                                    │            │
                                    │            │
 ┌─────────┐      ┌──────────┐      │            │      ┌──────────┐       ┌──────────┐
 │ Nanjing │──────┤  FC-BGP  │──────┤            ├──────│  FC-BGP  ├───────│ Shenzhen │
 └─────────┘      └──────────┘      │            │      └──────────┘       └──────────┘
                                    │            │
                                    └────────────┘
~~~~~~
{: #fig-rs-ex title="FC-BGP Testbed on FITI Infrastructure."}

# Test Experience and Results

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
