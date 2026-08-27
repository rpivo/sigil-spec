# Chain of Custody

> Status: draft. Non-normative. Describes an intended architecture and records the
> limitations found while examining it.

Earlier drafts of this repository assumed a single [signer](glossary.md#terms-sigil-defines) attesting to a finished recording.
The intended model is broader: a **[chain of custody](glossary.md#provenance-and-content-authenticity)**, where each stage in the production
path attests to what it saw and passes that forward, so a released file carries a summary of
how it was made.

This document sets out that chain, marks which links are viable, and records the boundaries
that turned out to be hard.

## The motivating example

A guitar recording, tracked through to release:

1. A guitar is played, connected by an analog instrument cable.
2. An audio interface converts the signal to digital.
3. The interface passes audio to a [DAW](glossary.md#audio-and-video-production), which verifies the interface as the source.
4. The DAW exports a mixdown carrying an [attestation](glossary.md#terms-sigil-defines) of the session's sources.
5. A distribution platform verifies the export on upload.
6. The result describes what tools were involved, including whether any were generative.

## Where the chain can start

**Attestation begins at the analog-to-digital boundary.** The [A/D converter](glossary.md#audio-and-video-production) is the first
device in a conventional signal path that holds samples and can therefore compute over them.

This rules out the analog links. A passive instrument cable is two conductors and a shield.
It has no power, no processor, and no key storage, so it cannot sign anything. Building an
active signing cable would put a noise source in the signal path, which professional practice
will not accept, and would not help regardless: a cable can only claim it was present. It
cannot know what was connected to it. Any source can be plugged into any cable.

This generalizes into a rule worth applying to every proposed link:

> **An attestation is only worth having from a device that can bind its claim to the actual
> samples. A device that can only assert its own presence adds cost without adding evidence.**

**Exception: digital instruments.** Where capture is digital at the source, such as digital
pickup systems, USB microphones, or networked audio under Dante or AES67, the chain can start
at the instrument. The boundary is digital capture, not physical position in the path.

## The analog hole, and why it is smaller here than elsewhere

An audio interface cannot verify what is feeding its input. A speaker played into a
microphone, or a converter output patched into an instrument input, is not distinguishable
from a live performance **by the interface itself**.

It may still be distinguishable further along the chain. A robust audio [watermark](glossary.md#provenance-and-content-authenticity) survives a
digital-to-analog-to-digital round trip: audience measurement watermarks are engineered to
survive a television speaker playing into a room and being captured by a microphone, which is
a far harsher path than a line-level cable. So content that was watermarked at generation
carries that mark through analog re-capture, and a [verifier](glossary.md#terms-sigil-defines) downstream can still recover it.

This is a real advantage of audio over metadata-based provenance in other media. A photograph
of a screen loses the original's [C2PA](glossary.md#organizations-and-standards-bodies) [manifest](glossary.md#provenance-and-content-authenticity), because the provenance lived in a container
that did not make the trip. A watermarked signal survives being played and re-recorded.

Two limits remain, and neither is closed by adoption.

**It only works against cooperating generators.** A model that does not watermark its output,
or one modified locally to stop doing so, produces unmarked audio. Watermarking provides
accountability among participants; it is not a detector of parties who decline to
participate.

**[Capture attestation](glossary.md#provenance-and-content-authenticity) still cannot establish live performance.** What an interface signs is

> These samples entered the digital domain through this device at this time.

It does not claim a human performed them. This bound is not a defect of the design, it is a
property of analog inputs, and it is the same limitation C2PA capture hardware operates
under: a signed photograph may be a photograph of a screen. Capture attestation remains
valuable, and overstating it is the failure mode to avoid. See
[03-threat-model.md](03-threat-model.md).

## Per-stage attestation

### Capture device

Signs that identified samples passed through it. Requires a device key provisioned at
manufacture and a [secure element](glossary.md#audio-and-video-production) to hold it, which places a real cost and key management
burden on the hardware vendor. Open questions include real-time signing cost on
entry-level hardware, how the signature is bound to samples across the host transport so a
compromised host cannot substitute audio, and what happens to the enormous installed base of
existing interfaces.

### Digital audio workstation

The most consequential link, because the DAW is the only party that observes every source in
a session: live capture, imported audio, samples, virtual instruments, MIDI, and generative
plugins.

Two design consequences follow.

**Attestation should be per source, not per file.** A useful export claim distinguishes a
track captured through an attested interface from one imported without provenance from one
produced by a named generative plugin. A single file-level flag discards the information
that makes the claim worth anything.

**The claim should summarize rather than carry the chain.** Embedding the full upstream
history exceeds any plausible [carrier](glossary.md#terms-sigil-defines) capacity. See R12 in
[04-requirements.md](04-requirements.md).

This granularity aligns closely with the [DDEX](glossary.md#organizations-and-standards-bodies) AI disclosure categories, which suggests
expressing DAW claims in that vocabulary rather than inventing a parallel one. See
[spec/06-interop.md](spec/06-interop.md).

### Distribution platform

Verifies on ingest and surfaces the result. The reporting requirements in
[spec/05-verification.md](spec/05-verification.md) apply with particular force here, since
this is the stage whose output reaches the public.

## The weakest link problem

Transitive trust degrades to its worst participant. If a DAW's signature is trusted, then
anyone able to make that DAW export a file obtains a valid signature, and the DAW cannot
determine whether an imported file was generated.

The consequence shapes the whole protocol:

> **The value of a chain lies in honest reporting of unattested inputs, not in the presence
> of a signature.**

An export declaring three attested tracks and one unverified import is more useful than one
that merely verifies. An implementation that silently reports unknown inputs as clean
destroys the value of every chain it participates in, which makes honest declaration of
unattested sources a conformance requirement rather than a recommendation.

## Assurance tiers

The chain degrades gracefully, and each tier is independently deployable.

| Tier | Requires | Supported claim |
|---|---|---|
| **0. Export attestation** | A DAW and a receiving platform. Software only. | The DAW reports this session's sources as it observed them. |
| **1. [Attested capture](glossary.md#provenance-and-content-authenticity)** | Interface hardware with a secure element and provisioned keys. | Samples demonstrably entered through an identified device. |
| **2. Extended chain** | Digital capture at the instrument. | Custody from digital source to release. |

Tier 0 requires no hardware and delivers most of the practical value, which makes it the
realistic starting point. A proposal requiring the full chain requires agreement from cable
makers, interface manufacturers, DAW vendors, and platforms simultaneously, and is
correspondingly unlikely to move.

**C2PA already defines [assurance levels](glossary.md#provenance-and-content-authenticity)** expressing how strongly an attestation is
hardware-backed, with Google integrating Assurance Level 2 into Pixel camera hardware.
Mapping these tiers onto that existing vocabulary is preferable to defining a competing one.

## Recordings that predate the chain

Everything captured before the protocol exists sits outside it, and claims about that
material are a different kind of claim. See
[06-existing-recordings.md](06-existing-recordings.md).

## Open questions

Tracked in [notes/open-questions.md](notes/open-questions.md).

- Whether each stage re-signs, vouching for what it verified, or whether attestations
  accumulate independently and are evaluated together.
- How a chain summary stays within carrier capacity as sessions grow.
- What a verifier reports when a chain is partially attested, which is the common case.
- Whether plugin and virtual instrument vendors can be brought in, and what a plugin
  attestation would even assert.
- How this survives [stem](glossary.md#audio-and-video-production) delivery, where a release is assembled from separately exported
  files.
