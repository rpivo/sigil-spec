# Overview

> Status: draft. Non-normative.

Sigil is a proposed protocol for carrying a cryptographic attestation of origin inside an
audio or video signal, imperceptibly, in a form that any adopting vendor can verify.

## In one paragraph

A creator, tool, or model signs a small structured claim about how a recording was made.
That signature is embedded into the signal itself rather than into the file container, at a
level below human perception. A verifier reading the file can recover the signature, check
it against a known public key, and report what the signer claimed. Because the attestation
travels in the signal and is self-contained, it survives container changes and verifies
without a network lookup.

## What a claim asserts

A Sigil claim is a statement by an identified signer, at a point in time, about a specific
recording. The intended shape of that statement is covered in
[spec/01-payload.md](spec/01-payload.md). The important property is what it does *not*
assert: Sigil does not prove a recording is authentic, only that a particular party signed a
particular claim about it. Trust in the claim reduces to trust in the signer.

## What verification returns

Three outcomes, and the distinction between the second and third matters:

1. **Valid signature.** A recognized signer made this claim about this recording.
2. **No signature found.** Says nothing. The file may predate the protocol, may have come
   from a non-adopting tool, or may have been processed in a way that destroyed the carrier.
3. **Signature present but invalid.** The recording was altered after signing, or the
   signature was forged or corrupted.

Any user-facing implementation must not collapse outcome 2 into "not AI generated." That
inference is unsupported and is the most likely way this protocol gets misused in practice.

## Where it fits

Sigil is a carrier and attestation layer. It is complementary to, not a replacement for:

- **C2PA**, which provides a richer provenance model and an established manifest format.
  Sigil could plausibly serve as a registered soft binding algorithm within it.
- **DDEX**, which provides the music industry vocabulary for AI involvement and the delivery
  pipeline that carries it. Sigil could make a DDEX disclosure verifiable rather than merely
  declared.
- **SMPTE**, which governs carriage in professional film and television workflows, and which
  is actively studying how provenance data should flow through MXF.

See [spec/06-interop.md](spec/06-interop.md).
