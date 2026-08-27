# Requirements

> Status: draft. Non-normative. These are candidate requirements, not agreed ones.

A viable protocol has to satisfy all of the following. Any one of them failing is
disqualifying, so they double as a list of ways this idea could turn out not to work.

## Perceptual

- **R1.** The embedded signature must be imperceptible to listeners and viewers under
  critical conditions, meaning studio monitoring and reference displays, not consumer
  playback. [Mastering](glossary.md#audio-and-video-production) engineers will reject anything audible on a good chain, and their
  rejection ends adoption in music.
- **R2.** Embedding must be acceptable to professional practice, meaning it does not
  meaningfully constrain dynamic range, stereo imaging, or color grading.

## Robustness

- **R3.** Survive lossy encoding at commercial delivery bitrates, across the codecs actually
  used in music and film distribution.
- **R4.** Survive [sample rate](glossary.md#audio-and-video-production) and [bit depth](glossary.md#audio-and-video-production) conversion, and frame rate conversion.
- **R5.** Survive [loudness normalization](glossary.md#audio-and-video-production) as applied by streaming platforms.
- **R6.** Survive partial excerpting. A [verifier](glossary.md#terms-sigil-defines) should be able to recover an [attestation](glossary.md#terms-sigil-defines)
  from a fragment rather than needing a complete file. The minimum recoverable duration is
  an open question.
- **R7.** Degrade honestly. A damaged [carrier](glossary.md#terms-sigil-defines) must [fail closed](glossary.md#cryptography-and-specification), reporting no signature or an
  invalid one, never a false valid.

## Cryptographic

- **R8.** Verification must be possible offline given the [signer](glossary.md#terms-sigil-defines)'s [public key](glossary.md#cryptography-and-specification).
- **R9.** The [attestation](glossary.md#terms-sigil-defines) must be bound to the recording such that transplanting it to
  different content fails verification. See [03-threat-model.md](03-threat-model.md).
- **R10.** Key [revocation](glossary.md#cryptography-and-specification) must be supported, and the revocation-checking path must not
  silently reintroduce a hard network dependency into normal verification.
- **R11.** [Algorithm agility](glossary.md#cryptography-and-specification). Signature schemes get broken. The payload must carry enough
  versioning to migrate without breaking existing [verifiers](glossary.md#terms-sigil-defines).

## Capacity

- **R12.** The [carrier](glossary.md#terms-sigil-defines) must fit a full signature plus content binding plus claim data, not
  merely an identifier. This is the requirement that most sharply distinguishes Sigil from
  [SMPTE](glossary.md#organizations-and-standards-bodies) ST 2112 and [ATSC](glossary.md#organizations-and-standards-bodies) A/334, which carry identifiers, and it is the one most likely to
  be technically binding. Payload budget is an open question.

## Ecosystem

- **R13.** Royalty-free and implementable without a patent license, or the professional
  bodies will not adopt it.
- **R14.** A reference implementation and test vectors, without which no standards body will
  take a proposal seriously.
- **R15.** Interoperate with rather than duplicate [C2PA](glossary.md#organizations-and-standards-bodies) and [DDEX](glossary.md#organizations-and-standards-bodies). See
  [spec/06-interop.md](spec/06-interop.md).

## Tension to resolve

R12 (carry a full signature) works against R1 (imperceptible) and R3 through R6 (robust).
Payload capacity, imperceptibility, and robustness form a three-way tradeoff, and every
watermarking system is a particular choice within it. Establishing whether a viable point
exists for a full cryptographic payload is the first thing worth testing empirically, and it
should happen before any spec text is written.
