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

This document defines a EAT claim to carry information about measured components.

--- middle

# Introduction

This document defines a EAT {{-rats-eat}} claim to carry information about measured components.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

In this document, CDDL {{-cddl}} {{-cddlplus}} is used to describe the data formats.

The reader is assumed to be familiar with the vocabulary and concepts defined in {{-rats-arch}}.

# Information Model {#measured-component}

A measured component information element includes the computed digest on the software or configuration payload, along with metadata that helps in identifying the measurement and the authorizing entity for component installation.

{{tab-mc-info-elems}} describes the information model of a measured component.

| IE | Description | Requirement Level |
|----|-------------|-------------------|
| Component Name | The name given to a measured component. It is important that this name remains consistent across different releases to allow for better tracking of the same measured item across updates. When combined with a consistent versioning scheme, it enables better signaling from the appraisal procedure to the relying parties. | REQUIRED |
| Component Version | A value representing the specific release or development version of the measured component.  Using Semantic Versioning is RECOMMENDED. | OPTIONAL |
| Digest Value | Hash of the invariant part of the component that is loaded in memory at startup time. | REQUIRED |
| Digest Algorithm | Hash algorithm used to compute the Digest Value. | REQUIRED |
| Signer | A unique identifier of the entity authorizing installation measured component. | REQUIRED |
| Countersigners | One or more unique identifiers of further authorizing entities for component installation | OPTIONAL |
{: #tab-mc-info-elems title="Measured Component Information Elements"}

# Data Model

Recycle from:

* [Initial sketch](https://github.com/EntrustCorporation/draft-x509-evidence/issues/2)
* [PSA SW components](https://www.ietf.org/archive/id/draft-tschofenig-rats-psa-token-20.html#section-4.4.1)
* [COSE Key Thumbprint (signer ID)](https://datatracker.ietf.org/doc/draft-ietf-cose-key-thumbprint)
* [CoSWID SW name and version](https://www.rfc-editor.org/rfc/rfc9393.html#section-2.3)
* [CoRIM digest](https://www.ietf.org/archive/id/draft-ietf-rats-corim-03.html#section-1.3.8)

Also consider:

* [New claims design considerations](https://www.ietf.org/archive/id/draft-ietf-rats-eat-25.html#appendix-E)

# Examples

TODO

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
