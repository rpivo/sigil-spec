# Interoperability

> Status: draft. Non-normative.

Sigil is more likely to matter as a component of the existing standards landscape than as a
competitor to it. This document tracks the three relationships that matter.

## C2PA

**Opportunity.** C2PA defines soft bindings, watermarks or fingerprints that allow a stripped
manifest to be re-discovered, and maintains an approved algorithm list plus a Soft Binding
Resolution API. Registering Sigil as an approved soft binding algorithm is a far shorter
adoption path than establishing a parallel standard.

**Tension.** A C2PA soft binding conventionally carries a pointer to a remotely stored
manifest. Sigil's premise is that the attestation itself travels in the signal and verifies
offline. These are reconcilable, since a carrier can hold both a self-contained attestation
and a manifest pointer, but the design has to be explicit about it.

**To determine.** The requirements and process for the approved algorithm list, and whether a
self-contained attestation fits the soft binding model or needs an extension.

## DDEX

**Opportunity.** The strongest fit. DDEX already defines the AI disclosure vocabulary the
music industry uses and moves it through distributor delivery. Sigil could turn a
self-reported disclosure into a verifiable one, with no new vocabulary and no change to how
labels and distributors describe AI involvement.

**Approach.** Align the Sigil claim vocabulary directly with the DDEX categories, so an
attestation is a signed assertion of the same disclosure that already travels in the
delivery metadata. A verifier could then confirm that the DDEX declaration accompanying a
release matches what the recording itself carries.

**To determine.** DDEX's process for extension proposals, and whether verification belongs
in a DDEX standard at all or alongside one.

## SMPTE

**Opportunity.** The Content Provenance and Authenticity in Media Study Group is pre-standard
and explicitly gathering use cases and requirements, which is a genuine opening for outside
input. SMPTE has also already standardized inaudible identifier carriage in ST 2112, so the
concept of a watermark carrying structured data through professional workflows is
established there.

**Tension.** SMPTE has signaled it does not want to invent a competing scheme, preferring to
ensure its standards can carry whatever the market adopts. That favors framing Sigil as
something SMPTE carriage should support rather than as a SMPTE standard in itself.

**To determine.** How the study group accepts submissions, and its timeline.

## Sequencing

Music first. DDEX has a defined gap, a shorter path from proposal to pilot, and no incumbent
verification mechanism to displace. Film via SMPTE is a slower process against a study group
that has not yet reported. Attempting both at once is likely to mean neither.
