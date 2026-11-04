# ResoNative: overview

> **Source code is private.** This repository describes the platform and its design principles.
> **Live:** [rn.volskyi.pro](https://rn.volskyi.pro)

**A deterministic loudspeaker and room design platform. Every number comes from cited physics, not from
forum folklore and not from an AI at runtime.**

![Python](https://img.shields.io/badge/Python-3.14-3776AB)
![NumPy](https://img.shields.io/badge/NumPy%20%2F%20SciPy-core-013243)
![pint](https://img.shields.io/badge/units-pint-informational)
![tests](https://img.shields.io/badge/tests-126-success)

## The problem

DIY and small-studio speaker design runs on calculators of unknown provenance, spreadsheets with silent
unit mistakes, and advice that contradicts itself. Open the same project a year later and you get
different curves.

## Principles

1. **Physics first, folklore labelled.** Models come from cited literature; anecdotal knowledge is
   allowed only as explicitly lower-tier evidence
2. **SI units internally, always.** Dimensional checks in the core; a value without a unit is invalid data
3. **Provenance on every record.** Driver and material records carry source, date, method (datasheet /
   measured / estimated) and confidence
4. **Consistency is enforced.** A driver whose Qts contradicts Qes/Qms, or whose Vas contradicts Cms/Sd,
   is flagged and excluded from trusted calculations until resolved
5. **Every result states its class** (`measured`, `simulated-validated` or `estimate`) and the UI shows
   that badge next to the number
6. **Reproducible.** A project file stores inputs only; curves are recomputed deterministically

## What it does

- **Enclosures**: sealed and vented alignments from Thiele/Small parameters, with excursion and port air-velocity checks
- **Crossovers**: digital (Linkwitz-Riley / Butterworth, shelves, baffle-step compensation) and passive
  networks (L-pads, Zobel, values snapped to E24)
- **Directivity**: spinorama-style aggregates (on-axis, listening window, early reflections, sound power)
- **Room acoustics**: room modes, Schroeder frequency, boundary interference, porous absorbers via
  Delany-Bazley + transfer-matrix layers, Helmholtz/membrane resonators, diffusor geometry
- **Measurement bridge**: imports measured frequency and impedance responses and compares simulation
  against measurement honestly (level offsets are reported, never silently normalised away)
- Driver and material library with consistency checks and provenance; cabinet blueprints
- Browser UI that works offline: no build step, no CDN
- CLI for checks and batch calculations
- A scientific audit of the legacy prototype, documenting every proven formula error with evidence before anything was reused

## Architecture

```
 web/  (offline browser UI, vanilla JS)
   │  JSON API
 reso/ core ── physics models (cited) ── unit-checked data layer
   │
 data/  seed driver records · project.yaml per project (inputs only)
```

Deployed as a container behind Caddy, with a forced-command SSH key as the only deploy path.

## My role

Product definition, platform blueprint and decision log, physics model selection, implementation,
the regression test suite, and deployment.
