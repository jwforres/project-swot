# Project SWOT Analysis Skill

A comprehensive Claude Code skill for conducting in-depth SWOT (Strengths, Weaknesses, Opportunities, Threats) analysis of technology projects.

## Overview

This skill guides you through a structured methodology for evaluating technology projects, frameworks, platforms, or open-source tools. It uses forked subagents to prevent bias, incorporates human validation at key checkpoints, and produces professional reports and presentations.

## Use Cases

- **Adoption decisions**: "Should we adopt Project X?"
- **Investment tracking**: "What's the state of Project X we've been tracking?"
- **Competitive intelligence**: Understanding competitor tooling or competing projects
- **Dependency audits**: Assessing risk of existing dependencies
- **General research**: Exploring the technology landscape

## Output

The skill automatically generates four deliverables:

1. **Detailed Markdown Report** (`[project]_swot_report.md`) - Comprehensive analysis with sources
2. **PDF Report** (`[project]_swot_report.pdf`) - Professional PDF version
3. **Executive Presentation** (`[project]_swot_overview.pptx`) - 5-slide PowerPoint deck
4. **Presentation PDF** (`[project]_swot_overview.pdf`) - PDF version of slides

## Software Requirements

### macOS (via Homebrew)
```bash
brew install pandoc tectonic
brew install --cask libreoffice
pip3 install python-pptx
```

### Fedora/RHEL (via dnf)
```bash
sudo dnf install -y pandoc pandoc-pdf libreoffice python3 python3-pip
pip3 install --user python-pptx
```

### Ubuntu/Debian (via apt)
```bash
sudo apt install -y pandoc texlive-xetex libreoffice python3-pip
pip3 install --user python-pptx
```

### Verification
```bash
which pandoc soffice python3
python3 -c "import pptx; print('python-pptx installed')"
```

## Installation

1. Clone this repository into your Claude Code skills directory:
   ```bash
   cd ~/.claude/skills/
   git clone https://github.com/[your-username]/project-swot.git
   ```

2. Install required software (see Software Requirements above)

3. Use the skill in Claude Code:
   ```
   /project-swot
   ```

## Workflow

The skill guides you through five phases:

1. **Evaluation Context** - Define your purpose and relationship to the project
2. **Project Discovery** - Web search for current project state with human validation
3. **SWOT Gathering** - Forked subagents analyze positive and negative aspects separately
4. **Context Integration** - Provide organizational factors and weight findings
5. **Final Output** - Generate comprehensive reports and presentations

## Key Features

### Bias Prevention
- **Forked subagents**: Separate analysis of strengths/opportunities vs weaknesses/threats prevents anchoring bias
- **Fresh data**: All metrics fetched via web search, not training data
- **Verification tags**: Claims tagged as [VERIFIED] or [CLAIMED] with sources
- **Human checkpoints**: Validation required at each phase

### AI Pitfall Prevention
- Avoids outdated knowledge by always using web search
- Translates marketing language to technical specifics
- Fetches fresh metrics with retrieval dates
- Analyzes complete architecture, not just API surface
- Explicitly researches project governance

### Professional Output
- Executive summary with bottom-line recommendation
- Risk assessment matrix with likelihood and impact
- Trajectory analysis (ascending, stable, or declining)
- Comprehensive source documentation
- AI-generated disclaimer on all outputs

## Example Usage

```
User: /project-swot
Claude: What specific project, tool, or technology do you want to evaluate?
User: https://github.com/example/awesome-project
Claude: What's the purpose of this evaluation?
...
```

The skill will guide you through the analysis and automatically generate all four output files.

## License

MIT License - See LICENSE file for details

## Contributing

Contributions welcome! Please feel free to submit issues or pull requests.

## Acknowledgments

This skill is designed to work with [Claude Code](https://github.com/anthropics/claude-code), Anthropic's official CLI for Claude.
