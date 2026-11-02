# Knowledge Base Index

Quick reference for the Climate Science Assistant's knowledge base.

## Last Updated
2026-07-26

## Files Overview

### e3sm_experiments.md
Past and planned E3SM experiments, their configurations, and outcomes.

**Quick Links:**
- No experiments recorded yet

### compset_usage.md
Patterns and rationale for compset selection across different science questions.

**Key Compsets:**
- I1850CNPRDCTCBCTOP: Land-only with full carbon-nitrogen
- F1850: Atmosphere-land coupled
- WCYCL1850NS: Fully coupled water cycle

### climate_concepts.md
Climate science domain knowledge, definitions, and explanations.

**Topics:**
- No concepts recorded yet

### user_preferences.md
User's research patterns, preferences, and workflow habits.

**Preferences:**
- No preferences recorded yet

### lessons_learned.md
Insights, mistakes, and improvements discovered through interactions.

**Key Lessons:**
- No lessons recorded yet

---

## Shared Knowledge (outside this directory)

### ~/.config/goose/shared-knowledge/machines.md
HPC machine information (hostnames, batch systems, compilers, E3SM machine config, etc.), shared with the `swe-hpc-assistant` recipe. Not duplicated here - see that file directly for machine details.

---

## Quick Lookup

- **Air–sea / ocean–atmosphere flux observation-vs-model agreement** →
  `climate_concepts.md`, entry [2026-10-29]. Covers LHF/SHF/momentum/CO2 flux biases,
  COARE algorithm vs. input-variable error attribution, reanalysis product rankings
  (ERA5 best single; ensemble best overall), and E3SM validation implications.
- **Southern Ocean + RRM flux evaluation (heat/momentum/freshwater)** →
  `climate_concepts.md`, entry [2026-10-29] "Southern Ocean & RRM Flux Evaluation".
  OAFlux caveats, compensating-error trap, wind-stress latitude vs. magnitude,
  E−P 36% uncertainty, meltwater and AABW, RRM spin-up, evaluation strategy table.
- **Evaluation pitfalls / methodology lessons** → `lessons_learned.md`, four entries
  dated 2026-10-29: net-flux compensating errors; reference-product resolution mismatch;
  never mix E and P across products; eddying configs need their own spin-up.
- **User's reference datasets and focus** → `user_preferences.md`: WHOI-OAFlux for LH/SH;
  heat/momentum/freshwater coupling; Southern Ocean + RRM configurations.

---

## Search Tips

Use grep to search across knowledge:
```bash
cd ~/.config/goose/recipes/scientific-assistant/knowledge
grep -r "search term" .

# To also search shared machine knowledge:
grep "search term" ~/.config/goose/shared-knowledge/machines.md
```

## Maintenance Log

- 2026-07-26: Initial knowledge base created
- 2026-08-19: Removed local machines.md; machine knowledge consolidated into ~/.config/goose/shared-knowledge/machines.md (shared with swe-hpc-assistant)
- 2026-10-29: Added literature synthesis on observation–model agreement for ocean–atmosphere fluxes to climate_concepts.md
- 2026-10-29: Added Southern Ocean/RRM flux evaluation entry, 4 lessons-learned entries, and populated user research focus (heat/momentum/freshwater coupling, SO + RRM, OAFlux)
