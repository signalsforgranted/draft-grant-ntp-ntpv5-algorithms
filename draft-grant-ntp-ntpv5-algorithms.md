---
title: "NTPv5 Algorithms"
category: info
docname: draft-grant-ntp-ntpv5-algorithms-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Internet"
workgroup: "NTP"
venue:
  group: NTP
  type: Working Group
  mail: ntp@ietf.org
  arch: https://mailarchive.ietf.org/arch/browse/ntp/
  github: signalsforgranted/draft-grant-ntp-ntpv5-algorithms
  latest: https://signalsforgranted.github.io/draft-grant-ntp-ntpv5-algorithms
author:
 -
    fullname: Sarah Grant
    email: sarah.grant.ietf@gmail.com

normative:

informative:
  RFC5905:
  I-D.draft-ietf-ntp-ntpv5:
...

--- abstract

This document describes considerations of synchronisation algorithms with version 5 of the Network Time Protocol (NTP), and defines the use of NTP version 4's algorithm when used with NTP version 5.

--- middle

# Introduction

NTP version 4 (NTPv4) [RFC5905] defines various algorithms and logic which handle several different aspects of acquiring and sustaining synchronisation between NTP clients and servers including filtering of measurements, security mechanisms, source selection, clock control, as well as other algorithms.

However, NTP version 5 (NTPv5) [draft-ietf-ntp-ntpv5] explicitly does not define these to allow for implementations to define their own algorithms which may differ or be optimised based on the deployment use case or system constraints.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Algorithm Considerations

TODO: General considerations, including interop (When Algorithms Collide)

TODO: Extension field use

TODO: Signalling of algorithms? If so, this would likely require an IANA registry

# NTPv4 Algorithm use with NTPv5

Support for NTPv4 algorithms is not required for any NTPv5 implementation, however those supporting both versions of NTP may find it easier,

TODO: Describe any adaptations that NTPv5 implementations have to make

# Security Considerations

This document introduces no security considerations.

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge that perhaps this was not the smartest idea.
