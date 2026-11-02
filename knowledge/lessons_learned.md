# Lessons Learned

Insights, mistakes, corrections, and improvements discovered through interactions.

## Format

Document what worked, what didn't, and what was learned. Focus on actionable insights.

---

## Categories

### Model Configuration Lessons
None yet recorded.

### Scientific Insights
None yet recorded.

### Workflow Improvements
None yet recorded.

### Communication Lessons
None yet recorded.

### Technical Discoveries
None yet recorded.

---

## Detailed Lessons

### [2026-07-26] Knowledge Base Initialization

**Context:** Setting up the scientific assistant with persistent learning capability.

**What Worked:**
- Structured markdown files for different knowledge domains
- Clear separation of concerns (experiments, concepts, preferences, lessons)
- Template-driven documentation for consistency
- Integration with Goose recipe system

**Design Decisions:**
- File-based knowledge storage for transparency and version control
- Markdown format for readability and tool compatibility
- Separate index for quick reference
- Date-stamped entries for temporal context

**Future Improvements:**
- May need search/indexing as knowledge grows
- Consider periodic consolidation to avoid sprawl
- Add cross-referencing between related entries
- Implement archival strategy for old information

**Principle Established:** 
Learning should be:
1. Structured (predictable format)
2. Contextual (include why, not just what)
3. Incremental (build on past knowledge)
4. Actionable (guide future decisions)

---

## Templates

### Success Template
```markdown
## [YYYY-MM-DD] What Worked: [Topic]

**Context:** [Situation]

**Approach:** [What was done]

**Outcome:** [Result]

**Why It Worked:** [Analysis]

**Apply To:** [Similar situations where this applies]
```

### Failure Template
```markdown
## [YYYY-MM-DD] What Didn't Work: [Topic]

**Context:** [Situation]

**Approach:** [What was tried]

**Problem:** [What went wrong]

**Root Cause:** [Why it failed]

**Better Approach:** [What to do instead]

**Warning Signs:** [How to avoid this in future]
```

### Insight Template
```markdown
## [YYYY-MM-DD] Insight: [Discovery]

**Context:** [How this came up]

**Discovery:** [What was learned]

**Implications:** [Why this matters]

**Applications:** [Where to use this knowledge]

**Related Concepts:** [Links to other knowledge]
```

---

## Reflection Guidelines

After significant interactions, ask:
1. What worked well in this interaction?
2. What could have been done better?
3. What new knowledge was gained?
4. Are there patterns emerging across multiple interactions?
5. What questions remain unanswered?

---

## Update Triggers

Add a lesson when:
- **Mistake identified**: User corrects or clarifies something
- **New workflow discovered**: More efficient way to accomplish a task
- **Unexpected behavior**: Model/system does something surprising
- **Successful pattern**: Approach works well, should be repeated
- **Clarification needed**: Topic requires better explanation next time
- **Tool limitation found**: Feature gaps or workarounds discovered

---

## Cross-References

Link lessons to:
- Related experiments (e3sm_experiments.md)
- Relevant concepts (climate_concepts.md)
- User preferences (user_preferences.md)
- Compset selection (compset_usage.md)

---

## Periodic Review

Monthly (or every ~20 interactions):
1. Review all lessons
2. Identify recurring themes
3. Consolidate related lessons
4. Update other knowledge files with insights
5. Archive or remove outdated lessons

---

## Principles for Learning

### Good Lessons Are:
- **Specific**: Include concrete details and examples
- **Contextual**: Explain the situation and constraints
- **Analytical**: Provide reasoning, not just facts
- **Actionable**: Guide future decisions
- **Honest**: Acknowledge mistakes and uncertainty

### Avoid:
- Vague generalizations without examples
- Lessons without context
- Blame or judgment
- Overfitting to single instances
- Lessons that don't change future behavior

---

## Notes

This is arguably the most important knowledge file. While the others record **what**, this file records **why** and **how**. It's where the assistant truly learns to improve over time.

---

## [2026-10-29] Net-Flux Validation Can Hide Compensating Errors

**Context:** User evaluates E3SM air-sea coupling against WHOI-OAFlux in Southern Ocean
and regionally refined configurations.

**Lesson:** Validating or tuning a coupled model on **net** surface heat flux is unreliable.
Hyder et al. 2018 showed most CMIP5-era atmospheric models achieving a small net heat flux
bias in the Southern Ocean did so through **compensating errors** between shortwave and
turbulent components. A model can match the reference in the net while still distorting
upper-ocean thermal structure.

**Practice to adopt:**
1. Always decompose into SW / LW / LH / SH before declaring agreement.
2. Use structural metrics that are hard to satisfy by compensation - e.g. the Josey
   Pacific vs. Atlantic/Indian sector heat-flux asymmetry.
3. Treat any single flux product (including OAFlux) as an ensemble member in the Southern
   Ocean, where only two validating flux buoys exist.

---

## [2026-10-29] Reference-Product Resolution Mismatch in RRM Evaluation

**Context:** Same as above - regionally refined mesh configurations.

**Lesson:** Coarse, objectively-analyzed reference products cannot represent mesoscale
air-sea feedbacks that a refined mesh resolves. Observed eddy-scale modulation is large
(momentum flux ~+89%, SHF ~+187% over a warm-core eddy). Apparent "bias" in a refined
region may be **missing variance in the reference product**, not model error.

**Practice to adopt:** Coarse-grain model output to the reference product's effective
resolution before computing bias statistics in refined regions, and report the scale
at which the comparison was made.

---

## [2026-10-29] Do Not Mix E and P Across Products

**Context:** Freshwater flux evaluation.

**Lesson:** E-P is a small residual of two large terms; its uncertainty (~36%) exceeds that
of E (~8%) and P (~9%) by a factor of four or more. Combining reanalysis precipitation with
satellite evaporation has been shown to yield a *negative* global ocean water budget
(Iwasaki et al. 2013).

**Practice to adopt:** Use internally consistent E/P pairs from a single product family,
and state the pairing explicitly in any budget analysis.

---

## [2026-10-29] Eddying/Refined Configurations Need Their Own Spin-Up

**Context:** RRM / high-resolution Southern Ocean configurations.

**Lesson:** Zeller et al. 2024 found that initializing a 1/10° coupled configuration from a
biased non-eddying (1/2°) equilibrated state produced **immense spurious open-ocean deep
convection** with a global circulation imprint, because the coarse-resolution biased state
is not stable at eddying resolution.

**Practice to adopt:** Budget for a dedicated spin-up at the target resolution rather than
branching from a coarse-mesh equilibrium; watch for open-ocean deep convection as the
diagnostic symptom if branching is unavoidable.

