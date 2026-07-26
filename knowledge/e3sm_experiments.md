# E3SM Experiments Knowledge Base

Record of E3SM experiments planned, executed, and their outcomes.

## Format

Each experiment entry should include:
- Date planned/executed
- Science question/hypothesis
- Configuration (compset, resolution, machine)
- Key namelist or XML modifications
- Results or status
- Lessons learned
- Links to related experiments

---

## Experiments

### [2026-07-26] Knowledge Base Initialization

**Context:** Setting up the scientific assistant knowledge base.

**Note:** This file will be populated as experiments are planned and executed.

---

## Templates

### Planning Template
```markdown
## [YYYY-MM-DD] Experiment Name

**Science Question:** What hypothesis is being tested?

**Configuration:**
- Compset: [name and rationale]
- Resolution: [choice and reasoning]
- Machine: [computing platform]
- Case name: [CASENAME]
- Run length: [simulation period]

**Modifications:**
- Namelist changes: [if any]
- XML variable changes: [if any]
- Code modifications: [components and files]

**Expected Outcomes:**
[What variables/outputs to analyze]

**Status:** Planned | Running | Complete | Failed

**Related Experiments:**
[Links to similar experiments]
```

### Results Template
```markdown
## [YYYY-MM-DD] Experiment Name - Results

**Outcomes:**
- [Key findings]
- [Performance metrics]
- [Unexpected behaviors]

**Data Location:**
- Run directory: [path]
- Archive: [path]

**Analysis:**
[Scientific interpretation]

**Lessons Learned:**
- [What worked well]
- [What to improve]
- [Suggestions for future experiments]

**Follow-up Questions:**
[Areas to explore further]
```
