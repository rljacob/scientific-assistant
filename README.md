# Scientific Assistant - Climate Science

A learning scientific assistant specialized in E3SM climate modeling and experiment planning.

## Description

This recipe provides a persistent, learning assistant for climate science research using E3SM. The assistant maintains a knowledge base that grows over time, helping with experiment planning, model configuration, and scientific analysis.

## Features

- **Experiment Planning**: Guides E3SM experiment design with appropriate compsets and resolutions
- **Continuous Learning**: Updates knowledge base after each interaction
- **Context Awareness**: References past experiments and user preferences
- **E3SM Integration**: Deep understanding of E3SM codebase and workflows
- **Structured Knowledge**: Maintains organized markdown files for different knowledge domains

## Knowledge Base

Located at `~/.config/goose/recipes/scientific-assistant/knowledge/`:

- **e3sm_experiments.md**: Past experiments, configurations, and results
- **compset_usage.md**: Compset selection patterns and rationale
- **climate_concepts.md**: Climate science domain knowledge
- **user_preferences.md**: Research patterns and preferences
- **lessons_learned.md**: Insights from past interactions
- **index.md**: Cross-references and quick lookup

## Setup

For detailed setup instructions including GitHub sync and HPC installation, see **[SETUP.md](SETUP.md)**.

**Quick setup:**
```bash
# On local machine - initialize git and push to GitHub
cd ~/.config/goose/recipes/scientific-assistant
./sync-knowledge.sh

# On HPC machine - clone and install goose
pip install --user goose-ai
mkdir -p ~/.config/goose/recipes
cd ~/.config/goose/recipes
git clone git@github.com:YOUR_USERNAME/scientific-assistant.git
```

## Usage

## Recipe File Format

Goose supports two recipe formats:

1. **recipe.yaml** (YAML format - required for `goose run`)
2. **recipe.md** (Markdown format - for newer Goose versions)

This recipe includes both formats with identical content. Use whichever your Goose version requires.

### Start the Assistant

**Current Goose version (local):**
```bash
goose session --recipe scientific-assistant
```

**Older Goose version (if on HPC):**
```bash
goose run --recipe scientific-assistant
```

> **Note:** Older Goose versions use `goose run --recipe` while newer versions support `goose session --recipe`. Use whichever works on your machine.

### Sync Across Machines

```bash
# Before and after work on any machine
cd ~/.config/goose/recipes/scientific-assistant
./sync-knowledge.sh
```

### Example Interactions

**Planning an experiment:**
```
User: I need to test my ELM carbon cycle changes
Assistant: [Loads past experiments and compset knowledge]
          Recommends specific configuration with rationale
          Documents the plan in knowledge base
```

**Learning from results:**
```
User: The experiment showed unexpected soil moisture patterns
Assistant: [Updates lessons_learned.md and climate_concepts.md]
          References this in future similar situations
```

**Referencing past work:**
```
User: What did we use for testing river routing before?
Assistant: [Searches e3sm_experiments.md]
          Provides past configuration and outcomes
```

## How It Learns

The assistant updates its knowledge base after significant interactions by:

1. **Extracting new information** from the conversation
2. **Categorizing** into appropriate knowledge files
3. **Structuring** with dates and context
4. **Cross-referencing** related concepts
5. **Reflecting** on what worked well

## Workflow

### Session Start
1. Loads relevant knowledge based on context
2. References past experiments if applicable
3. Ready to assist with current task

### During Interaction
1. Provides expertise from both training and knowledge base
2. Suggests configurations based on past successes
3. Asks clarifying questions when needed

### Session End
1. Updates knowledge files with new information
2. Documents experiments, preferences, or insights
3. Cross-references related knowledge

## Knowledge Update Guidelines

Updates are made when:
- Planning or completing an experiment
- User explains a concept or correction
- User expresses a preference or pattern
- Learning from a mistake or clarification
- Discovering a useful workflow or approach

## Customization

Edit the recipe file to customize:
- Knowledge file structure
- Update frequency
- Communication style
- Domain focus areas

## Requirements

- Access to E3SM codebase
- Goose developer extension (for shell, file operations)
- Supported E3SM computing machine (for experiment execution)

## Tips

- Be explicit about preferences to help the assistant learn
- Reference past experiments by date or name
- Ask the assistant to explain its reasoning
- Provide feedback on suggestions to improve future recommendations

## Maintenance

Periodically (every 1-2 months):
- Review knowledge files for outdated information
- Consolidate related entries
- Archive old experiments
- Update compset patterns based on new E3SM releases

## Version

1.0.0 - Initial release (2026-07-26)

## Author

Created for climate science research with E3SM
