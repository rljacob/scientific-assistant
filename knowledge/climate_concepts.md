# Climate Science Concepts

Domain knowledge about climate science, Earth system processes, and modeling concepts.

## Format

Organize by topic with clear definitions, relationships, and E3SM relevance.

---

## Earth System Components

### Atmosphere
- **Role**: Heat transport, moisture transport, radiation balance, chemistry
- **Key Processes**: Convection, cloud formation, precipitation, atmospheric circulation
- **E3SM Components**: EAM (Energy Atmosphere Model), EAMxx/SCREAM (next-gen)
- **Key Variables**: Temperature, pressure, humidity, winds, precipitation

### Land Surface
- **Role**: Carbon cycle, water cycle, energy balance, vegetation dynamics
- **Key Processes**: Photosynthesis, evapotranspiration, soil moisture, snow dynamics
- **E3SM Component**: ELM (E3SM Land Model)
- **Key Variables**: Leaf area index (LAI), soil moisture, carbon fluxes, runoff

### Ocean
- **Role**: Heat storage, carbon storage, ocean circulation, sea level
- **Key Processes**: Thermohaline circulation, upwelling, mixing, biogeochemistry
- **E3SM Component**: MPAS-Ocean
- **Key Variables**: Sea surface temperature (SST), salinity, currents, carbon uptake

### Sea Ice
- **Role**: Albedo feedback, ocean-atmosphere coupling, freshwater flux
- **Key Processes**: Freezing/melting, dynamics, thermodynamics
- **E3SM Component**: MPASSI (MPAS Sea Ice)
- **Key Variables**: Ice thickness, concentration, velocity

### Land Ice
- **Role**: Sea level rise, freshwater input, ice sheet dynamics
- **Key Processes**: Ice flow, calving, surface mass balance
- **E3SM Component**: MALI (MPAS-Albany Land Ice)
- **Key Variables**: Ice thickness, velocity, mass balance

### River Routing
- **Role**: Freshwater delivery to ocean, flood dynamics
- **Key Processes**: Routing, reservoir management, flooding
- **E3SM Component**: MOSART (Model for Scale Adaptive River Transport)
- **Key Variables**: River discharge, inundation

---

## Key Climate Processes

### Carbon Cycle
- **Definition**: Exchange of carbon between atmosphere, land, ocean
- **Components**: Photosynthesis, respiration, decomposition, ocean uptake
- **Climate Relevance**: CO2 feedback, carbon-climate coupling
- **E3SM Implementation**: ELM biogeochemistry, ocean carbon model

### Water Cycle
- **Definition**: Movement of water through Earth system
- **Components**: Precipitation, evaporation, runoff, storage
- **Climate Relevance**: Drought, floods, water resources
- **E3SM Implementation**: Fully coupled in WCYCL compsets

### Energy Balance
- **Definition**: Incoming solar radiation vs. outgoing longwave radiation
- **Components**: Shortwave absorption, longwave emission, albedo
- **Climate Relevance**: Temperature regulation, climate sensitivity
- **E3SM Implementation**: Radiation schemes in EAM/EAMxx

---

## Modeling Concepts

## [2026-08-28] E3SM Component GPU/CPU Architecture (for cloud deployment)

**Context:** User building a cloud (GCP) architecture diagram for running E3SM on GPU nodes; needed a rundown of which components use GPUs vs CPU-only, parallelization model, I/O patterns, GPU requirements, and run workflow.

