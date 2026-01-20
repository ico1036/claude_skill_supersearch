# Super Search Execution Guide

Complete step-by-step implementation guide for deep research tasks.

## Table of Contents
1. [Phase 1: Scope Definition](#phase-1-scope-definition)
2. [Phase 2: Parallel Search Execution](#phase-2-parallel-search-execution)
3. [Phase 3: Source Validation](#phase-3-source-validation)
4. [Phase 3.5: Noise Filtering](#phase-35-noise-filtering)
5. [Phase 4: Synthesis](#phase-4-synthesis)
6. [Phase 5: Output Formatting](#phase-5-output-formatting)
7. [Domain-Specific Strategies](#domain-specific-strategies)
8. [Output Templates](#output-templates)

---

## Phase 1: Scope Definition

### 1.1 Parse Research Request

Extract these elements from user request:
- **Primary question**: Core question to answer
- **Sub-questions**: Related questions that support the main answer
- **Context**: Why user needs this information
- **Time sensitivity**: How recent does information need to be?
- **Depth required**: Quick overview vs. comprehensive analysis

### 1.2 Define Research Parameters

```
Topic: [main subject]
Domain: [tech/business/academic/market/general]
Time Threshold: [only information after YYYY-MM]
Exclusions: [what to skip]
Success Criteria: [what constitutes complete answer]
```

### 1.3 Set Exclusion Criteria

Standard exclusions (apply to all research):
- Content farms (low-quality aggregators)
- Paywalled content without summary
- Information older than relevance threshold
- Sources with undisclosed commercial bias

Topic-specific exclusions (set per research):
- Competitor-sponsored content (for product research)
- Non-peer-reviewed claims (for scientific research)
- Anonymous sources (for factual claims)

---

## Phase 2: Parallel Search Execution

### 2.1 Design Search Matrix

For any topic, design 6 parallel searches:

| Search Type | Query Pattern | Purpose |
|-------------|---------------|---------|
| Direct | "[topic]" | Core information |
| Terminology | "[synonym] OR [related term]" | Broaden coverage |
| Critical | "[topic] problems OR limitations OR criticism" | Balance perspective |
| Expert | site:[authority-domain] "[topic]" | High-quality sources |
| Recent | "[topic]" + time filter (past year) | Current developments |
| Comparative | "[topic] vs [alternative]" | Context and options |

### 2.2 Execute Searches in Parallel

Launch all 6 searches simultaneously:

```
# Example: Researching "Kubernetes security best practices"

Search 1: "Kubernetes security best practices"
Search 2: "K8s security" OR "container orchestration security"
Search 3: "Kubernetes security vulnerabilities" OR "K8s security issues"
Search 4: site:kubernetes.io security best practices
Search 5: "Kubernetes security 2024 2025" (recent)
Search 6: "Kubernetes vs Docker Swarm security" OR "Kubernetes security alternatives"
```

### 2.3 Additional Search Patterns by Domain

**Technology Research:**
```
site:github.com [project] issues security
site:stackoverflow.com [technology] best practices
site:arxiv.org [technology] paper
[technology] benchmark comparison
```

**Market Research:**
```
site:crunchbase.com [company/industry]
[market] market size TAM SAM
[industry] trends forecast
[company] funding valuation
```

**Competitive Research:**
```
[company] vs [competitor]
[product] alternatives
[company] reviews complaints
[product] pricing comparison
```

**Academic Research:**
```
site:scholar.google.com [topic]
[topic] systematic review meta-analysis
[topic] research paper PDF
[author name] [topic]
```

---

## Phase 3: Source Validation

### 3.1 Apply 5-Point Validation

For each source, complete this checklist:

| Check | Pass Criteria | Fail Action |
|-------|--------------|-------------|
| **Currency** | Within time threshold | Find newer source |
| **Relevance** | Directly addresses question | Skip or note as tangential |
| **Authority** | Known expert/institution | Verify credentials or skip |
| **Corroboration** | 2+ independent sources agree | Mark as unverified |
| **Accessibility** | User can access content | Note paywall status |

### 3.2 Credibility Scoring

Rate each source:
- **5/5**: Primary source, official documentation, peer-reviewed
- **4/5**: Established expert, reputable publication
- **3/5**: Quality blog post, industry analyst
- **2/5**: Forum discussion, user review
- **1/5**: Anonymous source, unverified claim

Only include findings from 3/5 or higher sources in main findings.
Note 2/5 and below as "community sentiment" or "unverified claims."

---

## Phase 3.5: Noise Filtering

### 3.5.1 Automatic Exclusion Triggers

Exclude sources containing these patterns:

**SEO/Content Farm Indicators:**
- Generic "Top 10" lists without depth
- Excessive keyword repetition
- Thin content with heavy ads
- No author attribution
- Recycled content across domains

**Bias Indicators:**
- Undisclosed affiliate relationships
- Competitor hit pieces
- Press releases disguised as articles
- Paid promotional content

**Quality Red Flags:**
- Factual errors in verifiable claims
- Outdated information presented as current
- Missing citations for statistics
- Sensationalist headlines

### 3.5.2 Replacement Search Strategy

When excluding a popular but low-quality source:
```
# Original query found content farm result
"[topic] best practices"

# Replacement query to find quality alternative
site:[authoritative-domain] "[topic]"
"[topic]" site:edu OR site:gov
"[topic]" research paper
```

---

## Phase 4: Synthesis

### 4.1 Organize by Theme

Group findings into logical categories:
1. Core facts (undisputed information)
2. Best practices (recommended approaches)
3. Risks/limitations (potential issues)
4. Alternatives (other options)
5. Future outlook (trends, predictions)

### 4.2 Identify Consensus vs. Conflict

For each finding, note:
- **Consensus**: All sources agree
- **Majority view**: Most sources agree, some dissent
- **Divided**: Sources split roughly evenly
- **Minority view**: Few sources support, note why included

### 4.3 Gap Analysis

Document what couldn't be determined:
- Questions without authoritative answers
- Areas needing primary research
- Information behind paywalls
- Topics requiring expert consultation

---

## Phase 5: Output Formatting

### 5.1 Structure Research Output

```markdown
# [Research Topic] - Deep Research Report

## Executive Summary
- [Key finding 1]
- [Key finding 2]
- [Key finding 3]
- [Key finding 4]
- [Key finding 5]

## Methodology
- Search queries executed: [count]
- Sources evaluated: [count]
- Sources included: [count]
- Time range: [date range]

## Detailed Findings

### [Theme 1]
[Findings with confidence indicators]

**Confidence: High/Medium/Low**
**Sources: [list]**

### [Theme 2]
...

## Alternatives & Options
| Option | Best For | Limitations |
|--------|----------|-------------|
| A | [scenario] | [limitation] |
| B | [scenario] | [limitation] |
| C | [scenario] | [limitation] |

## Sources
### High Credibility (5/5)
- [Source 1](URL) - [brief description]

### Good Credibility (4/5)
- [Source 2](URL) - [brief description]

### Moderate Credibility (3/5)
- [Source 3](URL) - [brief description]

## Limitations & Gaps
- [What couldn't be determined]
- [Areas needing further research]

## Recommended Follow-up
- [Suggested next steps]
```

### 5.2 Confidence Indicator Guide

Apply these labels to findings:

| Label | Meaning | Visual |
|-------|---------|--------|
| **High** | Multiple authoritative sources | [High Confidence] |
| **Medium** | Single authority or multiple secondary | [Medium Confidence] |
| **Low** | Limited sources | [Low Confidence] |
| **Unverified** | Single source, needs corroboration | [Unverified] |

---

## Domain-Specific Strategies

### Technology Evaluation

**Additional searches:**
```
[technology] GitHub stars contributors
[technology] production use case
[technology] migration from [alternative]
[technology] security audit CVE
```

**Key validation:**
- Check last commit date (active development?)
- Verify company/maintainer backing
- Review issue resolution rate

### Market Analysis

**Additional searches:**
```
[market] total addressable market TAM
[market] growth rate CAGR
[market] key players market share
[market] regulatory environment
```

**Key validation:**
- Cross-reference market size across reports
- Check report publication date
- Verify analyst credentials

### Competitive Intelligence

**Additional searches:**
```
[competitor] LinkedIn employee count
[competitor] job postings
[competitor] recent news funding
[competitor] customer reviews G2 Capterra
```

**Key validation:**
- Separate facts from speculation
- Note source of competitive claims
- Verify with multiple data points

### Academic/Scientific Research

**Additional searches:**
```
[topic] systematic review
[topic] meta-analysis
[topic] replication study
[author] h-index citations
```

**Key validation:**
- Check journal impact factor
- Verify peer review status
- Look for replication/confirmation

---

## Output Templates

### Quick Research (15-minute)

```markdown
# [Topic] - Quick Research Summary

**Key Findings:**
1. [Finding 1] [Confidence]
2. [Finding 2] [Confidence]
3. [Finding 3] [Confidence]

**Top Sources:**
- [Source 1](URL)
- [Source 2](URL)

**Gaps:** [What needs deeper research]
```

### Standard Research (30-60 minutes)

Use full structure from Phase 5.1.

### Comprehensive Research (2+ hours)

Full structure plus:
- Historical context section
- Stakeholder analysis
- Risk assessment matrix
- Decision framework
- Appendix with raw data

---

## Quality Checklist

Before delivering research output, verify:

- [ ] All key questions answered or gaps noted
- [ ] Every finding has confidence indicator
- [ ] All sources accessible to user
- [ ] No single-source claims marked as high confidence
- [ ] Alternatives provided for major decisions
- [ ] Limitations clearly stated
- [ ] Follow-up actions suggested
