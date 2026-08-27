# Requirements

> Status: draft. Non-normative. These are candidate requirements, not agreed ones.

A viable protocol has to satisfy all of the following. Any one of them failing is
disqualifying, so they double as a list of ways this idea could turn out not to work.

## Perceptual

- **R1.** The embedded signature must be imperceptible to listeners and viewers under
  critical conditions, meaning studio monitoring and reference displays, not consumer
  playback. Mastering engineers will reject anything audible on a good chain, and their
  rejection ends adoption in music.
- **R2.** Embedding must be acceptable to professional practice, meaning it does not
  meaningfully constrain dynamic range, stereo imaging, or color grading.

## Robustness

- **R3.** Survive lossy encoding at commercial delivery bitrates, across the codecs actually
  used in music and film distribution.
- **R4.** Survive sample rate and bit depth conversion, and frame rate conversion.
- **R5.** Survive loudness normalization as applied by streaming platforms.
- **R6.** Survive partial excerpting. A verifier should be able to recover an attestation
  from a fragment rather than needing a complete file. The minimum recoverable duration is
  an open question.
- **R7.** Degrade honestly. A damaged carrier must fail closed, reporting no signature or an
  invalid one, never a false valid.

## Cryptographic

- **R8.** Verification must be possible offline given the signer's public key.
- **R9.** The attestation must be bound to the recording such that transplanting it to
  different content fails verification. See [03-threat-model.md](03-threat-model.md).
- **R10.** Key revocation must be supported, and the revocation-checking path must not
  silently reintroduce a hard network dependency into normal verification.
- **R11.** Algorithm agility. Signature schemes get broken. The payload must carry enough
  versioning to migrate without breaking existing verifiers.

## Capacity

- **R12.** The carrier must fit a full signature plus content binding plus claim data, not
  merely an identifier. This is the requirement that most sharply distinguishes Sigil from
  SMPTE ST 2112 and ATSC A/334, which carry identifiers, and it is the one most likely to
  be technically binding. Payload budget is an open question.

## Ecosystem

- **R13.** Royalty-free and implementable without a patent license, or the professional
  bodies will not adopt it.
- **R14.** A reference implementation and test vectors, without which no standards body will
  take a proposal seriously.
- **R15.** Interoperate with rather than duplicate C2PA and DDEX. See
  [spec/06-interop.md](spec/06-interop.md).

## Tension to resolve

R12 (carry a full signature) works against R1 (imperceptible) and R3 through R6 (robust).
Payload capacity, imperceptibility, and robustness form a three-way tradeoff, and every
watermarking system is a particular choice within it. Establishing whether a viable point
exists for a full cryptographic payload is the first thing worth testing empirically, and it
should happen before any spec text is written.
