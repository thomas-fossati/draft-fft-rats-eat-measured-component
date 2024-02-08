---
v: 3

title: A Measured Component Claim for EAT
abbrev: "EAT Measured Component"
docname: draft-fft-rats-eat-measured-component-latest
category: std
consensus: true
submissionType: IETF

ipr: trust200902
area: "Security"
workgroup: "Remote ATtestation ProcedureS"
keyword: [ EAT, claim, measured, component ]

stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 - name: Simon Frost
   organization: Arm
   email: Simon.Frost@arm.com
 - name: Thomas Fossati
   organization: Linaro
   email: Thomas.Fossati@linaro.org
 - name: Hannes Tschofenig
   organization: Siemens
   email: Hannes.Tschofenig@siemens.com

normative:
  STD90:
    -: json
    =: RFC8259
  RFC8610: cddl
  RFC9165: cddlplus
  RFC9334: rats-arch
  STD94:
    -: cbor
    =: RFC8949
  IANA.cwt:
  IANA.jwt:
  BCP26:
    -: ianacons
    =: RFC8126
  I-D.ietf-rats-eat: rats-eat

informative:
  RFC7942: impl-status

entity:
  SELF: "RFCthis"

--- abstract

This document defines ...

--- middle

# Introduction

EAT {{-rats-eat}} ...

# Conventions and Definitions

{::boilerplate bcp14-tagged}

In this document, CDDL {{-cddl}} {{-cddlplus}} is used to describe the data formats.

The reader is assumed to be familiar with the vocabulary and concepts defined in {{-rats-arch}}.

# Measured Component {#measured-component}

{: vspace="0"}

`key-0`:
: TODO

`key-1`:
: TODO

# Examples

TODO

# Implementation Status

This section records the status of known implementations of the protocol defined by this specification at the time of posting of this Internet-Draft, and is based on a proposal described in {{-impl-status}}.
The description of implementations in this section is intended to assist the IETF in its decision processes in progressing drafts to RFCs.
Please note that the listing of any individual implementation here does not imply endorsement by the IETF.
Furthermore, no effort has been spent to verify the information presented here that was supplied by IETF contributors.
This is not intended as, and must not be construed to be, a catalog of available implementations or their features.
Readers are advised to note that other implementations may exist.
According to {{-impl-status}}, "this will allow reviewers and working groups to assign due consideration to documents that have the benefit of running code, which may serve as evidence of valuable experimentation and feedback that have made the implemented protocols more mature.
It is up to the individual working groups to use this information as they see fit".

# IANA Considerations

[^rfced] replace "{{&SELF}}" with the RFC number assigned to this document.

## CWT `measured-component` Claim Registration

IANA is requested to add a new `measured-component` claim to the "CBOR Web Token (CWT) Claims" registry {{IANA.cwt}} as follows:

* Claim Name: measured-component
* Claim Description: Measured Component
* Claim Key: TBD
* Claim Value Type(s): CBOR Map
* Change Controller: IETF
* Specification Document(s): {{measured-component}} of {{&SELF}}

The suggested value for the Claim Key is TBD

## JWT `measured-component` Claim Registration

IANA is requested to add a new `measured-component` claim to the "JSON Web Token Claims" sub-registry of the "JSON Web Token (JWT)" registry {{IANA.jwt}} as follows:

* Claim Name: measured-component
* Claim Description: Measured Component
* Claim Value Type(s): JSON object
* Change Controller: IETF
* Specification Document(s): {{measured-component}} of {{&SELF}}

--- back

# Collected CDDL {#collected-cddl}

TODO

# Open Issues

The list of currently open issues for this documents can be found at [](https://github.com/thomas-fossati/draft-fft-rats-eat-measured-component/issues).

<cref>Note to RFC Editor: please remove before publication.</cref>

# Acknowledgments
{:numbered="false"}

The authors would like to thank
TBD
for their comments, reviews and suggestions.

[^rfced]: RFC Editor:

