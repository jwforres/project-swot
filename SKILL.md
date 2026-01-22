---
name: project-swot
description: Interactive SWOT analysis for evaluating a single technology project. Use when the user wants to assess a specific tool, framework, platform, or open-source project they're tracking — for adoption decisions, investment evaluation, competitive intelligence, or ongoing monitoring. Guides through structured questioning, uses forked subagents for unbiased positive/negative analysis, and produces a comprehensive project assessment with actionable recommendations.
---

# Single-Project SWOT Analysis Skill

A guided methodology for evaluating one technology project in depth, with bias protections and human checkpoints.

## When to Use

- "Should we adopt Project X?"
- "What's the state of Project X we've been tracking?"
- "Evaluate this framework for our use case"
- "Is Project X mature enough for production?"
- "Help me understand the risks of depending on Project X"

## Software Requirements

This skill generates markdown reports, PDFs, and PowerPoint presentations. The following software must be installed:

### Required Packages

**macOS (via Homebrew):**
```bash
brew install pandoc tectonic
brew install --cask libreoffice
pip3 install python-pptx
```

**Fedora/RHEL (via dnf):**
```bash
sudo dnf install -y pandoc pandoc-pdf libreoffice python3 python3-pip
pip3 install --user python-pptx
```

**Ubuntu/Debian (via apt):**
```bash
sudo apt install -y pandoc texlive-xetex libreoffice python3-pip
pip3 install --user python-pptx
```

### Package Details

1. **pandoc** - Converts markdown to PDF
2. **tectonic** (macOS) or **pandoc-pdf/texlive** (Linux) - LaTeX engine for PDF generation
3. **libreoffice** - Converts PowerPoint (PPTX) to PDF via `soffice` command
4. **python-pptx** - Python library for creating PowerPoint presentations

### Verification

After installation, verify all tools are available:
```bash
which pandoc tectonic soffice python3
python3 -c "import pptx; print('python-pptx installed')"
```

## Workflow Phases

```
Phase 0: Evaluation Context  → Human defines purpose and relationship
Phase 1: Project Discovery   → Web search + Human validates
Phase 2: SWOT Gathering      → FORKED subagents (positive vs negative)
Phase 3: Context Integration → Human provides organizational factors
Phase 4: Implications        → Synthesize + Human validates
Phase 5: Output              → Final report with recommendations
```

## AI Pitfalls to Actively Prevent

Guard against these documented failure modes:

| Pitfall | Prevention |
|---------|------------|
| **Outdated Knowledge** | Always web search; never rely on training data for project status |
| **Marketing Language** | Translate all claims to specific technical capabilities |
| **Stale Metrics** | Fetch fresh data; include retrieval dates for all metrics |
| **Training Data Override** | When search contradicts training, trust current documentation |
| **Surface-Level Analysis** | Analyze complete architecture, not just API surface |
| **Overconfidence** | Tag claims as [VERIFIED] or [CLAIMED]; acknowledge uncertainty |
| **Governance Blindness** | Explicitly research project governance, not just features |

---

## Phase 0: Evaluation Context
**Human input required — frames entire analysis**

Ask the user these questions sequentially (not all at once):

### Question 1: Subject
"What specific project, tool, or technology do you want to evaluate? (Please provide the name and, if helpful, a URL or brief description)"

### Question 2: Purpose
"What's the purpose of this evaluation?
- **Adoption decision** — considering using this for a specific purpose
- **Investment tracking** — monitoring something you or your org has invested in
- **Competitive intelligence** — understanding a competitor's tooling or a competing project
- **Dependency audit** — assessing risk of an existing dependency
- **General research** — exploring the landscape"

### Question 3: Current Relationship
"What's your current relationship with this project?
- Never used it
- Evaluated previously (when?)
- Currently using in limited capacity
- Currently using in production
- Considering migrating away from it"

### Question 4: Success Criteria
"What would you need to learn from this analysis to consider it useful? What questions are you trying to answer?"

### Question 5: Constraints (if adoption-related)
If purpose is adoption or dependency audit, ask:
"Are there any hard constraints I should know about? (e.g., licensing requirements, deployment model, integration needs, team expertise)"

**Store this context** — reference it throughout to ensure relevance.

**Checkpoint:** "Here's my understanding of what you need: [summary]. Correct before I proceed?"

---

## Phase 1: Project Discovery
**Web search to gather current project state**

### Search Tasks

Use web search to find current information:

1. **Official sources:**
   - Search: `[project name] official documentation`
   - Search: `[project name] github` (if open source)
   - Search: `[project name] release notes [current year]`

