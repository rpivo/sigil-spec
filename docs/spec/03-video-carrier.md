# Video Carrier

> Status: stub. Nothing here is decided.

How an [attestation](../glossary.md#terms-sigil-defines) is embedded in video.

## To specify

- Embedding domain and technique.
- Payload capacity per unit time.
- Survival through the grading and finishing chain, which is more aggressive than the
  broadcast transmission chain that [ATSC](../glossary.md#organizations-and-standards-bodies) A/335 targets.
- Behavior under crop, scale, and [letterbox](../glossary.md#audio-and-video-production), which have no clean audio analogue and are
  routine in delivery.
- Whether an [attestation](../glossary.md#terms-sigil-defines) must be recoverable from a single frame or requires a sequence.

## Open

- Whether video needs its own attestation at all when audio is present, or whether the audio
  [carrier](../glossary.md#terms-sigil-defines) suffices for most film and television delivery. Silent content and audio
  replacement in post argue for independence.