**Key Information:**
- Only **EAMxx (SCREAM)** is GPU-capable today — rewritten in C++/Kokkos. Legacy **EAM**, **MPAS-O**, **MPAS-SI**, **ELM**, **MOSART**, and the **CPL7/MOAB coupler** are all CPU-only Fortran (or coupler infra) with no GPU port.
- Parallelization: base model is MPI (MPMD) across components, each with independent domain decomposition (cubed-sphere for atm, unstructured MPAS mesh for ocn/ice). GPU offload is hybrid **MPI + Kokkos**, typically **1 MPI rank per GPU**, with Kokkos parallelizing over columns/levels within a rank (CUDA backend for NVIDIA). CPU components use standard MPI (+optional OpenMP) per core.
- I/O: PIO2 abstracts NetCDF (serial/gather) vs PnetCDF (true parallel) — PnetCDF preferred at scale. Restarts + history streams (h0/h1...) need a shared parallel filesystem across all nodes (not local SSD alone), since multiple components/nodes access the same case dir concurrently. SCREAM/EAMxx at storm-resolving res can produce very high output volume (potentially TB/day).
- GPU requirements: Kokkos CUDA backend is the practical path on GCP (no maintained HIP/AMD production path). EAMxx is memory-bandwidth sensitive → favor high-HBM-bandwidth GPUs (A100/H100) with 40-80GB capacity. Multi-node scaling benefits from NVLink/NVSwitch intra-node and GPUDirect RDMA / GPUDirect-TCPX(O) inter-node with CUDA-aware MPI.
- Run workflow: stage input/forcing data (Globus/DTN → shared FS or GCS) → CIME build (`create_newcase`/`case.setup`/`case.build`, NVHPC or GNU compiler + nvcc_wrapper for Kokkos build, CUDA-aware OpenMPI/MVAPICH2-GDR) → Slurm-based job submission (Slurm-GCP/Cluster Toolkit) with MPMD launch placing EAMxx ranks on GPU node pool and other components on CPU node pool → periodic restart checkpointing (important for spot/preemptible GPU VM cost control) → output archived to GCS, post-processing/viz (E3SM Diags, NCO, xarray/Dask, ParaView/VisIt) on separate decoupled analysis nodes.

**Related Concepts:** Atmosphere (EAMxx/SCREAM), Ocean (MPAS-O), Land Surface (ELM), Radiative Transfer (EAM/EAMxx radiation schemes), see `compset_usage.md` for EAMxx/SCREAM compset (F2010-SCREAMv1).
**Follow-up:** Confirm actual GCP GPU VM family chosen (A2 vs A3) and whether Cluster Toolkit or custom Slurm-GCP config will be used; check if a GCP CIME machine port already exists or needs to be created; verify current PnetCDF/parallel-FS support status on target GCP filesystem (Filestore vs Managed Lustre) before finalizing diagram.

---

### Spatial Resolution
- **Definition**: Grid spacing for discretizing the Earth system
- **Trade-offs**: Higher resolution = more detail but higher computational cost
- **E3SM Resolutions**: 
  - Low-res: ne4pg2 (~7° atmosphere)
  - Standard: ne30pg2 (~1° atmosphere)
  - High-res: ne120pg2 (~0.25° atmosphere)

### Temporal Resolution
- **Time Step**: Frequency of model calculations
- **Output Frequency**: How often data is saved
- **Run Length**: Total simulation period

### Coupling
- **Definition**: How components exchange information
- **Types**: Synchronous vs. asynchronous
- **E3SM Coupler**: MCT (Model Coupling Toolkit) or MOAB

### Spin-up
- **Definition**: Initial run to reach quasi-equilibrium
- **Purpose**: Avoid drift from unrealistic initial conditions
- **Duration**: Decades to centuries for carbon cycle

### Boundary Conditions
- **Definition**: External forcing (CO2, aerosols, solar, etc.)
- **Types**: Prescribed vs. interactive
- **Time Periods**: Pre-industrial, historical, future scenarios

---

## Science Questions and Approaches

### Model Evaluation
- **Compare to observations**: Reanalysis, satellite, in-situ
- **Metrics**: Bias, RMSE, pattern correlation
- **Tools**: E3SM diagnostics packages

### Sensitivity Studies
- **Perturb parameters**: Test model response
- **Isolate processes**: Use simplified configurations
- **Quantify uncertainty**: Ensemble experiments

### Process Studies
- **Focus on mechanisms**: Understand cause and effect
- **Idealized experiments**: Simplify to key processes
- **Attribution**: Separate natural vs. forced changes

---

## Terminology