2. **Gather baseline facts** (tag each with source and date):
   - Project description (in technical terms, not marketing)
   - Governance model (foundation-backed, single-vendor, community-led)
   - License type and any license changes history
   - Primary use cases
   - Notable adopters (verify, don't assume)
   - Current version, release frequency

3. **Gather quantitative metrics** (note retrieval dates):
   - GitHub stars, forks, open issues, contributors (if applicable)
   - Package downloads (npm, PyPI, etc. if applicable)
   - Last commit date, release cadence

### Critical: Avoid These Errors
- Do NOT use training data for metrics — always search
- Do NOT adopt marketing language — translate to technical specifics
- Do NOT assume governance model — verify from project docs

### Human Validation Checkpoint
Present findings and ask:
"Here's what I found about [Project]. Is this accurate? Is there anything I've misunderstood or that you know to be different?"

**Output:** Validated project profile

---

## Phase 2: SWOT Gathering (FORKED SUBAGENTS)
**Critical: Use forked contexts to prevent bias contamination**

### Why Forked Isolation Matters
Analyzing strengths first creates anchoring that colors how weaknesses are perceived (and vice versa). To prevent this, run two separate analyses in forked contexts that have no knowledge of each other's findings.

### Positive Analysis (Strengths + Opportunities)

Fork a subagent with these instructions:

> **Task:** Conduct ONLY the POSITIVE aspects analysis for [Project].
> Focus exclusively on Strengths and Opportunities. Do NOT analyze weaknesses or threats.
>
> **Use web search to research:**
> 1. Technical capabilities and architecture advantages
> 2. Community health and ecosystem support  
> 3. Documentation quality and developer experience
> 4. Roadmap and future direction
> 5. Growing adoption or momentum
> 6. Standards alignment or specification compliance
>
> **For each finding:**
> - Tag as [VERIFIED-{date}] if confirmed via official docs/code, or [CLAIMED] if from unofficial sources
> - Include source URL
> - Note relevance to: [evaluation context from Phase 0]
>
> **Search queries to run:**
> - "[project] features capabilities"
> - "[project] advantages benefits"
> - "[project] roadmap [current year]"
> - "[project] adoption case studies"
> - "[project] performance benchmarks"
>
> Return a structured list of strengths and opportunities.

Save the output.

### Negative Analysis (Weaknesses + Threats)

Fork a **separate** subagent with these instructions:

> **Task:** Conduct ONLY the NEGATIVE aspects analysis for [Project].
> Focus exclusively on Weaknesses and Threats. Do NOT analyze strengths or opportunities.
>
> **Use web search to research:**
> 1. Technical limitations and architectural constraints
> 2. Missing features or capability gaps
> 3. Governance or sustainability risks
> 4. Competitive pressure from alternatives
> 5. Dependency risks or security concerns
> 6. Community complaints and common pain points
>
> **For each weakness, tag as:**
> - [FUNDAMENTAL] — architectural/design limitation unlikely to change
> - [CURRENT-STATE] — gap that may be addressed in future
>
> **For each finding:**
> - Tag as [VERIFIED-{date}] or [CLAIMED]
> - Include source URL
> - Note relevance to: [evaluation context from Phase 0]
>
> **Search queries to run:**
> - "[project] issues problems limitations"
> - "[project] vs alternatives comparison"
> - "[project] github issues" (look for recurring complaints)
> - "[project] criticism concerns"
> - "[project] security vulnerabilities"
> - "[project] maintainer mass layoff OR funding" (governance risks)
>
> Return a structured list of weaknesses and threats.

Save the output.

### Merge Results

After both forked analyses complete, combine the results into a unified SWOT.

### Human Verification Checkpoint
Present the combined SWOT and ask:
"Here are the findings. Before I analyze implications, please verify:
1. Do the strengths match your understanding?
2. Are there weaknesses I've missed that you're aware of?
3. Any findings that seem inaccurate?"

---

## Phase 3: Context Integration
**Human provides organizational factors AI cannot know**

### Questions to Ask

1. "How do the identified **strengths** align with your specific needs? Which matter most for your situation?"

2. "Which **weaknesses** are dealbreakers vs. acceptable trade-offs for your use case?"

3. "Are there **organizational factors** that affect this evaluation?
   - Team expertise with similar technologies
   - Existing infrastructure or tooling
   - Risk tolerance
   - Timeline pressures
   - Budget constraints"

4. "Is there anything in the **opportunities** or **threats** that's particularly relevant given your roadmap or strategy?"

### Weight Assignment (Human Required)
Ask user to rate importance for their context:
- 🔴 Critical — must be strong here
- 🟡 Important — weighs in decision
- ⚪ Nice-to-have — won't drive decision

Apply these weights to prioritize findings.

---

## Phase 4: Implications Analysis
**Synthesize findings with human validation**

### Generate Assessment

Based on weighted SWOT, analyze:

1. **Adoption Readiness** (if applicable):
   - Is this production-ready for the user's use case?
   - What gaps would need to be addressed?
   - Estimated effort to adopt
     - **Note**: All effort estimates should assume AI-assisted code development and design tools are available
     - Consider reduced implementation time but similar learning/integration overhead

2. **Risk Profile**:
   - Top 3 risks of adopting/depending on this project
   - Top 3 risks of NOT adopting (opportunity cost)
   - Likelihood and impact for each

3. **Trajectory Assessment**:
   - Is this project gaining or losing momentum?
   - What's the governance/sustainability outlook?
   - Are there signals of concerning direction?

4. **Alternative Consideration**:
   - Without doing full comparison, note: "Based on the weaknesses identified, alternatives worth considering might include: [brief list]"
   - Suggest the swot-analysis skill if user wants to compare options in depth

### Human Validation Checkpoint
"Here's my assessment of implications. Does this align with your intuition? What am I missing about your situation?"

---

## Phase 5: Final Output
**Generate comprehensive report and presentation**

### Step 1: Create Detailed Markdown Report

Create file `[project_name]_swot_report.md`:

```markdown
# Project SWOT Analysis: [Project Name]

**⚠️ DISCLAIMER: This report is AI generated with minimal human input and validation.**

**Date:** [Current date]
**Purpose:** [From Phase 0]
**Analyst Context:** [User's relationship and constraints]

---

## Executive Summary

[2-3 paragraph synthesis: what is this project, what's its current state, and what's the bottom line for the user's purpose]

**Bottom Line:** [One sentence recommendation/assessment]

---

## Project Profile

| Attribute | Value | Source | Date Verified |
|-----------|-------|--------|---------------|
| Current Version | | | |
| License | | | |
| Governance | | | |
| GitHub Stars | | | |
| Last Release | | | |
| Primary Use Case | | | |

---

## SWOT Analysis

### Strengths
[Prioritized by user's weights, with verification tags and sources]

### Weaknesses  
[Prioritized by user's weights, tagged as FUNDAMENTAL or CURRENT-STATE]

### Opportunities
[With timeframes where known]

### Threats
[With likelihood assessment]

---

## Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk 1] | H/M/L | H/M/L | [Approach] |
| [Risk 2] | H/M/L | H/M/L | [Approach] |
| [Risk 3] | H/M/L | H/M/L | [Approach] |

---

## Trajectory Assessment

[Is this project ascending, stable, or declining? Evidence for assessment]

---

## Recommendations

Based on your stated purpose of [purpose] and constraints of [constraints]:

1. **[Primary recommendation]**
2. **[Secondary recommendation]**
3. **[If applicable: what to monitor going forward]**

### When This Assessment Would Change
- [Condition 1 that would alter the recommendation]
- [Condition 2]

---

## Sources Consulted

[List all URLs with access dates]

---

## Methodology Notes

- Positive and negative analysis conducted in separate forked contexts to prevent bias
- All metrics fetched fresh on [date]
- User-provided context integrated in Phase 3
```

### Step 2: Convert Markdown to PDF

After creating the markdown report, convert it to PDF:

```bash
pandoc [project_name]_swot_report.md -o [project_name]_swot_report.pdf --pdf-engine=tectonic -V geometry:margin=1in
```

### Step 3: Create Executive Presentation

Generate a professional PowerPoint presentation with 5 slides covering the four SWOT categories.

Use corporate branding with professional colors:
- Primary accent color for headers (e.g., RGB: 238, 0, 0 or similar corporate red)
- Black for body text
- White for header text on colored backgrounds
- Clean, modern design

**Slide 1: Title Slide**
- Project Name SWOT Analysis
- Subtitle with evaluation purpose and date
- **DISCLAIMER text**: "⚠️ This report is AI generated with minimal human input and validation."

**Slide 2: Strengths** (10 key strengths)
**Slide 3: Weaknesses** (10 key weaknesses)
**Slide 4: Opportunities** (10 key opportunities)
**Slide 5: Threats** (10 key threats)

Create as `[project_name]_swot_overview.pptx`

### Step 4: Convert Presentation to PDF

Convert the PowerPoint to PDF:

```bash
soffice --headless --convert-to pdf --outdir [output_directory] [project_name]_swot_overview.pptx
```

### Step 5: Verify All Files Created

Confirm the following files exist:
1. `[project_name]_swot_report.md` - Detailed markdown report
2. `[project_name]_swot_report.pdf` - Detailed PDF report
3. `[project_name]_swot_overview.pptx` - Executive presentation
4. `[project_name]_swot_overview.pdf` - Presentation PDF

Present a summary to the user listing all generated files with their sizes.

---

## Bias Safeguards Summary

| Safeguard | Implementation |
|-----------|----------------|
| Positive/negative isolation | Separate forked subagents for S+O vs W+T |
| Fresh data requirement | Web search for all metrics with dates |
| Marketing translation | Convert all claims to technical specifics |
| Verification tagging | Every finding tagged [VERIFIED] or [CLAIMED] |
| Human context anchoring | Phases 0 and 3 require human input |
| Explicit uncertainty | Acknowledge gaps in knowledge |
| Source documentation | URLs and dates for all significant claims |

---

## Escalation Triggers

Suggest deeper analysis or the comparison skill when:
- User realizes they need to compare alternatives
- Project evaluation reveals need for architectural deep-dive
- Multiple stakeholders need audit-ready documentation
- Regulatory or compliance factors require formal assessment
