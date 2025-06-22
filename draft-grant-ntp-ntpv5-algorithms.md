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
workgroup: "Network Time Protocols"
venue:
  group: "Network Time Protocols"
  type: "Working Group"
  mail: "ntp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ntp/"
  github: "signalsforgranted/draft-grant-ntp-ntpv5-algorithms"
  latest: "https://signalsforgranted.github.io/draft-grant-ntp-ntpv5-algorithms/draft-grant-ntp-ntpv5-algorithms.html"
author:
 -
    fullname: Sarah Grant
    email: sarah.grant.ietf@gmail.com

normative:

informative:
  RFC5905:
  RFC9523:
  I-D.draft-ietf-ntp-ntpv5:
...

--- abstract

This document describes considerations of synchronisation algorithms with version 5 of the Network Time Protocol (NTP), and defines the use of NTP version 4's algorithm when used with NTP version 5.

--- middle

# Introduction

NTP version 4 (NTPv4) [RFC5905] defines various algorithms and logic which handle several different aspects of acquiring and sustaining synchronisation between NTP clients and servers including filtering of measurements, security mechanisms, source selection, clock control, as well as other algorithms. Later Khronos [RFC9523] defined a companion method to run alongside with NTPv4 clients which aims to detect and mitigate time-shifting based attacks.

However, NTP version 5 (NTPv5) [draft-ietf-ntp-ntpv5] explicitly does not define synchronisation algorithms to allow for implementations to define their own which may be optimised for specific deployment use case or system constraints. For all implementations there are many things that should be taken into consideration in the development of both new algorithms as well as the porting of existing algorithms to NTPv5.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Algorithm Considerations

TODO: General considerations, including interop (When Algorithms Collide)

## Use of Extension Fields

Algorithms may choose to require additional information be sent by either client or server, however this brings the risk of these fields not being sent by peers which do not support. Algorithms SHOULD handle the absence of any extension fields and define behaviour when they are not present.

TODO: Signalling of algorithms? If so, this would likely require an IANA registry

# NTPv4 Algorithm use with NTPv5

Support for NTPv4 algorithms is not required for NTPv5 implementations, however those supporting both versions of NTP may find it easier to include it as a default or fall-back option in configurations where others are not set.

NTPv5 introduces several key differences to NTPv4 that implementations should be aware of when either building new implementations of the NTPv4 algorithms or when adapting existing. Most notably, the timestamp format has been changed with NTPv5 to ensure longevity and prevent rollover in the immediate future, which should be taken into consideration when processing and producing packets.

## Use of non-UTC timescales

In addition to UTC, NTPv5 includes support for the transmission of TAI, UT1, and leap-smeared UTC. Implementations SHOULD NOT mix timestamps from different timescales when performing calculations, and it's recommended they minimise the conversion of timescales where possible.

## Leap Seconds and Leap Second Smearing

TODO: Cover smearing and leap seconds. NTPv5 already has normative language around not including leap seconds on smeared timescale, however, NTP implementations should have some accommodation for leap second action (adding/removing) that may be linked to synchronisation in some way.

# NTPv4 Algorithm use with NTPv5

Support for NTPv4 algorithms is not required for NTPv5 implementations, however those supporting both versions of NTP may find it easier to include it as a default or fall-back option in configurations where others are not set.

NTPv5 introduces several key differences to NTPv4 that implementations should be aware of when either building new implementations of the NTPv4 algorithms or when adapting existing. Most notably, the timestamp format has been changed with NTPv5 to ensure longevity and prevent rollover in the immediate future, which should be taken into consideration when processing and producing packets.

TODO: Put in any other points

# Security Considerations

This document introduces no security considerations.

TODO: Discuss general attacks on time via algorithms, e.g. time-shifting

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge that perhaps this was not the smartest idea.
