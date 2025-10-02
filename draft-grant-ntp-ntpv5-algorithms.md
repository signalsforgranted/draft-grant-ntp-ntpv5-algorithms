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
  RFC7164:
  RFC7384:
  RFC8915:
  RFC9523:
  RFC9769:
  I-D.draft-ietf-ntp-ntpv5:
  IEEE1588: DOI.10.1109/IEEESTD.2020.9120376
  SMPTE2059:
    title: "SMPTE Profile for Use of IEEE-1588 Precision Time Protocol in Professional Broadcast Applications"
    date: 2021
    target: https://pub.smpte.org/pub/st2059-2/st2059-2-2021.pdf
...

--- abstract

This document describes considerations of synchronisation algorithms with version 5 of the Network Time Protocol (NTP), and provides guidance on the use of NTP version 4's algorithms when used with NTP version 5.

--- middle

# Introduction

NTP version 4 (NTPv4) [RFC5905] defines various algorithms and logic which handle several different aspects of acquiring and maintaining synchronisation of NTP clients including filtering of measurements, security mechanisms, source selection, and clock control, amongst others. Over time NTPv4 has seen additional algorithms be defined to improve security and accuracy, with Khronos [RFC9523] defining a companion method to run alongside with NTPv4 clients that aims to detect and mitigate time-shifting based attacks, and interleaved modes [RFC9769] which defines additional operational modes for both clients and servers by holding additional state and performing additional checks on timestamp values.

However, NTP version 5 (NTPv5) [I-D.draft-ietf-ntp-ntpv5] explicitly does not define these algorithms in conjunction with the wire protocol to allow for the creation and evolution of new algorithms and implementations which may be optimised for specific deployment use case or system constraints. For all implementations there are many factors that should be taken into consideration in the development of both new algorithms as well as the porting of existing algorithms to NTPv5, such as trade-offs between precision and security, costs of complexity, etc.

The decoupling of algorithms to their dependent wire protocol is not new - PTP [IEEE1588] has the concept of "profiles", each of which define different behaviours and algorithms adapted for specific deployments (for example in automotive or power industries), and may even include additional capabilities to the protocol, for example the "daily jam" function in SMPTE ST-2059 [SMPTE2059] where discontinuity is deliberately transmitted to remove built up discrepancies in values.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

This document uses the terminology established in {{I-D.draft-ietf-ntp-ntpv5}}.

# Algorithm Considerations

**TODO**: General considerations, including interop (When Algorithms Collide)

**TODO**: Discuss divergence risk of algorithm from implementation - existing NTPv4 implementations differ greatly

## Extension Fields

Algorithms may choose to use additional information be sent by either client or server, however this brings the risk of these fields not being correctly handled by peers which do not support them. Algorithms must have explicit behaviours defined when any required extension fields are not present.

## Non-UTC timescales

In addition to UTC, NTPv5 includes support for the transmission of TAI, UT1, and leap-smeared UTC timescales. Algorithms may choose to support a limited subset of timescales, and use different logic depending on the timescale supported. Implementations shouldn't mix timestamps from different timescales when performing calculations, and it's recommended they minimise the conversion of timescales where possible to reduce potential confusion and aide in accuracy.

## Leap Seconds and Leap Second Smearing

Existing NTP implementations commonly use one of several known approaches to applying leap seconds to system time: they may "freeze" the clock where the leap second is inserted at the beginning of the last second of the day, or the system clock is "slewed" or "smeared" either before or commencing from the leap second [RFC7164], keeping system time monotonic but less accurate during the period.

Server implementations which use drifting mechanisms to smooth the leap second insertion such as slewing or smearing must only apply it to only to UTC, and must set the timescale flag in packets to clients as "Leap-smeared UTC".

# Use of NTPv4 Algorithms with NTPv5

NTPv5 implementations may use NTPv4 algorithms. Those supporting both versions of NTP may find it easy to include as a default or fall-back option in configurations where others are not set.

NTPv5 introduces several key differences to NTPv4 that implementations should be aware of when either building new implementations of the NTPv4 algorithms or when adapting existing. Most notably, the timestamp format has been changed with NTPv5 to ensure longevity and prevent rollover in the immediate future, which should be taken into consideration when processing and producing packets.

# Security Considerations

General security considerations for time protocols are discussed in RFC 7384 [RFC7384], and security considerations specific to NTPv5 [I-D.draft-ietf-ntp-ntpv5] should also be noted. Not all threats can be sufficiently mitigated through the use of algorithms, for example packet manipulation, spoofing, and cryptographic performance attacks may be better mitigated through the use of authenticated encryption via NTS [RFC8915].

Designers of new algorithms should take into consideration the expected threat model of deployments and should define which threats could potentially be mitigated from those which are not in scope for the intended use cases, for example closed network deployments may have a reduced risk of man in the middle adversaries compared to deployments on public internet.

**TODO**: Discuss general attacks on time via algorithms, e.g. time-shifting

**TODO**: NTPv4 algorithm specific vulnerabilities - Byzatine issues

# IANA Considerations

This document has no IANA actions.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge that perhaps this was not the smartest idea.
