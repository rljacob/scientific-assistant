# User Preferences and Patterns

Track the user's research patterns, preferences, and workflow habits to provide personalized assistance.

## Format

Document preferences, habits, and patterns observed through interactions.

---

## Research Focus

### Primary Domains
- **Air-sea coupling: heat, momentum, and freshwater fluxes** (stated 2026-10-29).
  Explicitly NOT primarily ocean biogeochemistry / CO2 flux - physical coupling is the focus.
- Southern Ocean.

### Secondary Interests
- Observation-model agreement / model evaluation methodology for surface fluxes.

### Current Projects
- Southern Ocean configurations.
- Regionally refined mesh (RRM) configurations.

---

## E3SM Workflow Preferences

### Preferred Machines
- Not yet specified

### Typical Configurations
- Components: coupled atmosphere-ocean (EAM / MPAS-Ocean); sea ice likely relevant
  given Southern Ocean focus (MPASSI, meltwater / buoyancy forcing).
- Resolutions: **regionally refined meshes (RRM)** in use, plus Southern Ocean configurations.
- Run lengths: To be determined

### Testing Approach
- To be documented

---

## Communication Preferences

### Explanation Style
- Level of detail: To be determined
- Technical depth: To be determined
- Preference for examples: To be determined

### Documentation Style
- Preferred format: To be determined
- Code comments: To be determined

---

## Common Questions/Tasks

### Recurring Topics
- None yet identified

### Frequent Workflows
- None yet identified

---

## Tools and Extensions

### Frequently Used Tools
- To be documented

### Preferred Analysis Methods
- **Reference dataset: WHOI OAFlux** for surface latent and sensible heat flux over ocean
  (stated 2026-10-29). Note OAFlux is turbulent-flux only - radiation and precipitation
  must come from separate products (ISCCP/CERES, GPCP etc.).

---

## Time and Scheduling Patterns

### Typical Work Hours
- Not yet observed

### Project Timelines
- To be documented

---

## Learning Style

### How User Learns Best
- To be observed

### Areas of Expertise
- To be identified

### Knowledge Gaps
- To be identified

---

## Feedback and Corrections

### Misconceptions Corrected
- None yet

### Preferred Terminology
- To be documented

---

## History

### [2026-07-26] Initial Setup
Created user preferences file. Will populate as patterns emerge through interactions.

**Note:** This file should be updated regularly but respectfully. Focus on:
- Work patterns that improve assistance
- Scientific preferences that inform suggestions
- Communication styles that enhance clarity
- Avoid overly personal or sensitive information

---

## Update Guidelines

Add entries when you observe:
1. **Repeated behaviors** (uses same machine, same resolution pattern, etc.)
2. **Stated preferences** (user explicitly says "I prefer...")
3. **Workflow patterns** (typical sequence of tasks)
4. **Communication feedback** (asks for more/less detail)
5. **Technical preferences** (naming conventions, directory structure, etc.)

Do NOT add:
- Speculative information
- Overly personal details
- One-time occurrences (wait for patterns)
- Assumptions without evidence

---

### [2026-10-29] Stated Research Focus

**Context:** Follow-up to a literature query on observation-model agreement for air-sea fluxes.

**Stated preferences:**
- Primary interest is **heat / momentum / freshwater coupling** (physical air-sea coupling),
  explicitly chosen over the ocean-carbon / BGC branch when offered both.
- Runs **Southern Ocean** and **regionally refined** configurations.
- Uses **WHOI-OAFlux** as the reference for surface latent and sensible heat flux over ocean.

**Implications for future assistance:**
- Default to physical-coupling framing (surface buoyancy flux, wind stress, Ekman transport,
  water-mass formation, ACC/MOC) rather than carbon-cycle framing.
- When discussing flux evaluation, surface OAFlux caveats relevant to the Southern Ocean
  (sparse validating buoys, turbulent-only coverage, shared LHF overestimate).
- RRM-specific concerns are in scope: spin-up, initialization from coarse state,
  resolved mesoscale air-sea feedback vs. coarse reference products.
