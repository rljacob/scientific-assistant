# Compset Usage Patterns

Guidelines and rationale for selecting E3SM compsets based on science questions.

## Compset Selection Framework

### By Component Focus

#### Testing ELM (Land) Only
- **I1850CNPRDCTCBCTOP**: Full carbon-nitrogen with permafrost and CTBC fire
  - Use when: Testing carbon cycle, nitrogen dynamics, permafrost processes
  - Resolution: ne4pg2_ne4pg2 (fast testing)
  
- **I1850GSWCNPRDCTCBC**: Similar with different physics
  - Use when: Comparing land physics configurations

#### Testing EAM (Atmosphere) Only
- **F1850**: Atmosphere-land coupled, prescribed ocean/ice
  - Use when: Testing atmospheric physics, aerosols, clouds
  - Resolution: ne4pg2_oQU480 (low res for testing)

#### Testing EAMxx (SCREAM)
- **F2010-SCREAMv1**: Next-gen atmosphere model
  - Use when: Working on EAMxx/SCREAM code
  - Resolution: ne4pg2_ne4pg2

#### Testing MOSART (River Routing)
- **RMOSGPCC**: River routing only
  - Use when: Testing river routing code
  - Resolution: r05_r05

#### Testing MPAS-Ocean
- **CMPASO-NYF**: Ocean-ice coupled with normal year forcing
  - Use when: Testing ocean physics
  - Resolution: T62_oQU120

#### Testing MPAS-SeaIce
- **DTESTM**: Data ocean with MPAS sea ice
  - Use when: Testing sea ice code
  - Resolution: T62_oQU240

### Multi-Component Testing

#### Two Components Modified (ELM + EAM)
- **F1850**: Atmosphere-land coupled
  - Covers both components efficiently

#### Multiple Components Modified
- **WCYCL1850NS**: Fully coupled water cycle (no stub)
  - Use when: Changes affect 2+ components beyond ELM+EAM
  - Resolution: ne4pg2_r05_oQU480
  - Warning: Slower, more complex

### By Time Period

#### Pre-industrial (1850)
- **\*1850\***: Pre-industrial CO2 and forcing
  - Use when: Long-term climate, equilibrium studies

#### Historical
- **\*HIST**: Transient historical forcing (1850-present)
  - Use when: Comparing to observations, attribution

#### Present-day (2000/2010)
- **\*2000 or \*2010**: Modern CO2 and forcing
  - Use when: Current climate, short-term validation

#### Future Scenarios
- **\*SSP\***: Shared Socioeconomic Pathways
  - Use when: Climate projections

### By Physics Options

Look for compset suffixes:
- **RDCTCBC**: Full chemistry and carbon cycle
- **ECO**: Ecosystem dynamics
- **BGC**: Biogeochemistry
- **PT**: Prescribed tracers
- **CMIP6**: CMIP6-configured physics

## Resolution Pairing Strategies

### Fast Testing
- Atmosphere/Land: ne4pg2_ne4pg2
- Ocean: oQU480 or oQU240
- River: r05

### Production/Science Runs
- Atmosphere/Land: ne30pg2 or ne120pg2
- Ocean: oEC60to30 or higher
- River: r05

### Performance Testing
- Match production resolution
- Consider node/core count constraints

## Common Patterns

### Code Development Cycle
1. **Development**: Use lowest resolution (ne4pg2)
2. **Verification**: Short run at target resolution
3. **Testing**: System tests (SMS, ERS)
4. **Production**: Full resolution, long integration

### Cross-Component Impact Assessment
1. Start with single component (stub others)
2. Add coupled components incrementally
3. Full system last

## Selection Checklist

When choosing a compset, ask:
1. Which components have code changes?
2. What time period is scientifically relevant?
3. What processes must be active (carbon, chemistry, etc.)?
4. What resolution balances science needs and computational cost?
5. Are there similar past experiments to reference?

---

## Usage History

### [2026-07-26] Initial Framework
Created compset selection guidelines based on E3SM documentation and development workflows.

---

## Notes

- Compset definitions: `cime_config/allactive/config_compsets.xml`
- Query available: `cd cime/scripts && ./query_config --compsets all`
- Machine constraints matter: Some machines may not support all compsets
