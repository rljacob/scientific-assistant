# HPC Machine Information

Machine-specific configurations, paths, and preferences for E3SM work.

## Format

Document machine-specific details to help with cross-machine workflows.

---

## Machines

### Local Development Machine

**Hostname:** (to be recorded on first use)

**Purpose:** 
- Code development
- Documentation
- Analysis
- Planning

**E3SM Setup:**
- E3SM path: /Users/jacob/E3SM
- Cannot run cases (not a supported machine)

**Notes:**
- Use for code editing and planning
- Transfer to HPC for actual runs

---

### HPC Machines

#### [Machine Name - To Be Added]

**Hostname:** 

**Account/Project:**

**E3SM Configuration:**
- Machine name in CIME: 
- Compiler preferences:
- Queue/partition preferences:
- Typical node allocation:

**File Locations:**
- E3SM checkout:
- Case directory:
- Archive location:
- Scratch space:

**Batch System:**
- Type: (SLURM, PBS, etc.)
- Typical wait times:
- Node hours allocation:

**Preferences:**
- Default compsets:
- Default resolutions:
- Typical run lengths:

**Quirks/Notes:**
- Special module requirements:
- Known issues:
- Performance tips:

---

## Sync Strategy

**Current Approach:** GitHub-based sync with Goose on both machines

**Setup:**
- Git repository: (to be added - your GitHub URL)
- Sync script: `~/.config/goose/recipes/scientific-assistant/sync-knowledge.sh`
- Goose installed: Local ✓, HPC (to be installed)

**Sync Commands:**
```bash
# Before work - get latest
cd ~/.config/goose/recipes/scientific-assistant
./sync-knowledge.sh

# After work - push updates
./sync-knowledge.sh
```

**Workflow:**
1. Local: Plan experiments with Goose → sync
2. HPC: Pull sync → run experiments → use Goose to document → sync
3. Local: Pull sync → Goose now knows about HPC results
4. Both machines can use Goose to update knowledge
5. Git keeps everything synchronized

**Automation:**
- Manual sync: Run `./sync-knowledge.sh` before/after work
- Optional cron: Can set up hourly sync on HPC
- Git hooks: Can auto-push on commit (see SETUP.md)

---

## Cross-Machine Workflows

### Code Development → Testing → Production

**Development (Local):**
1. Edit code in E3SM directory
2. Commit changes
3. Plan experiment with assistant

**Testing (HPC):**
1. Sync code and knowledge base
2. Create test case (low resolution)
3. Run and verify
4. Update knowledge base with results

**Production (HPC):**
1. Create production case
2. Submit long run
3. Monitor progress
4. Document outcomes

---

## Machine-Specific Learning

### [Machine Name] Performance Notes
- To be populated

### [Machine Name] Common Issues
- To be populated

---

## Update History

### [2026-07-26] Initial Machine Knowledge File
Created to handle multi-machine workflows.

**Next Steps:**
- Add specific HPC machine details on first use
- Document sync strategy
- Record machine-specific preferences
