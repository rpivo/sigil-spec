# Conventions

> Status: draft. Non-normative.

## Document status

No document in `docs/spec/` is [normative](../glossary.md#cryptography-and-specification) yet, meaning none of it states a requirement that
something must satisfy in order to conform. Nothing here is stable enough to implement
against. When a section stabilizes, replace its banner with a version and status line.

Note that [normative](../glossary.md#cryptography-and-specification) is a property of the text rather than a claim about the document's
standing. A specification can be entirely normative and adopted by no one. See
[../glossary.md](../glossary.md).

## Requirement keywords

Once sections become normative, the key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT,
SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL are to be interpreted as described in
[BCP 14](../glossary.md#cryptography-and-specification) (RFC 2119, RFC 8174) when, and only when, they appear in all capitals.

## Terminology

This section defines the terms Sigil itself introduces. When these sections become
normative, the definitions below are binding.

Terminology **borrowed** from other bodies, along with acronyms and domain jargon from both
the media production and cryptography sides, is collected in
[../glossary.md](../glossary.md). Those entries are informal restatements for readability;
the owning body's specification governs.

Terms are chosen to avoid collision with existing standards. Note that both [C2PA](../glossary.md#organizations-and-standards-bodies) and [SMPTE](../glossary.md#organizations-and-standards-bodies)
use *binding* as a term of art, [C2PA](../glossary.md#organizations-and-standards-bodies) for [soft bindings](../glossary.md#provenance-and-content-authenticity) and [SMPTE](../glossary.md#organizations-and-standards-bodies) for Open Binding of
IDentifiers, so this document avoids the bare word.

| Term | Meaning |
|---|---|
| **Claim** | The structured assertion a [signer](../glossary.md#terms-sigil-defines) makes about a recording. |
| **[Attestation](../glossary.md#terms-sigil-defines)** | A claim plus its signature and [content anchor](../glossary.md#terms-sigil-defines). |
| **[Content anchor](../glossary.md#terms-sigil-defines)** | The content-derived value that ties an [attestation](../glossary.md#terms-sigil-defines) to a specific recording. |
| **[Carrier](../glossary.md#terms-sigil-defines)** | The imperceptible channel in the signal that conveys an attestation. |
| **[Signer](../glossary.md#terms-sigil-defines)** | The party whose key signs a claim. A tool, model operator, studio, or individual. |
| **[Verifier](../glossary.md#terms-sigil-defines)** | An implementation that recovers and checks an attestation. |
| **Recovery** | Extracting an attestation from a [carrier](../glossary.md#terms-sigil-defines). Distinct from verifying it. |

## Open

- Whether to define a formal conformance profile structure, and at what granularity.
