# Skyfield #1128: patches and reproducible evidence

Prepared and published 7 November 2026. These contributions address NaN-star propagation and two additional almanac edge cases.

## Submitted for review

- [NaN-star pull request #1156](https://github.com/skyfielders/python-skyfield/pull/1156) — open for maintainer review.
- [Almanac improvements and CML offer on #1145](https://github.com/skyfielders/python-skyfield/pull/1145#issuecomment-6709051089) — published as additions to the existing contributor's work.

## Code for review

- [NaN-star fix](https://github.com/cstolting-collab/python-skyfield/commit/7ba3386442392d4870fd1a6988ac17bfbccbef17): propagate unknown positions through `.apparent()`; retain valid rows and document explicit Hipparcos filtering.
- [Retain a usable hour-angle rate](https://github.com/cstolting-collab/python-skyfield/commit/a2d105c6f3a517df98568dee306e2525111d53ae): avoid a zero rate when a later measurement stops moving.
- [Preserve search-window precision](https://github.com/cstolting-collab/python-skyfield/commit/21bad299621db23733614bea50058299d87270eb): keep split-time precision when sampling and interpolating near interval boundaries.
- [Combined tested source](https://github.com/cstolting-collab/python-skyfield/commit/0d533c3223a2a9f31e0a6813421f3f7a3164c2f0).

The almanac additions build directly on [karimosama7's PR #1145](https://github.com/skyfielders/python-skyfield/pull/1145). The original contributor's commit is preserved in the branch history. The NaN-star branch is independent of that PR.

## Download the evidence

[Skyfield_CML_Review_2026-11-07.zip](Skyfield_CML_Review_2026-11-07.zip) contains the three patches, reproduction scripts, numerical observations, CML decisions, source hashes, logs, limitations, and setup instructions.

SHA-256: `6ecc0f2bc91b12db636bed22094d29a166c2c9781c64250dd1ff71ec3dac4d81`.

The ZIP is the unchanged, tested preparation snapshot. Its statement that nothing had been published describes its preparation time; this page records the subsequent publication.

## Observed results

| Check | Result |
| --- | --- |
| Final almanac scenarios | 1,420/1,420 on each of NumPy 2.3.5 and 2.5.3 |
| Additional failures resolved by the precision patch | 29 scenarios; one additional root cause |
| Ordinary/holdout comparisons | 960 cases retain event counts and flags |
| Separate bisection check | 96 events agree within 1 microsecond using the same astronomical model |
| Full Hipparcos catalog | 117,955 valid positions unchanged; 263 unknown rows retained and propagated |
| Deliberate CML failure controls | 12/12 rejected |
| Combined native suite, NumPy 2.3.5 | 591 pass; 1 unchanged upstream datetime-list casting failure |

These results describe finite stress tests and seeded samples, not a production failure-rate survey or proof that every input is correct. The independent bisection checks the solver, using the same ephemeris and apparent-position model.

## CML's role and offer

The harness invokes the real CML `StateGovernance` public runtime API at pinned commit `8331c3b1ac6450b12e8a944bd29c629e0f21ae4d`. The Python adapter computes numerical observations; CML enforces declared conditions and records accepted evidence. CML did not invent the mathematical corrections, and ordinary host logic using the same predicates agreed with its decisions.

The Skyfield patches add no CML dependency. The harness and evidence are available here for review. I am happy to share my CML engine as well if it would be useful to the maintainers or contributor; its existing repository and access settings have not been changed by this publication.

Prepared and tested with Codex assistance. Thanks to ajorennie and karimosama7 for diagnosing the two paths, ejeschke for the almanac reproduction, and Brandon Rhodes for the NaN-propagation direction.
