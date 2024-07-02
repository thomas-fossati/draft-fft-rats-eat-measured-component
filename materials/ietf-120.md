autoscale: true
footer: IETF 120 Vancouver, RATS WG
slidenumbers: true

# Measured Components
## [draft-fft-rats-eat-measured-component](https://datatracker.ietf.org/doc/draft-fft-rats-eat-measured-component)
### IETF 120 Vancouver, RATS WG

^ the document was briefly presented in Brisbane
the chairs asked for feedback from the working group
since then we received reviews and comments from Giri, Laurence and Carl, which we have incorporated
we think the document is in much better shape because of that and we want to have a chance to present it again

---

# EAT `Measurements`

EAT defines an extensible `Measurements` claim, which: _"[c]ontains descriptions, lists, evidence or measurements of the software that exists on the entity or any other measurable subsystem of the entity."_

Currently, CoSWID is the only format supported.

CoSWID is not a good fit for environments that do not have a file system onto which measurements can be anchored.

^ which is the case for early boot components

---

# PSA `Software Components`

The PSA profile has defined its own "software components" format:

```
psa-software-component = {
  ? &(measurement-type: 1)  => text
    &(measurement-value: 2) => psa-hash-type
  ? &(version: 4)           => text
    &(signer-id: 5)         => psa-hash-type
  ? &(measurement-desc: 6)  => text
}
```

---

# Generalising `psa-software-component`

Refactor `psa-software-component` to take into account the recommendations for "new claims design considerations" in [Appendix E of EAT](https://www.ietf.org/archive/id/draft-ietf-rats-eat-25.html#appendix-E):

:white_check_mark: Interoperability and Relying Party Orientation
:white_check_mark: Operating System and Technology Neutral
:white_check_mark: Security Level Neutral
:white_check_mark: Reuse of Extant Data Formats

---

# Measured Component Information Model

| IE | Description | Requirement Level |
|----|-------------|-------------------|
| Component Name | The name given to the measured component. It is important that this name remains consistent across different releases to allow for better tracking of the same measured item across updates. When combined with a consistent versioning scheme, it enables better signaling from the appraisal procedure to the relying parties. | REQUIRED |
| Component Version | A value representing the specific release or development version of the measured component.  Using [Semantic Versioning](https://semver.org/spec/v2.0.0.html) is RECOMMENDED. | OPTIONAL |
| Digest Value | Hash of the measured component. | REQUIRED |
| Digest Algorithm | Hash algorithm used to compute the Digest Value. | REQUIRED |
| Signers | One or more unique identifiers of entities signing the measured component. | OPTIONAL |

^ NOTE on signers' semantics: A signer is an entity that digitally signs the measured component.  For example, as in UEFI Secure Boot [UEFI2] and Arm Trusted Board Boot [TBBR-CLIENT].  A signer is associated with a public key. ... . In some cases, multiple parties may need to sign a component to indicate their endorsement or approval.  This could include roles such as a firmware update system, fleet owner, or third-party auditor.

---

# Measured Component Information Model (cont.)

Are we missing anything absolutely required? (SVN?)

^ Machine-comparable, monotonically increasing version of the component where a greater value indicates a newer version. This value MUST increment for every update associated with security-relevant releases

---

# Measured Component Data Model

^ Reuse EAT `sw-version-type` and CoRIM `digest`.

```
;# import sw-version-type from RFCYYYY as eat
;# import digest from RFCXXXX as corim

measured-component = [
  id:               component-id
  measurement:      corim.digest
  ? signers:        [ + signer-type ]
]

component-id = [
  name:      text
  ? version: eat.sw-version-type
]
```

^ signer-type is "any defined by" EAT profile
^ signers' optionality too is defined by the profile

---

# EAT sockets

The document defines how the `Measurements` extension is consumed by EAT.

CBOR & JSON serialisations:
```
mc-cbor = bstr .cbor measured-component
mc-json = tstr .json measured-component
```

---

# EAT sockets (CBOR)

CBOR EAT (`.feature "cbor"`)

```
$measurements-body-cbor /= mc-cbor            ; native
$measurements-body-cbor /= tstr .b64u mc-json ; tunnel

```

---

# EAT sockets (JSON)

JSON EAT (`.feature "json"`)

```
$measurements-body-json /= mc-json            ; native
$measurements-body-json /= tstr .b64u mc-cbor ; tunnel
```

---

# Summary

Covers a gap for early boot measurements, which CoSWID does not address well

Extends EAT, reusing the available type system as much as possible

Proven data format based on experience with PSA

---

# [fit] adopt me?<br/> 🧀 :rat:
