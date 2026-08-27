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

## Two structural limits

**The analog hole.** A capture device cannot verify what feeds its analog input. Audio played
into a microphone or patched into an instrument input is indistinguishable from a live
performance. Capture attestation therefore claims that samples entered the digital domain
through an identified device, and never that a human produced them. C2PA capture hardware
operates under the same bound.

**Weakest-link degradation.** In a chain of custody, trust degrades to the least trustworthy
participant. Any party able to induce a trusted signer to sign obtains a valid signature. The
mitigation is not technical but definitional: honest declaration of unattested inputs must be
a conformance requirement. See [05-chain-of-custody.md](05-chain-of-custody.md).

## The absence inference

The most likely misuse of this protocol is not an attack. It is a reasonable-sounding
inference that does not hold.

Once adoption is broad, it becomes tempting to read a missing signature as evidence that
content is not of organic origin. The reasoning is that compliant generators mark their
output, so unmarked material is suspect. **The protocol must not support this inference, and
implementations must not present results in a way that implies it.**

The population of unsigned recordings permanently contains:

- The entire catalogue of work created before the protocol existed.
- Everything captured on the large installed base of hardware that will never attest.
- Analog and vintage workflows, where the absence of modern signal handling is the point.
- Field recordings and archival material.
- Anyone who cannot afford to re-equip, which distributes the harm unevenly.
- Output from generators that decline to participate.

The last item is why this does not resolve with adoption. Universal cooperation among
compliant vendors still leaves unsigned material produced by non-cooperating ones, so the
unsigned population always mixes the innocent with the evasive. There is no adoption
threshold past which absence becomes informative.

The supportable framing is asymmetric:

> **A valid attestation raises confidence. Its absence returns to the status quo and carries
> no adverse weight.**

Anything stronger converts a provenance system into a penalty on everyone who was recording
before it existed. See [spec/05-verification.md](spec/05-verification.md) for the
corresponding reporting requirements.

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
6. **The launderer of provenance.** Wants to pass unattested or generated content through a
   trusted signer so it emerges with a valid attestation. Distinct from the forger: no key is
   compromised and no signature is faked. The chain is used exactly as designed.

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
