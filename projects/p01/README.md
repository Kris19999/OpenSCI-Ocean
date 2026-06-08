# P01: Scale-Dependent Regime Transition in Submesoscale Air-Sea Coupling Revealed by SWOT

> Does air-sea coupling undergo a regime transition from mesoscale to submesoscale, and can SWOT SSH combined with geostationary SST and scatterometer winds reveal where and how the coupling physics changes?

## Status

| Item | Content |
|---|---|
| Current stage | 🔬 D0 Explore (候选 / candidate) |
| Lead / proposer | Kris19999 |
| Target journal | Nature Communications (primary); GRL (fallback if regime change signal is weak but scale dependence is significant) |
| Start date | 2026-06-05 |
| Expected submission | TBD |

## D0 Priority Checklist

- [ ] Verify SWOT L2 sigma0-derived wind speed quality in the Gulf Stream and Kuroshio Extension pilot boxes
- [ ] Download SWOT KaRIn L2 SSH for Gulf Stream and Kuroshio Extension pilot boxes; verify swath geometry, noise floor, and along-track/cross-track resolution
- [ ] Download concurrent GOES/Himawari SST and ASCAT winds; quantify collocation sample size (SWOT pass × clear-sky SST × scatterometer overlap)
- [ ] Compute scale-dependent coupling coefficient (wind speed gradient vs. SST gradient regression slope) as a function of spatial filter cutoff wavelength, using ASCAT + GOES SST as a quick prototype
- [ ] Literature search: existing work on scale-dependent air-sea coupling coefficients, especially any evidence of regime change or nonlinearity at submesoscales
- [ ] If collocation sample size < statistical threshold, evaluate whether ERA5 or CCMP can extend temporal coverage for the scale-dependence analysis, with SWOT providing the submesoscale ocean structure anchor

## Scientific Question

The mesoscale framework for air-sea coupling (Chelton et al. 2004; Small et al. 2008) describes a quasi-linear relationship: warm SST anomalies destabilize the atmospheric boundary layer, accelerating near-surface winds, with a roughly constant coupling coefficient across mesoscale wavelengths (100–500 km). But what happens when the ocean exhibits sharp fronts and filaments at submesoscale (2–50 km)?

This project asks whether the air-sea coupling coefficient is scale-invariant or undergoes a regime transition at submesoscales. Three possibilities exist:

1. **Continuity**: the mesoscale linear coupling framework extends smoothly to finer scales, and the coupling coefficient remains constant.
2. **Saturation or decay**: at sufficiently sharp fronts, the atmospheric boundary layer cannot adjust fast enough, and coupling weakens — the atmosphere becomes "blind" to the finest ocean structures.
3. **Regime change**: nonlinear boundary-layer responses (convective triggering, hydraulic jumps, secondary circulations) activate at sharp fronts, producing a qualitatively different coupling signature — altered coupling slope, phase shift, or cross-wind component emergence.

Possibility 1 is the null hypothesis. Possibilities 2 and 3 are both publishable, but 3 is the high-impact outcome that would warrant Nature Communications.

The Gulf Stream and Kuroshio Extension are the natural laboratories: they host the world's sharpest SST fronts co-located with energetic submesoscale SSH structures that SWOT can resolve.

## Hypotheses

1. The air-sea coupling coefficient (regression slope of wind speed anomaly on SST anomaly, binned by spatial scale) varies with wavelength and does not remain constant from mesoscale into submesoscale.
2. At wavelengths below a critical threshold (hypothesized 20–50 km), the coupling relationship changes character: the slope, coherence, or phase between wind and SST fields shifts detectably relative to the mesoscale regime.
3. SWOT SSH gradient magnitude is a better predictor of the regime boundary than SST gradient alone, because SSH integrates the dynamical structure (frontal jets, strain field) that controls the sharpness and persistence of surface temperature fronts.
4. The scale at which regime transition occurs differs between the Gulf Stream and Kuroshio Extension, reflecting differences in frontal sharpness, mixed-layer depth, and atmospheric background state.
5. In the submesoscale regime, wind anomalies show a cross-wind component relative to SST fronts (not purely downwind as in mesoscale pressure-adjustment coupling), indicating a shift from pressure-adjustment dominance to vertical-mixing or secondary-circulation dominance.