### Common Acronyms
- **SST**: Sea Surface Temperature
- **LAI**: Leaf Area Index
- **GPP**: Gross Primary Productivity
- **NPP**: Net Primary Productivity
- **AMOC**: Atlantic Meridional Overturning Circulation
- **ENSO**: El Niño-Southern Oscillation
- **GCM**: Global Climate Model
- **ESM**: Earth System Model

### E3SM-Specific Terms
- **Compset**: Component configuration (atmosphere, land, ocean, etc.)
- **Grid/Resolution**: Spatial discretization
- **Namelist**: Runtime configuration file
- **Case**: Specific model configuration and run
- **CIME**: Common Infrastructure for Modeling the Earth

---

## Learning History

### [2026-07-26] Initial Knowledge Base
Created foundational climate science concepts relevant to E3SM modeling.

**Sources:**
- E3SM documentation
- General climate science knowledge
- Earth system modeling principles

---

## Notes for Future Expansion

Topics to add as they come up:
- Specific parameterizations (convection schemes, cloud microphysics, etc.)
- Observational datasets used for validation
- Common biases in E3SM components
- Regional climate features
- Extreme events
- Climate feedbacks
- Statistical methods for analysis

## [2026-08-28] OMEGA Ocean Model - Fact Check for Architecture Diagram

**Context:** User fact-checking architecture diagram claiming OMEGA (Ocean Model for E3SM Global Applications) replaces MPAS-Ocean as GPU-capable ocean component. Verified against E3SM-Project/Omega repo (components/omega) directly.

**Key Information:**
- **Portability layer**: Kokkos, NOT YAKL. Confirmed via `OmegaBuild.cmake` which sets `Kokkos_ENABLE_CUDA`, `Kokkos_ENABLE_HIP`, `Kokkos_ENABLE_SYCL`, `Kokkos_ENABLE_OPENMP`, `Kokkos_ENABLE_SERIAL` depending on detected `OMEGA_ARCH` (CUDA/HIP/SYCL/OPENMP/THREADS/SERIAL). Source dir `src/infra/` has `OmegaKokkos.h`, `OmegaKokkosFlatPar.h`, `OmegaKokkosHiPar.h`. (Note: EAMxx/SCREAM uses YAKL+Kokkos in places for atm; OMEGA is Kokkos-only, C++.) YAKL is referenced only in a commented-out TODO check in the build script, confirming it's not the chosen approach.
- **GPU-capable**: Yes, supports CUDA, HIP, and SYCL backends via Kokkos, plus CPU OpenMP/Threads/Serial fallback. Written in modern C++ (unlike Fortran-based MPAS-Ocean).
- **MPI+GPU model**: `OMEGA_MPI_ON_DEVICE` option (default ON) enables device (GPU) buffers directly in MPI communication (GPU-aware MPI / "MPI on device"), consistent with 1-rank-per-GPU pattern similar to EAMxx. Did not find explicit doc confirming exact rank:GPU ratio, but the GPU-aware MPI + Kokkos device execution space design strongly implies 1 MPI rank per GPU, matching EAMxx/SCREAM convention. **Caveat this as inferred, not explicitly documented.**
- **Status**: OMEGA is explicitly described in official E3SM docs (`docs/index.md` in E3SM repo, dated May 2026 checkout) as "not yet supported" — i.e., still early-development/experimental, not yet a production/default compset option in E3SM. It's described as "the ocean component **planned** for v4 of E3SM" (per components/omega/README.md) — planned, future default, not current default. MPAS-Ocean remains the current production ocean component (used in e.g. CMPASO-NYF compset, still the default reference case in OMEGA's own build scripts for cime integration).
- **I/O**: OMEGA has its own `IOStream` infrastructure (`src/infra/IOStream.h`) distinct from MPAS-Ocean's I/O. Default file format is **pnetcdf** (per config comments: "File format is only needed if differs from default (pnetcdf)"), but also explicitly supports alternate formats including **ADIOS** and HDF5 (`IO::FileFmt` enum includes pnetcdf, hdf5, adios). So: same underlying default (PnetCDF) as MPAS-Ocean/PIO stack, but architecturally OMEGA's IOStream is a new, distinct implementation (not verified whether it wraps PIO2 or calls pnetcdf directly) with added flexibility for ADIOS as an alternative — worth flagging as a possible difference for BP/streaming I/O workflows.
- **Sea ice coupling**: Did not find explicit evidence one way or the other in this quick check about whether OMEGA changes the coupler interaction pattern with MPAS-SI (sea ice). Sea ice (MPAS-SI) is a separate Fortran/MPAS-framework component and is not part of the Omega repo. Reasonable assumption is that near-term OMEGA integration targets the same coupler (CMEPS) interface pattern as MPAS-Ocean, since coupler interfaces are typically standardized at the driver level, but this was NOT explicitly verified in code and should be caveated as unconfirmed.

