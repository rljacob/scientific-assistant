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