## Data

All datasets are public. Raw data must not be committed; only download scripts, access notes, and processed outputs are tracked.

**Primary datasets (the observational triad):**

- **SWOT KaRIn L2 Low Rate SSH** (PO.DAAC): submesoscale ocean structure — fronts, eddies, filaments, strain. This is the primary SWOT product with well-validated quality. Used to identify and characterize submesoscale ocean features, not as a wind source.
- **SWOT L2 sigma0-derived wind speed**: primary atmospheric response field. Used to quantify wind-speed anomalies, wind-speed gradients, and near-surface wind kinetic energy proxy at SWOT swath resolution.
- **Geostationary SST**: high-frequency, cloud-permitting SST frontal structure.
  - GOES-East ABI SST / NOAA ACSPO (Gulf Stream sector)
  - Himawari-8/9 AHI SST (Kuroshio Extension sector)

**Supporting datasets:**

- ERA5 10 m winds: gridded background for large-scale atmospheric state removal.
- CCMP ocean surface winds: optional comparison for scale sensitivity.
- SCAT ocean vector winds (MEaSUREs-OSVW, MetOp ASCAT): the atmospheric response field. Vector winds enable decomposition into downwind and crosswind components relative to SST fronts.

## Method

### Pilot regions

- **Gulf Stream** (35–42°N, 75–55°W): GOES-East SST + SWOT SSH + ASCAT winds.
- **Kuroshio Extension** (30–40°N, 140–170°E): Himawari SST + SWOT SSH + ASCAT winds.

### Analysis framework

### Step 1: Collocation and quality control

Collocate SWOT wind speed, SWOT SSH, geostationary SST, and comparison wind products. Apply quality flags for SWOT, cloud masks for SST, rain/land contamination checks, and time-window sensitivity tests.

### Step 2: Multi-scale decomposition

Filter SWOT wind speed, SST, and SSH into scale bands such as 10, 20, 50, 100, 200, and 500 km. The key point is to avoid letting the coarser comparison products define the finest scale that SWOT can test.

### Step 3: SWOT-based wind-speed response diagnostics

Use SWOT wind speed as the core wind field:

```text
K10_SWOT = 0.5 * U10_SWOT^2
```

Primary diagnostics:

- `U10_SWOT'` and `K10_SWOT'` anomalies.
- `|grad U10_SWOT|` and `|grad K10_SWOT|`.
- Spatial collocation with `|grad SST_geo|` and `|grad SSH_SWOT|`.
- Differences between SWOT-resolved structures and ASCAT/ERA5/CCMP structures.

### Step 4: Scale-dependent coupling coefficients

Primary SWOT-based coefficients:

```text
|grad U10_SWOT| = alpha(lambda) |grad SST_geo| + residual
|grad K10_SWOT| = beta(lambda) |grad SST_geo| + residual
K10_SWOT' = gamma(lambda) SST_front_metric + residual
```

The coefficients should be estimated as functions of wavelength or filter cutoff `lambda`. A break, plateau, or phase shift in these curves would indicate scale-dependent coupling or a possible regime transition.

Vector-wind curl/divergence diagnostics are not part of the initial core analysis. They can be revisited later only if they become necessary for a separate mechanism test.

### Step 5: Spectral and coherence analysis

Compute co-spectrum, coherence, and phase between:

- SWOT wind-speed gradients and geostationary SST gradients.
- SWOT wind kinetic energy anomalies and geostationary SST fronts.
- SWOT wind kinetic energy anomalies and SWOT SSH gradients.
- SWOT wind speed and traditional wind products, to quantify what traditional products miss.

