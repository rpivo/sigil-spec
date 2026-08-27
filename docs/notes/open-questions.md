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

## Chain of custody

See [../05-chain-of-custody.md](../05-chain-of-custody.md).

18. Whether each stage re-signs, vouching for what it verified, or whether attestations
    accumulate independently and are evaluated together.
19. How a chain summary stays within carrier capacity as session complexity grows.
20. What a verifier reports for a partially attested chain, which is the common case.
21. Whether plugin and virtual instrument vendors can participate, and what a plugin
    attestation would assert.
22. How a chain survives stem delivery, where a release is assembled from separately
    exported files.
23. Whether the assurance tiers should map onto C2PA assurance levels rather than being
    defined independently.

24. How to structure the result model so the absence inference is hard to express, rather
    than merely discouraged in prose. Prohibiting it in text is unlikely to be enough if
    downstream products find a binary presentation commercially convenient.
25. Whether the protocol should say anything about generators that decline to watermark, or
    treat that as explicitly out of scope.

## Existing recordings

See [../06-existing-recordings.md](../06-existing-recordings.md).

26. Whether retroactive provenance assertions belong in this protocol, or are better left to
    existing rights and registration systems.
27. What corroborating evidence a provenance assertion must carry to be worth anything, and
    whether the standard should differ for corroborated and uncorroborated material.
28. Who accredits retroactive signers. Whether an existing registry body such as an ISRC
    agency could take the role rather than a new institution being founded.
29. Whether signer and beneficiary can be separated, given rightsholders hold both the
    catalogue and the strongest interest in its standing.
30. What happens to existing attestations when an accredited signer is compromised or
    withdrawn. Mass revocation across a back catalogue is worth designing for rather than
    discovering.
31. Whether every retroactive attestation should be publicly logged, following Certificate
    Transparency, and whether that log is the same one used for timestamping.
32. Whether prospective timestamping should be specified here or recommended alongside,
    given it is useful independently of the carrier and deployable immediately.
33. How a verifier presents a chain mixing capture attestations, timestamps, and provenance
    assertions, which is the normal case for catalogue material.

## The timestamp log

See [../07-timestamp-log.md](../07-timestamp-log.md).

34. Whether to operate a log or rely on an existing service, and the durability trade-offs.
35. Exact hashing, perceptual fingerprinting, or both, and what each contributes as evidence.
36. What identifying metadata accompanies an entry, and the privacy consequences of making it
    public.
37. What a verifier does with a fingerprint match that is not an exact hash match.

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
