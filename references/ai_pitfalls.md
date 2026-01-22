# AI Pitfalls Reference for Technology Evaluation

Quick reference for common AI analysis errors and how to prevent them.

**Methodology Attribution:** This AI pitfalls framework was developed by [Jessica Forrester](https://github.com/jwforres) and [Jason Greene](https://github.com/n1hility).

## Critical Errors to Avoid

### 1. Outdated Knowledge
**Problem:** AI states incorrect information based on training data instead of current reality.
**Prevention:** Always web search and verify against current official documentation. Never trust training data for:
- Project status, version numbers, release dates
- GitHub metrics (stars, contributors, issues)
- Governance structure or maintainer status
- Feature availability or roadmap items

### 2. Marketing Language Adoption
**Problem:** AI repeats vendor marketing claims without translating to technical specifics.
**Examples of marketing language to avoid:**
- "Enterprise-grade" → What specific features make it enterprise-grade?
- "Highly scalable" → What are the actual scaling characteristics?
- "Best-in-class" → Compared to what, on what metrics?

**Prevention:** For every capability claim, ask: "What specifically does this mean technically?"

### 3. Stale Metrics
**Problem:** AI uses cached or training-data metrics instead of current values.
**Prevention:** 
- Fetch all metrics fresh via web search
- Always include retrieval date with metrics
- Note if metrics seem inconsistent with other signals

### 4. Training Data Override
**Problem:** When web search contradicts training data, AI dismisses search results.
**Prevention:** Explicitly prioritize current documentation, especially for:
- Version-specific information
- Recent organizational changes
- Feature additions or deprecations
- Security vulnerabilities

### 5. Surface-Level Analysis
**Problem:** Analyzing only the API surface without understanding full system behavior.
**Prevention:** Trace complete paths:
- What happens under the hood?
- What dependencies are involved?
- Where are the actual performance characteristics determined?

### 6. False Differentiation
**Problem:** Claiming features are unique when competitors have equivalent functionality.
**Prevention:** For any claimed differentiator:
- Verify competitors don't have equivalent
- Check if feature exists under different name
- Distinguish implementation quality from feature presence

### 7. Governance Blindness
**Problem:** Focusing only on features while ignoring project health and sustainability.
**Prevention:** Always investigate:
- Who maintains this project?
- What's the funding model?
- Has there been leadership turnover?
- Are there signs of declining activity?

### 8. Overconfidence in Claims
**Problem:** Stating uncertain information with high confidence.
**Prevention:** 
- Tag all findings with verification status
- Acknowledge when information is from unofficial sources
- Note when claims conflict across sources

## Verification Tag Requirements

Every significant finding must include:

### For Strengths/Opportunities:
```
[VERIFIED-YYYY-MM-DD] - Confirmed via official docs or code
[CLAIMED] - Found in unofficial sources, needs verification
```

### For Weaknesses:
```
[FUNDAMENTAL] - Architectural limitation, unlikely to change
[CURRENT-STATE] - Gap that may be addressed in future
```

### For All Findings:
- Source URL (required)
- Retrieval/verification date
- Evidence supporting the claim

## Quality Checklist Before Presenting Findings

- [ ] All metrics fetched fresh with dates noted
- [ ] No marketing language without technical translation
- [ ] Source URLs provided for significant claims
- [ ] Verification tags applied to all findings
- [ ] Governance/sustainability explicitly addressed
- [ ] Current documentation prioritized over training knowledge
- [ ] Claimed differentiators verified against alternatives
- [ ] Uncertainty acknowledged where present
