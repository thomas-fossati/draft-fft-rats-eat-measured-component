---
v: 3

title: EAT Measured Component
abbrev: "EAT Measured Component"
docname: draft-fft-rats-eat-measured-component-latest
category: std
consensus: true
submissionType: IETF

ipr: trust200902
area: "Security"
workgroup: "Remote ATtestation ProcedureS"
keyword: [ EAT, measurements, claim, measured, component ]

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
  RFC7252: coap
  RFC8610: cddl
  RFC9165: cddlplus
  I-D.ietf-cbor-cddl-modules: cddlmod
  RFC9393: coswid
  I-D.ietf-rats-eat: rats-eat
  I-D.ietf-cose-key-thumbprint: cose-key-thumbprint
  I-D.ietf-rats-corim: corim

informative:
  RFC9334: rats-arch
  I-D.tschofenig-rats-psa-token: psa-token

entity:
  SELF: "RFCthis"

--- abstract

This document defines a "measured components" format that can be used with the EAT Measurements claim.

--- middle

# Introduction

{{Section 4.2.6 of -rats-eat}} defines a Measurements claim that:

> "[c]ontains descriptions, lists, evidence or measurements of the software that exists on the entity or any other measurable subsystem of the entity."

This claim allows for different measurement formats, each identified by a different CoAP Content-Format ({{Section 12.3 of -coap}}).
Initially, the only specified format is CoSWID of type "evidence", as per {{Section 2.9.4 of -coswid}}.

This document introduces the "measured components" format that can be used with the EAT Measurements claim in addition or as an alternative to CoSWID.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

In this document, CDDL {{-cddl}} {{-cddlplus}} {{-cddlmod}} is used to describe the data formats.

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
| Signer | A unique identifier of the entity authorizing installation of the measured component. | REQUIRED |
| Countersigners | One or more unique identifiers of further authorizing entities for component installation | OPTIONAL |
{: #tab-mc-info-elems title="Measured Component Information Elements"}

# Data Model

The data model is inspired by the "PSA software component" claim ({{Section 4.4.1 of -psa-token}}), which has been slightly refactored to take into account the recommendations about new EAT claims design in {{Appendix E of -rats-eat}}.

The following types and semantics have been reused:

* COSE Key Thumbprint {{-cose-key-thumbprint}}, for signer and countersigners;
* CoSWID software name and version {{-coswid}}, for component name and version;
* CoRIM digest {{-corim}}, for digest value and algorithm.

## CDDL

The `measured-component` data item:

~~~ cddl
{::include cddl/measured-component.cddlc}
~~~

The CDDL extending the EAT Measurements format:

~~~ cddl
{::include cddl/eat-plug.cddl}
~~~

The associated `content-type` MUST contain the CoAP Content-Format assigned by IANA for the `application/measured-component+cbor`.
When the `content-type` is instead the Content-Format for `application/measured-component+json`, the `content-format` contains the base64url-encoded value of TBD.

# Examples

The examples are CBOR only.
JSON examples will be added in a future version of this document.

The example in {{ex-1}} is a measured component with all the fields populated.

~~~ cbor-edn
{::include cddl/ex1.diag}
~~~
{: #ex-1 title="Complete Measured Component"}

The example in {{ex-eat-1}} is the same measured component as above but used as the format of a `measurements` claim in a EAT claims-set.
Note that the example uses a CoAP Content-Format value from the experimental range (65000), which will change to the value assigned by IANA for the `application/measured-component+cbor` Content-Format.

~~~ cbor-edn
{::include cddl/eat-ex1.diag}
~~~
{: #ex-eat-1 title="EAT Measurements Claim using a Measured Component"}

# Security Considerations {#seccons}

TODO

# IANA Considerations

[^rfced] replace "{{&SELF}}" with the RFC number assigned to this document.

## Media Types Registrations

IANA is requested to add the following media types to the "Media Types" registry {{!IANA.media-types}}.

| Name | Template | Reference |
|-----------------|-------------------------|-----------|
| `mc+cbor` | `application/measured-component+cbor` | {{&SELF}} |
| `mc+json` | `application/measured-component+json` | {{&SELF}} |
{: #tab-mc-regs title="Measured Component Media Types"}

### `application/measured-component+cbor`

{:compact}
Type name:
: application

Subtype name:
: measured-component+cbor

Required parameters:
: n/a

Optional parameters:
: n/a

Encoding considerations:
: binary (CBOR)

Security considerations:
: {{seccons}} of {{&SELF}}

Interoperability considerations:
: n/a

Published specification:
: {{&SELF}}

Applications that use this media type:
: Attesters, Verifiers and Relying Parties

Fragment identifier considerations:
: The syntax and semantics of fragment identifiers are as specified for "application/cbor". (No fragment identification syntax is currently defined for "application/cbor".)

Person & email address to contact for further information:
: RATS WG mailing list (rats@ietf.org)

Intended usage:
: COMMON

Restrictions on usage:
: none

Author/Change controller:
: IETF

Provisional registration:
: no

### `application/measured-component+json`

{:compact}
Type name:
: application

Subtype name:
: measured-component+json

Required parameters:
: n/a

Optional parameters:
: n/a

Encoding considerations:
: binary (JSON is UTF-8-encoded text)

Security considerations:
: {{seccons}} of {{&SELF}}

Interoperability considerations:
: n/a

Published specification:
: {{&SELF}}

Applications that use this media type:
: Attesters, Verifiers and Relying Parties

Fragment identifier considerations:
: The syntax and semantics of fragment identifiers are as specified for "application/json". (No fragment identification syntax is currently defined for "application/json".)

Person & email address to contact for further information:
: RATS WG mailing list (rats@ietf.org)

Intended usage:
: COMMON

Restrictions on usage:
: none

Author/Change controller:
: IETF

Provisional registration:
: no

## Measured Component Content-Format Registrations

IANA is requested to register two Content-Format numbers in the "CoAP Content-Formats" sub-registry, within the "Constrained RESTful Environments (CoRE) Parameters" Registry {{!IANA.core-parameters}}, as follows:

| Content-Type | Content Coding | ID | Reference |
| application/measured-component+cbor | - | TBD1 | {{&SELF}} |
| application/measured-component+json | - | TBD2 | {{&SELF}} |

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
