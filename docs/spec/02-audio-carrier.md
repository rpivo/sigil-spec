# Audio Carrier

> Status: stub. Nothing here is decided.

How an attestation is embedded in audio.

## To specify

- Embedding domain and technique.
- Payload capacity per unit time.
- Synchronization and how a verifier locates an attestation in an arbitrary excerpt (R6).
- Behavior on silence, near-silence, and sparse material. Solo piano and ambient recordings
  are the difficult cases for perceptual masking and should be treated as primary test
  material rather than edge cases.
- Interaction with loudness normalization (R5) and with mastering-stage limiting.

## Study first

SMPTE ST 2112 and ATSC A/334 both solve inaudible carriage at professional scale. Neither
carries a cryptographic payload, but the perceptual and robustness engineering is directly
applicable and should inform this section rather than being rediscovered. See
[../02-prior-art.md](../02-prior-art.md).

## Open

- Whether the audio and video carriers should share a payload format, or whether each should
  carry an independent attestation. Affects files where the two are muxed and later split.
