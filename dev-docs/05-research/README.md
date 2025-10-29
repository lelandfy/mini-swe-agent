# Research & Investigation

This directory contains research spikes, proof-of-concepts, benchmark results, and exploratory investigations.

## Purpose

Research docs help us:
- **Explore** new technologies before committing
- **Document** findings from investigations
- **Share** knowledge from experiments
- **Inform** future decisions with data

## When to Create Research Docs

Create research docs when:
- Evaluating new technologies or approaches
- Running performance benchmarks
- Investigating bugs or issues
- Prototyping potential features
- Analyzing competitors or alternatives

## Format

Research docs should include:
- **Goal:** What question are you trying to answer?
- **Context:** Why is this important?
- **Approach:** How did you investigate?
- **Findings:** What did you learn?
- **Recommendations:** What should we do?
- **References:** Links to code, data, external resources

## Naming Convention

```
descriptive-name-of-research.md
```

Examples:
- `langchain-integration-spike.md`
- `gpt4-vs-claude-benchmark.md`
- `cost-optimization-analysis.md`

## Types of Research

### Technology Spikes
Exploring new libraries, frameworks, or tools.

**Template:**
```markdown
# [Technology] Investigation

## Goal
What are we evaluating?

## Alternatives
What else could we use?

## Evaluation Criteria
- Performance
- Cost
- Ease of integration
- Maintenance burden

## Findings
What did we learn?

## Recommendation
Should we adopt? Why or why not?
```

### Benchmarks
Performance or cost measurements.

**Include:**
- Test methodology
- Raw data (or link to data)
- Charts/graphs
- Statistical analysis
- Conclusions

### Bug Investigations
Deep dives into complex issues.

**Include:**
- Reproduction steps
- Root cause analysis
- Potential fixes
- Workarounds

## Tips

- **Be thorough:** Future you will thank you
- **Show your work:** Include commands, code, data
- **Date your research:** Technology changes quickly
- **Link to outcomes:** If this led to a PRD or ADR, link to it
