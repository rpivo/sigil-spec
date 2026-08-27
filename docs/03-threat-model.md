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
  [signer](glossary.md#terms-sigil-defines)'s [public key](glossary.md#cryptography-and-specification) and current [revocation](glossary.md#cryptography-and-specification) state.
- **Survival of benign processing.** The [carrier](glossary.md#terms-sigil-defines) should survive the ordinary music and film
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
  diffusion-based [watermark](glossary.md#provenance-and-content-authenticity) removal is directly relevant here.
- **Protection against a compromised or dishonest signer.** Key compromise is handled by
  revocation, which is a mitigation and not a prevention.

## Two structural limits

**The [analog hole](glossary.md#provenance-and-content-authenticity).** A capture device cannot verify what feeds its analog input. Audio played
into a microphone or patched into an instrument input is indistinguishable from a live
performance. [Capture attestation](glossary.md#provenance-and-content-authenticity) therefore claims that samples entered the digital domain
through an identified device, and never that a human produced them. [C2PA](glossary.md#organizations-and-standards-bodies) capture hardware
operates under the same bound.

**Weakest-link degradation.** In a [chain of custody](glossary.md#provenance-and-content-authenticity), trust degrades to the least trustworthy
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
- Archival material and existing field recordings. Note that field recorders are digital
  capture devices and can attest going forward, so this shrinks over time for new material
  while remaining fixed for old.
- Anyone who cannot afford to re-equip, which distributes the harm unevenly.
- Output from generators that decline to participate.

Some of this is addressable. Released catalogue has an independent evidence trail, and
prospective timestamping can establish existence dates for material that lacks one. See
[06-existing-recordings.md](06-existing-recordings.md). The last item is why the inference
still does not become safe with adoption. Universal cooperation among
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
   made. Controls the file. Willing to [transcode](glossary.md#audio-and-video-production), resample, or slightly degrade.
2. **The forger.** Wants to attach a false [attestation](glossary.md#terms-sigil-defines), marking AI-generated content as
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

## Accepted risks

Some attacks have no available defence. Naming them and accepting them explicitly is
preferable to implying a coverage the protocol does not have.

### Fabricated historical provenance

A party presents recently generated material as an old recording, supported by manufactured
documentary evidence, and obtains a [provenance assertion](glossary.md#cryptography-and-specification) for it. No process reliably prevents
this. Forensic examination of physical media is costly and defeatable, and documentary
evidence can be constructed.

**Accepted, on the following grounds.**

- *Economically self-limiting.* The deception only pays where material is valuable
  specifically because of its claimed origin, which chiefly means significant lost
  recordings. That category attracts scrutiny in proportion to its value, so the
  highest-payoff cases are the most closely examined.
- *Low and falling base rate.* Genuine archival discoveries number in the hundreds per year
  against roughly a hundred thousand new tracks per day. The category is small enough to
  examine case by case, and shrinks as a proportion of activity as release volume grows. This
  is what makes expensive per-case review affordable. See
  [06-existing-recordings.md](06-existing-recordings.md).
- *A fixed, not growing, exposed window.* Once prospective timestamping runs, material
  published afterwards is expected to carry a log entry, so a claim of age without one is
  anomalous. The fakeable window is therefore bounded at everything predating the log and
  becomes a smaller fraction of the catalogue every year it operates.
- *Declining incentive.* Passing generated material off as historical pays only while
  disclosure carries stigma. As disclosure normalises, the return falls.
- *Detectable after the fact.* Where retroactive attestations are publicly logged with their
  evidence basis, a fabrication is discoverable and attributable even though it was not
  preventable.

**Explicitly not relied upon:** stylistic analysis of the recording. Period pastiche is among
the strongest capabilities of generative models and improves with model quality, so this
exploit becomes easier over time rather than harder. Stylistic adjudication also requires
expert judgment that does not scale, is subjective, and would penalise genuinely
anachronistic work, which is disproportionately the culturally significant kind.

### Non-participating generators

Content from models that do not watermark their output arrives unmarked and indistinguishable
from unmarked historical material. Accepted, because the protocol is a mechanism for
accountability among participants and was never a detector of parties who decline to
participate. See [06-existing-recordings.md](06-existing-recordings.md).

### Determined carrier removal

An adversary controlling a file and willing to degrade it can defeat any imperceptible
carrier. Accepted, on the grounds that the resulting file is unsigned, and unsigned content
carries no adverse inference, so removal returns the file to the status quo rather than
gaining the adversary anything beyond the loss of a positive signal.

## Prior work to review

- Diffusion-based image manipulation and failure modes of robust watermarking, arXiv 2603.12949
- SoK: Watermarking for AI-Generated Content, arXiv 2411.18479
- Black-box detection of language model watermarks, arXiv 2405.20777
