# Open Questions

> Working list. Nothing here is resolved. Add freely, resolve by linking to the spec section
> that settles the question.

## Blocking, answer before writing spec text

1. **Is there a viable point in the capacity, imperceptibility, robustness tradeoff?**
   A full cryptographic attestation is far larger than the identifiers SMPTE ST 2112 and
   ATSC A/334 carry. If a full signature plus content anchor cannot be embedded
   imperceptibly and survive commercial delivery encoding, the core premise fails and the
   fallback is a pointer-based design, which is what C2PA already does. **This should be
   tested empirically first.** Everything else is downstream of the answer.

2. **How is an attestation anchored to its recording?**
   The anchor must be stable under processing the protocol should survive, and sensitive to
   manipulation it should detect. Those pull in opposite directions. Without a good answer,
   the transplant attack in [../03-threat-model.md](../03-threat-model.md) is unmitigated.

3. **Does the Simmons and Winograd work already cover this?**
   arXiv 2405.12336 combines C2PA metadata, ATSC watermarking, and cryptography. Read in
   full. If it covers the music and film case adequately, the honest outcome is to
   contribute there instead of starting something new.

## Design

4. Shared or independent payload format across audio and video carriers.
5. Minimum recoverable excerpt duration.
6. Whether video needs an independent attestation when audio is present.
7. Payload encoding, CBOR versus COSE versus something else.
8. Whether to carry a C2PA manifest pointer alongside the self-contained attestation.
9. How multiple attestations coexist, since a recording may pass through several signers.
   Does a later signer overwrite, append, or layer?
10. Behavior on sparse material such as solo instruments, ambient, and near-silence.

## Governance

11. Trust model. Certificate hierarchy, signer registry, or decentralized.
12. Who administers the right to sign.
13. Whether to inherit C2PA's conformance framework rather than build one.
14. Licensing, and confirming a patent-free path given the density of existing watermarking
    patents in this space. This is a real risk and worth an early search.

## Strategy

15. Standalone protocol, C2PA soft binding algorithm, DDEX extension, or some combination.
16. Whether to approach DDEX and SMPTE with a proposal or first publish and seek comment.
17. What the reference implementation is written in, and who maintains it.

## Resolved

None yet.