### Step 6: Controls and interpretation

Repeat the analysis in weaker-front regions (e.g., subtropical gyres, 20–25°N) to test whether the scale-dependent coupling signature is specific to western boundary currents. Interpret results cautiously because wind kinetic energy can be affected by both large-scale atmospheric forcing and ocean-front-induced boundary-layer adjustment.


## Expected Outputs

- A D0 feasibility note on SWOT wind speed quality, collocation statistics, and initial coupling diagnostics.
- A data-requirements and discussion note in `literature/`.
- Reproducible scripts for collocation, filtering, SWOT wind kinetic energy, coupling coefficients, and spectral diagnostics.
- Candidate figures:
  1. SWOT wind speed / `K10_SWOT` structures over GOES/Himawari SST fronts.
  2. Comparison of SWOT wind speed with ASCAT/ERA5/CCMP in the same region.
  3. Scale-dependent coupling coefficient curves based on SWOT wind speed and geostationary SST.
  4. Coherence and phase spectra between SWOT wind speed, SST fronts, and SWOT SSH gradients.
  5. Gulf Stream versus Kuroshio Extension comparison.
  6. Control-region null or weak-coupling result.

## Feasibility and Risks

**Critical risks:**

- The regime transition may not exist — the coupling may be smoothly scale-invariant or simply decay monotonically. This would rule out NC but still support a GRL paper on the scale dependence itself.
- Collocation sample size between SWOT, clear-sky geostationary SST, and scatterometer passes may be small. Mitigation: extend temporal baseline, use ERA5 as supplementary wind field, prioritize high-coverage seasons.
- ASCAT resolution (~25 km) limits the smallest scales accessible for wind-SST coupling. The analysis can characterize coupling down to ~25 km from the wind side, while SWOT SSH extends ocean structure identification to ~2 km. The gap between 2–25 km must be discussed honestly.

**Manageable risks:**

- Geostationary SST cloud contamination: mitigate with multi-pass composites and quality flags.
- Atmospheric variability (synoptic storms, frontal passages) can mask coupling signals: conditional sampling on atmospheric stability and large-scale wind speed.
- Causal attribution (ocean drives atmosphere vs. atmosphere drives ocean) is inherently difficult from observations alone. Use lead-lag analysis exploiting geostationary SST temporal resolution, and frame claims carefully.
- AI-generated references must be manually verified.

**Pivot strategy:**

If regime transition is not detected but scale-dependent coupling is clear, pivot to GRL with the story: "Submesoscale air-sea coupling weakens below X km — implications for coupled model parameterization."

## Contributor Role

The proposer has worked on ocean submesoscale processes from the master's stage to the present, is affiliated with a National Science Fund for Distinguished Young Scholars team, and has published three related papers in JGR and Ocean Modelling. The proposer can contribute domain-expert AI review, physical interpretation, figure processing, reference verification, and manuscript-level scientific correction, especially for submesoscale dynamics and submesoscale air-sea interaction.

## Progress Log

| Date | Stage | Content | Output |
|---|---|---|---|
| 2026-06-05 | D0 | Participant submitted initial topic proposal (wind KE distribution + coupling) | Original README |
| 2026-06-05 | D0 | Reviewed and reshaped toward scale-dependent regime transition framing | Revised README |

## AI Interaction Log

See `logs/2026-06-05_D0_topic_proposal.md`.

## References

See `literature/literature_seed.md` for a seed list. Key framing references to verify:

- Chelton et al. (2004) — mesoscale SST-wind coupling framework
- Small et al. (2008) — air-sea interaction at mesoscale: review
- Renault et al. (2016, 2019) — current feedback and eddy killing
- Gaube et al. (2015) — SST-wind coupling and eddy-induced Ekman pumping
- Schneider & Qiu (2015) — SST fronts and atmospheric boundary layer adjustment

Each reference must be manually checked before citation.
