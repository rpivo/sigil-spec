# The Timestamp Log

> Status: draft. Non-normative.

Several documents here refer to "the log" as though it were understood. This one explains
what it is, what it can and cannot establish, and why it is the component worth building
first.

## What it is

Compute a hash of a recording, meaning a short fixed-length value, 32 bytes for SHA-256, that
changes completely if any byte of the file changes. Publish that hash to a public record that
can be appended to but never rewritten, together with the date.

Later, to show the recording existed by that date: recompute the hash, locate it in the log,
read the date. The audio is never published, only a fingerprint of it, which discloses
nothing about the content.

## Why entries cannot be backdated

The append-only property is mechanical rather than a matter of trust.

Entries are hashed together into a [Merkle tree](glossary.md#cryptography-and-specification), where each entry feeds into a parent hash and
so on up to a single root representing the whole log. That root is published periodically and
widely, to multiple independent parties. Once a root is public, no earlier entry can be
inserted, removed, or altered without changing the root, which every holder of a previous
root can detect.

Backdating therefore requires forging every published root since the target date, in the
hands of everyone holding a copy. This is the design behind [Certificate Transparency](glossary.md#cryptography-and-specification).

## What it establishes

**Establishes:** this exact content existed no later than this date.

**Does not establish:** who made it, how it was made, whether it was generated, or that it
did not exist earlier.

That is the right shape for the problem. The fraudulent-archival attack requires material to
appear older than it is, and a log entry sets an upper bound on age, which is the bound that
attack must defeat.

## Why a signature's own timestamp is not sufficient

An [attestation](glossary.md#terms-sigil-defines) can contain a date, and once signed that date cannot be altered without
invalidating the signature.

It nonetheless remains **the [signer](glossary.md#terms-sigil-defines)'s assertion**. A [signer](glossary.md#terms-sigil-defines) with an incorrect clock, a
manipulated clock, or bad intent can place any date in the payload and sign it validly. The
signature establishes that the signer asserted the date. It does not establish that the date
is true.

Trustworthy time requires an anchor outside the signer's control. This is why RFC 3161
timestamp authorities exist, and why code signing relies on [trusted timestamping](glossary.md#cryptography-and-specification) rather than
on the signing machine's clock.

The log is therefore not a duplicate of information already carried in the [attestation](glossary.md#terms-sigil-defines). It is
what makes time verifiable at all. See [00-overview.md](00-overview.md) for the general
contrast between the two mechanisms.

## Which material it helps

The counterintuitive part: **the log seals time going forward and cannot seal the past.**

A log begun in 2026 can only show that content existed in 2026 or later. For a 1973 master
that is close to worthless as an age claim, since 2026 is already well inside the period of
concern. Logging the existing catalogue does not establish that any of it predates generative
audio.

| Material | Helped | How |
|---|---|---|
| Pre-log archive | Barely, for age | Cannot establish age. Does provide tamper-evidence from the log date onward, showing a file is bit-identical to what existed when logged. |
| New recordings | Yes | Establishes real age, permanently verifiable afterwards. |
| The era after logging begins | Yes | Absence of an entry becomes anomalous, given sufficient coverage. |

Existing catalogue therefore continues to depend on documentary corroboration and accredited
[provenance assertions](glossary.md#cryptography-and-specification). See [06-existing-recordings.md](06-existing-recordings.md).

**The log's function is to create the future archive rather than to validate the existing
one.** Material recorded and logged now becomes material with provable age later, and only if
the logging happens at the time.

## Coverage, and why it can be measured per catalogue

The stronger protection is not per-file proof but the anomaly effect: once logging is
routine, content claiming a date with no corresponding entry is suspect.

This requires high coverage. A log holding a small fraction of releases produces no signal,
because absence is unremarkable. At near-total coverage, absence becomes evidence.

Coverage is meaningful **per catalogue, not only per industry**. A platform logging all of its
own ingest achieves a complete anomaly signal within its own catalogue immediately,
irrespective of adoption elsewhere. That makes the log useful to a single operator on day
one, which is not true of any other component of this protocol.

## The brittleness problem

Hashes are exact. One byte differs and the hash is unrelated, so the hash of a master does not
match the hash of any encoding derived from it. In a catalogue where files are transcoded
routinely, a naive implementation logs values that later files never match.

Two mitigations, likely both:

- **Log every version.** Master, distribution copies, each delivered encoding. Cheap, since an
  entry is a few dozen bytes.
- **Log an audio fingerprint alongside.** A perceptual identifier such as Chromaprint is
  robust across encodings, allowing a later file to be matched when its exact hash differs.
  Matching is approximate, so the evidence is weaker, but it finds material exact hashing
  misses.

This is the same exact-versus-robust tension as R12 in
[04-requirements.md](04-requirements.md) and should be treated as a design question rather
than an implementation detail.

## Two uses of the same structure

| Log | Contents | Defends against |
|---|---|---|
| **Timestamp log** | Hashes of recordings | Material claiming to be older than it is |
| **Transparency log** | Every retroactive attestation, with evidence basis and signer | A careless or captured accreditation body, by making bad vouching publicly discoverable |

Same machinery, different contents. They may be one log or two. See
[06-existing-recordings.md](06-existing-recordings.md).

## Implementation

Nothing here requires building a log from scratch.

- **OpenTimestamps.** Free, batches hashes and anchors them into Bitcoin. The lowest-effort
  way to begin.
- **Sigstore Rekor.** Open-source general-purpose transparency log, self-hosted or via the
  public instance.
- **RFC 3161 timestamp authority.** Older, centralized, well-supported in existing tooling.

A minimal deployment: on ingest, hash the audio, append hash, fingerprint, timestamp and
minimal identifiers, and publish a signed Merkle root daily.

## Why this is the component to build first

It is the only part of the protocol that:

- Requires nothing from hardware makers, [DAW](glossary.md#audio-and-video-production) vendors, or standards bodies.
- Modifies no audio, so archival objections do not apply.
- Delivers value to a single operator immediately rather than after adoption.
- Produces a running system, which a standards proposal needs and a document cannot supply.

And it has an unusual cost of delay. **The window this mechanism can never protect is exactly
everything predating the log**, and that window is fixed at the moment logging begins. Most
infrastructure decisions can be deferred at the cost of time. This one is deferred at the cost
of coverage that cannot later be recovered.

## Open questions

- Whether to run a log or use an existing service, and the durability implications of each.
- Exact hashing, perceptual fingerprinting, or both, and what each contributes evidentially.
- What identifying metadata accompanies an entry, and the privacy consequences of publishing
  it.
- Whether timestamp and transparency logs share infrastructure.
- What a [verifier](glossary.md#terms-sigil-defines) does with a fingerprint match that is not an exact hash match.
