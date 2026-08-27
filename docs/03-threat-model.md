# Threat Model

> Status: draft. Non-normative. This document is deliberately incomplete and should be
> filled in before any spec text is treated as stable.

A provenance protocol that overstates its guarantees is worse than none, because downstream
products will present its output as certainty. This document is about being precise on what
Sigil can and cannot claim.

## What Sigil attempts to guarantee

- **Attribution.** If a valid signature is recovered, a specific holder of a specific key
  signed a specific claim about this recording.
- **Tamper evidence.** Material alteration of the recording after signing should cause
  verification to fail rather than to silently pass.
- **Offline verification.** Verification should require no network call beyond having the
  signer's public key and current revocation state.
- **Survival of benign processing.** The carrier should survive the ordinary music and film
  post chain. Defining "ordinary" precisely is an open question, see
  [04-requirements.md](04-requirements.md).

## What Sigil explicitly does not guarantee

- **That an unsigned file is not AI generated.** Absence of a signature is not evidence of
  anything. This is the single most important limitation and any conforming implementation
  must surface it. See [00-overview.md](00-overview.md).
- **That a signed claim is true.** Sigil verifies that a signer made a claim. Whether the
  claim is accurate reduces entirely to trust in the signer.
- **Survival of determined adversarial removal.** An attacker who controls the file and is
  willing to degrade it can defeat any imperceptible carrier. Published work on
  diffusion-based watermark removal is directly relevant here.
- **Protection against a compromised or dishonest signer.** Key compromise is handled by
  revocation, which is a mitigation and not a prevention.

## Adversaries to characterize

Each of these needs a written section. They are listed rather than resolved.

1. **The stripper.** Wants to remove provenance so AI-generated content passes as human
   made. Controls the file. Willing to transcode, resample, or slightly degrade.
2. **The forger.** Wants to attach a false attestation, marking AI-generated content as
   human made or attributing content to another creator. Reduces largely to key security.
3. **The transplanter.** Wants to lift a valid signature from a genuine recording and apply
   it to a different one. This is the attack that determines how tightly the payload must be
   bound to the content itself, and it is the most important one to get right.
4. **The launderer.** Wants to erode the signature through repeated benign-looking
   processing, without any single step appearing adversarial.
5. **The false accuser.** Wants to make legitimate content fail verification in order to
   discredit it.

## Known open problem: the transplant attack

Signing a claim without binding it to the specific recording lets any valid signature be
lifted and reapplied. Binding it requires a content-derived value that is stable under the
processing the protocol is meant to survive, yet sensitive enough to change under
manipulation the protocol is meant to detect.

Those two requirements pull in opposite directions and the tension is the central technical
problem of the design. It is unresolved. See
[notes/open-questions.md](notes/open-questions.md).

## Prior work to review

- Diffusion-based image manipulation and failure modes of robust watermarking, arXiv 2603.12949
- SoK: Watermarking for AI-Generated Content, arXiv 2411.18479
- Black-box detection of language model watermarks, arXiv 2405.20777
