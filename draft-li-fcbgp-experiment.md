---
title: "Forwarding Commitment BGP Testbed and Deployment Experience"
abbrev: "fcbgp-testbed-and-deployment-experience"
category: exp

docname: draft-li-fcbgp-experiment-latest
submissiontype: IETF
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
  github: "FCBGP/FCBGP-experiment"
  latest: "https://fcbgp.github.io/FCBGP-experiment/draft-li-fcbgp-experiment.html"

author:
  - fullname: Ke Xu
    org: Tsinghua University
    city: Beijing
    country: China
    email: xuke@tsinghua.edu.cn
  - fullname: Xiaoliang Wang
    org: Tsinghua University
    city: Beijing
    country: China
    email: wangxiaoliang0623@foxmail.com
  - fullname: Zhuotao Liu
    org: Tsinghua University
    city: Beijing
    country: China
    email: zhuotaoliu@tsinghua.edu.cn
  - fullname: Li Qi
    org: Tsinghua University
    city: Beijing
    country: China
    email: qli01@tsinghua.edu.cn
  - fullname: Jianping Wu
    org: Tsinghua University
    city: Beijing
    country: China
    email: jianping@cernet.edu.cn
  - fullname: Ziwei Li
    org: Zhongguancun Laboratory
    city: Beijing
    country: China
    email: lizw@zgclab.edu.cn

normative:
informative:
---

# Abstract

Forwarding Commitment BGP (FC-BGP) is capable of establishing a secure inter-domain system that can simultaneously authenticate the AS_PATH attribute in BGP UPDATE messages and validate network forwarding on the data plane. To enhance the validation of FC-BGP, a prototype implementation was developed and evaluated on the FITI high-performance IPv6 backbone network, as well as on a network interconnecting Internet Exchange Points (IXPs). This document reports on the prototype implementation and the results of the evaluation.

# Introduction

_TODO: Add introduction._

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# FC-BGP Overview

_TODO: Describe the protocol overview._

# FC-BGP Testbed

## FITI

The prototypes of our solutions for FC-BGP are implemented and tested on FITI (Future Internet Technology Infrastructure). FITI is a major Chinese science and technology infrastructure investment, currently under construction and operation by 40 universities, including Tsinghua University, the Ministry of Education, and the National Development and Reform Commission.  
FITI holds 4,096 Autonomous System Numbers (ASNs) assigned by APNIC, along with a 240a:a000::/20 IPv6 address block. With sites distributed across China, it provides a genuine Internet environment suitable for large-scale network experimentation. FITI supports a maximum bandwidth of up to 1.2 Tbps.

## FC-BGP Testbed on FITI Infrastructure

_TODO: Describe the setup and configuration._

# Test Experience and Results

_TODO: Summarize findings._

# Security Considerations

_TODO: Add discussion on security._

# IANA Considerations

This document has no IANA actions.

# Acknowledgments
{:numbered="false"}

_TODO: Acknowledge contributors._
