# Regulatory Landscape

> Status: informational. Not legal advice. Verify against primary sources before relying on
> any of it.
> Last checked: 2026-08-27.

Relevant because it changes platform incentives, not because Sigil is a compliance product.

## In effect

**EU AI Act, Article 50.** Transparency obligations applied 2 August 2026. Providers of
generative systems face requirements around marking synthetic content in machine-readable
form.

**California AI Transparency Act.** Also took effect 2 August 2026. Notable for two specific
obligations: embedded disclosures, and a free public tool that surfaces provenance data.

## Upcoming, and the one that matters most

**1 January 2027.** Large online platforms are required to detect and surface
standards-compliant provenance data, and are barred from stripping it.

That last clause is the significant one for this project. Platform re-encoding is the
primary reason container-bound provenance fails in practice, described in
[../01-motivation.md](../01-motivation.md). A legal obligation not to strip provenance
changes the incentive that has made preservation optional, and it lands on a short timeline.

## Why this shapes the design rather than defining it

Regulation creates demand for a mechanism, not for a particular one. Sigil should be
designed against the technical requirements in [../04-requirements.md](../04-requirements.md)
and evaluated on whether it works, with regulatory timing treated as a reason the work is
timely rather than as a specification input.

## To verify

- Primary text of AI Act Article 50 and the current state of associated guidance.
- California SB 942 as enacted and amended, including definitions of covered providers.
- Whether either regime names specific standards, and whether C2PA conformance is treated as
  presumptive compliance anywhere.
