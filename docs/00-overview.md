# Overview

> Status: draft. Non-normative.

Sigil is a proposed protocol for carrying a cryptographic attestation of origin inside an
audio or video signal, imperceptibly, in a form that any adopting vendor can verify.

Its purpose is accountability rather than detection: to make an auditable origin record cheap
to attach and awkward to omit, not to determine whether any given file was generated.

## In one paragraph

A creator, tool, or model signs a small structured claim about how a recording was made.
That signature is embedded into the signal itself rather than into the file container, at a
level below human perception. A verifier reading the file can recover the signature, check
it against a known public key, and report what the signer claimed. Because the attestation
travels in the signal and is self-contained, it survives container changes and verifies
without a network lookup.

## Two mechanisms, often confused

This project involves two distinct pieces, and they are easy to run together because they are
discussed side by side. They are independent, and each works without the other.

|  | The signature | The log |
|---|---|---|
| **Where it lives** | Inside the audio, as an imperceptible carrier | On a public record, external to the file |
| **Travels with the file** | Yes | No |
| **What it says** | A signer claims something about this recording | Content with this hash existed by this date |
| **Verifying it** | Offline, no lookup required | Requires querying the log |
| **Who creates it** | A capture device, a DAW, or an accredited signer | A platform, label, or archive |
| **When it becomes useful** | Once other vendors adopt it | Immediately, and compounds thereafter |

**The signature answers who vouches for this and what they claim. The log answers when this
existed.** Neither can answer the other's question: a hash discloses nothing about origin, and
a signature carries no trustworthy notion of time. See
[07-timestamp-log.md](07-timestamp-log.md).

They compose. Logging the hash of a signed recording gives both an accountable origin claim
and independent evidence of when it existed.

Of the two, the signature is what this specification is about. The log is established
technology, and is treated here as supporting infrastructure that happens to be deployable
without anyone else's cooperation.

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

Outcome 2 is uninformative **in both directions**. It must not be presented as "not AI
generated", and it must not be presented as suspect either. A valid attestation raises
confidence; its absence returns to the status quo and carries no adverse weight. Collapsing
outcome 2 in either direction is the most likely way this protocol gets misused in practice.
See [03-threat-model.md](03-threat-model.md).

## Multiple signers

A recording usually passes through several parties before release, and each may attest to
what it observed. Verification therefore reports a chain rather than a single claim, and a
partially attested chain is the normal case rather than an error. See
[05-chain-of-custody.md](05-chain-of-custody.md).

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
