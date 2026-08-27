# Conventions

> Status: draft. Non-normative.

## Document status

No document in `docs/spec/` is normative yet. When a section stabilizes, replace this banner
with a version and status line.

## Requirement keywords

Once sections become normative, the key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT,
SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL are to be interpreted as described in
BCP 14 (RFC 2119, RFC 8174) when, and only when, they appear in all capitals.

## Terminology

This section defines the terms Sigil itself introduces. When these sections become
normative, the definitions below are binding.

Terminology **borrowed** from other bodies, along with acronyms and domain jargon from both
the media production and cryptography sides, is collected in
[../glossary.md](../glossary.md). Those entries are informal restatements for readability;
the owning body's specification governs.

Terms are chosen to avoid collision with existing standards. Note that both C2PA and SMPTE
use *binding* as a term of art, C2PA for soft bindings and SMPTE for Open Binding of
IDentifiers, so this document avoids the bare word.

| Term | Meaning |
|---|---|
| **Claim** | The structured assertion a signer makes about a recording. |
| **Attestation** | A claim plus its signature and content anchor. |
| **Content anchor** | The content-derived value that ties an attestation to a specific recording. |
| **Carrier** | The imperceptible channel in the signal that conveys an attestation. |
| **Signer** | The party whose key signs a claim. A tool, model operator, studio, or individual. |
| **Verifier** | An implementation that recovers and checks an attestation. |
| **Recovery** | Extracting an attestation from a carrier. Distinct from verifying it. |

## Open

- Whether to define a formal conformance profile structure, and at what granularity.