**Related Concepts:** compset_usage.md (CMPASO-NYF used as OMEGA's default reference case in its own CMake integration - see OmegaBuild.cmake `read_cime_config()`); EAMxx/SCREAM GPU architecture (YAKL/Kokkos hybrid, 1 rank/GPU) for comparison.

**Follow-up:** 
- Should verify actual coupler/sea-ice interaction pattern with someone closer to the OMEGA driver code (components/omega/src/drivers) or E3SM Ocean team docs at docs.e3sm.org/Omega.
- Should verify exact MPI rank:GPU ratio recommendation once OMEGA has published performance/scaling docs or run scripts with explicit PE layouts.
- Recommend caveating diagram: label OMEGA as "in development / not yet production" per E3SM's own docs (as of the E3SM repo state checked, dated ~May 2026).

---

## [2026-10-29] Observation–Model Agreement for Ocean–Atmosphere Fluxes

**Context:** User asked what the research says about agreement between measured and
modeled air–sea fluxes. Literature survey via Consensus (two searches: flux-product
vs. buoy evaluations; eddy-covariance vs. bulk parameterization).

**Key Information:**

- **Component hierarchy of agreement** (best → worst):
  shortwave radiation > state variables (Ta, U, RH) > longwave > momentum/stress >
  latent heat > sensible heat > trace-gas (CO2/DMS) fluxes.
- **Magnitudes:**
  - ORA-IP 16-reanalysis ensemble: global bias +4.2 ± 1.1 W/m²; ensemble spread
    >40 W/m² over western boundary currents; 16 W/m² too little heat gain at Stratus
    buoy, attributed to excess latent cooling from imposed winds.
  - Tropical Indian Ocean Qnet RMSE: ~100 W/m² (NCEP-2) → ~45 W/m² (modern ensemble).
  - TropFlux/OAFlux vs. moorings: r ~0.9, RMSD ~43 W/m². NCEP: r ~0.78, >65 W/m².
  - Northern South China Sea tower: LHF RMSE >90.5 W/m², SHF RMSE >33.4 W/m²,
    momentum underestimated 0.07–0.08 N/m².
  - EC vs. COARE 3.5 (Southern Ocean, 1-hr): 22% (tau), 70% (SHF), 26% (LHF).
- **Error attribution — the central result:** discrepancies are driven mainly by
  *input bulk variables*, not the flux algorithm. Recomputing fluxes from reanalysis
  variables through COARE moves them toward observations. Using observed SST alone
  brought East Asian marginal-sea biases within ~10 W/m². Pacific Arctic study
  explicitly concluded the parameterization scheme was not the dominant cause.
  → Dominant sensitivities: wind speed and specific humidity (LHF), air temperature (SHF), SST.
- **Systematic sign of bias:** nearly all products *overestimate* LHF; most overestimate
  SHF; several *underestimate* momentum flux. COARE 3.5 tends to underestimate at high wind.
- **Gas transfer is the weakest link:** EC-derived transfer velocities scale better with
  friction velocity (u*) than with mean wind speed; SOAP data did not support the sharp
  bubble-mediated high-wind increase predicted by physical models; one shelf-sea study
  found EC CO2 fluxes exceeding bulk estimates by ~an order of magnitude.
- **EC is not a perfect reference:** hourly EC CO2 flux uncertainty ~20% (large fluxes)
  to ~50% (small fluxes), dominated by sampling error, not instrument noise.
  Optimal averaging window 1–3 h. Direct methods are best used to *calibrate the
  coefficients inside* bulk algorithms (COAREG), not to replace them.
- **Ensemble means outperform any single product** (error partially independent across
  products). ERA5 is the best single product in most recent evaluations; higher
  resolution demonstrably helps.
- **Spatial structure of error:** western boundary currents, high southern latitudes,
  coastal/shelf seas, and the marginal ice zone are the divergence hotspots.
  Mesoscale eddies modulate fluxes strongly (tau ~+89%, SHF ~+187% over a warm eddy).
- **Caution:** ESA Ocean Heat Flux assessment found high noise variance in all products
  and only a weak signal in common with in situ observations.

**E3SM Relevance:**
- Observational uncertainty (~40–50 W/m² point-wise RMSE) is a *hard floor* for coupled
  flux validation and may exceed the drift signal being diagnosed. Validate against a
  multi-product ensemble, not a single product.
- Effort to reduce coupled net-heat-flux bias is better spent on the atmospheric boundary
  layer (near-surface wind and humidity) and SST than on retuning the flux scheme itself.
- Regionally refined mesh / coastal configurations should expect worse agreement than
  open-ocean; coastal evaluations are systematically poorer.
- Relevant to EAM–MPAS-Ocean coupling, MPASSI marginal-ice-zone fluxes, and ocean
  biogeochemistry (CO2 uptake) evaluation.

**Related Concepts:** Ocean (MPAS-Ocean), Atmosphere (EAM), Sea Ice (MPASSI),
Carbon Cycle (ocean uptake), Earth System Components above.

**Follow-up / Open Questions:**
- Which flux reference product(s) does the user currently use for E3SM evaluation?
- Is interest in heat/momentum flux coupling, or ocean carbon uptake (BGC) validation?
- Does COARE-family algorithm behavior at high wind matter for the configurations in use
  (Southern Ocean, storm-track regions)?

**Key references:** Valdivieso et al. 2017 (ORA-IP, Clim Dyn); Tang et al. 2023
(Earth-Sci Rev, 15-product review vs 139 buoys); Kumar et al. 2012 (TropFlux, Clim Dyn);
Bentamy et al. 2017 (ESA OHF, RSE); Pokhrel et al. 2020 (Earth Space Sci);
Bharti et al. 2019 (CAPRICORN, JGR-Atmos); Dong et al. 2021 (ACP, EC CO2 uncertainty);
Landwehr et al. 2017 (SOAP, ACP); Fairall et al. 2022 (COAREG trace gas).

---

## [2026-10-29] Southern Ocean & RRM Flux Evaluation: Heat / Momentum / Freshwater

**Context:** Follow-up after user specified focus on heat/momentum/freshwater coupling,
Southern Ocean + regionally refined configurations, and WHOI-OAFlux as the LH/SH reference.
Extends the earlier [2026-10-29] general flux-agreement entry above.

### WHOI-OAFlux as a reference: strengths and caveats

- **Strength:** among the best-performing products vs. independent moorings
  (r ~0.9, RMSD ~43 W/m², vs. r ~0.78 / >65 W/m² for NCEP). Objectively analyzed synthesis,
  so less contaminated by any single model's bias.
- **Caveat 1 - turbulent only.** OAFlux provides LH/SH; radiation must come from
  ISCCP/CERES and precipitation from GPCP or similar. **Do not mix E and P across products
  arbitrarily** - combining reanalysis P with satellite E yields a *negative* global ocean
  water budget (Iwasaki et al. 2013).
- **Caveat 2 - shares the systematic LHF overestimate.** The 139-buoy review found *all*
  products, OAFlux included, overestimate monthly LHF.
- **Caveat 3 - Southern Ocean is weakly constrained.** Only TWO SO flux buoy sites over the
  past decade (south of Tasmania; upstream of Drake Passage). Josey et al. 2023 argue for a
  third in the Atlantic/Indian sector. High southern latitudes are a known product-divergence
  hotspot (Bentamy et al. 2017). => In the SO, OAFlux is largely bulk algorithm over
  reanalysis inputs, not an observationally anchored field. Use as ENSEMBLE MEMBER, not truth.
- **Caveat 4 - mid/high-latitude South Atlantic:** CMIP5 and reanalyses overestimate LH/SH
  vs. OAFlux, plausibly from *underestimated surface wind speed* (Lindemann et al. 2021).

### Southern Ocean: the compensating-error trap (most important finding)

- Hyder et al. 2018 (Nat Comms): SO SST biases across CMIP5 are primarily caused by
  **atmospheric net surface flux bias linked to cloud-related shortwave errors**. Critically,
  **most AMIP5 models with small NET heat flux bias achieve this through COMPENSATING ERRORS.**
  => Tuning/validating on net heat flux alone is actively misleading. Always decompose into
  SW / LW / LH / SH.
- Gao et al. 2024: two distinct mechanisms by latitude.
  - 70°-50°S: warm bias from **excessive summertime net heat flux** (cloud + clear-sky SW).
  - North of 50°S: **dynamical** - underestimated westerlies weaken northward Ekman transport
    and cold advection => year-round warm SST bias. Westerly bias traced to atmosphere-only runs.
- Song et al. 2025: SO warm SST bias can be mitigated by parameterizing **near-inertial
  internal wave (NIW) driven vertical mixing** in the thermocline (eddy-resolving CGCM),
  enhancing downward heat flux out of the surface boundary layer. Alternative pathway to
  blaming surface flux alone.
- **Josey asymmetry metric (2023)** - recommended structural diagnostic: sea-air humidity
  gradient is ~60% larger in the Pacific sector, driving ~20 W/m² stronger latent heat loss
  than the Atlantic/Indian sector. CMIP6 shows weaker Atlantic/Indian heat gain than
  reanalyses. Tests spatial structure, so harder to satisfy via compensating errors than a
  zonal mean.

### Momentum / wind stress

- CMIP6 genuinely improved over CMIP5/CMIP3: **stronger, less equatorward-biased wind stress**,
  improved Drake Passage Ekman upwelling, more models with ACC transport within observational
  uncertainty (Beadling et al. 2020). But upper SO remains **warm and fresh**; several models
  have extremely weak ACC transport.
- Beadling et al. 2019: ACC quality depends jointly on (a) magnitude AND (b) **latitude** of
  the zonal-mean westerly wind stress maximum, (c) wind-driven surface divergence,
  (d) surface buoyancy fluxes, (e) NADW properties entering the SO. Only 8/31 CMIP5 models
  got ACC right "for approximately the right reasons."
- Park et al. 2025: inter-model SO MOC spread in the **upper cell** is governed by
  model-dependent **sensitivity to wind stress position**, NOT wind stress magnitude.
  Latitude of max stress (SAM-like mean state) is the controlling parameter.
  => Diagnose the LATITUDE of the stress maximum, not just its magnitude.

### Freshwater flux - the weakest-constrained of the three

- **Yu et al. 2017 (key numbers):** across 10 reanalyses + 2 satellite E-P + 2 salinity products:
  - E   = 129 ± 10 cm/yr  (8% uncertainty)
  - P   = 118 ± 11 cm/yr  (9% uncertainty)
  - **E-P = 11 ± 4 cm/yr (36% uncertainty)**
  E-P uncertainty exceeds that of E and P by **a factor of 4+** because it is a small residual
  of two large terms. Dominant contributor: **precipitation in the tropical wet zone.**
- Gutenstein et al. 2020: global ocean mean E-P agrees within uncertainty across products, but
  **regional differences are dominated by P everywhere**; spread among satellite E, and between
  satellite P products, is **larger than E-P itself**. ERA5 performs best for budget closure
  (R²=0.86 for E_ocean - VIMD vs P_ocean).
- Liu et al. 2022: uncertainty 10% in FWF vs 15% in SSS; spread across E-P-SSS product
  combinations prevents using any single combination for a salinity budget.
- Iwasaki et al. 2013: reanalysis E biased high vs satellite in tropics/subtropics, traced to
  **low air specific humidity** and bulk-algorithm differences.
- **Park et al. 2025:** inter-model differences in the **LOWER overturning cell** (AABW
  formation) are largely driven by variations in **surface buoyancy flux - primarily meltwater**
  - and mean stratification.
  => For SO water-mass formation, freshwater forcing (E-P + meltwater) is likely a LARGER
  structural uncertainty than heat flux.

### Regionally refined mesh (RRM) specific

- **Zeller et al. 2024 (Ocean Modelling) - critical for RRM spin-up:** compared coupled 1/2°
  vs 1/10° SO configurations.
  - Initialized from observed state, only the NON-EDDYING config rapidly built a strong warm bias.
  - Initializing the EDDYING config from the biased non-eddying state caused **immense spurious
    open-ocean deep convection**, with an undesirable imprint on global circulation.
  - Conclusion: **eddying/refined configurations require their OWN spin-up**; do not hand off a
    coarse-resolution equilibrated state.
  - A warmer SO => stronger Agulhas, weaker ACC (reduced meridional density gradient 45-65°S).
    Eddying runs show stronger subpolar gyres when warmer; non-eddying response is inconsistent.
- **Reference-product resolution mismatch:** mesoscale eddies modulate fluxes strongly
  (tau ~+89%, SHF ~+187% over a warm eddy; Bharti et al. 2019). An RRM region resolves air-sea
  feedbacks that a coarse objectively-analyzed product like OAFlux cannot represent.
  => Some apparent "bias" in the refined region is MISSING VARIANCE IN THE REFERENCE,
  not model error. Compare like-for-like spatial scales before attributing bias.
- Coastal/shelf evaluations are systematically worse than open ocean - relevant if refinement
  extends onto shelves.

### Recommended evaluation strategy (heat / momentum / freshwater)

| Flux        | Reference approach                              | Key diagnostic                          | Expected floor |
|-------------|-------------------------------------------------|-----------------------------------------|----------------|
| LH / SH     | OAFlux + ERA5 + satellite/hybrid as ensemble     | Decompose; never tune on net alone      | ~40-50 W/m²    |
| Shortwave   | CERES, evaluated separately                      | SO summertime SW is the known culprit   | -              |
| Momentum    | Reanalysis ensemble                              | LATITUDE of stress max, not magnitude   | -              |
| E - P       | Consistent E/P pairs, never mixed across products| Tropical P dominates error              | ~36%           |
| Structure   | Josey zonal asymmetry metric                     | Pacific vs Atlantic/Indian sector        | -              |

**Related Concepts:** [2026-10-29] Observation-Model Agreement for Ocean-Atmosphere Fluxes
(entry above); Ocean (MPAS-Ocean); Sea Ice (MPASSI, meltwater); Atmosphere (EAM, cloud/SW).

**Follow-up / Open Questions:**
- Which E3SM version/compset and RRM region? (determines whether Zeller spin-up caution applies)
- Is the RRM region Southern Ocean / Antarctic shelf, or elsewhere?
- Are ice-shelf meltwater fluxes active (relevant to AABW / lower cell)?
- Is CERES or ISCCP paired with OAFlux for the radiative terms?

**Key references:** Hyder et al. 2018 (Nat Comms); Beadling et al. 2020 (J Clim, CMIP3-6);
Beadling et al. 2019 (J Clim, ACC); Gao et al. 2024 (J Ocean Univ China); Josey et al. 2023
(Phil Trans R Soc A); Park et al. 2025 (npj Clim Atmos Sci); Zeller et al. 2024 (Ocean Modelling);
Song et al. 2025 (GRL, NIW mixing); Yu 2019 (Ann Rev Mar Sci, budget closure);
Yu et al. 2017 (J Clim, E-P uncertainty); Gutenstein et al. 2020 (HESS);
Iwasaki et al. 2013 (RSE); Liu et al. 2022 (Remote Sens); Lindemann et al. 2021 (South Atlantic).
