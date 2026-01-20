---
name: super-search
description: Comprehensive deep research skill with systematic validation, source hierarchy, and parallel multi-query search strategy. Use when user needs thorough research on any topic - market analysis, technology evaluation, competitive research, academic investigation, trend analysis, product comparison, or any complex question requiring multiple sources and verified information. Triggers on requests like "research X", "investigate Y", "deep dive on Z", "compare A vs B", "find information about", or any task requiring comprehensive information gathering beyond simple lookups.
---

# Super Search

Systematic deep research framework with 5-point validation, source credibility hierarchy, and parallel search strategy. Transforms scattered web searches into verified, actionable intelligence.

## Core Principles

### 1. Parallel Multi-Query Strategy
Never rely on a single search. Execute 4-6 parallel searches from different angles:
- Primary query (direct topic search)
- Alternative terminology (synonyms, related concepts)
- Negative angle ("problems with X", "X failures", "X criticism")
- Expert sources (site:specific-domain searches)
- Recent developments (time-filtered searches)
- Comparative context ("X vs Y", "alternatives to X")

### 2. Source Credibility Hierarchy
Prioritize sources in this order:
1. **Primary sources**: Official documentation, research papers, company announcements
2. **Verified platforms**: Industry databases, professional publications, peer-reviewed content
3. **Expert analysis**: Established analysts, domain experts with track records
4. **Secondary aggregations**: News articles, well-researched blog posts
5. **Community sources**: Forums, social media (use for sentiment, not facts)

### 3. 5-Point Validation Checklist
Every key finding must pass:

| Check | Question | Action if Failed |
|-------|----------|------------------|
| Currency | Is this information current/recent? | Find more recent source |
| Relevance | Does this directly answer the question? | Refocus search |
| Authority | Is the source credible for this topic? | Find authoritative source |
| Corroboration | Is this confirmed by multiple sources? | Cross-reference |
| Accessibility | Can user verify this information? | Provide accessible links |

### 4. Explicit Exclusion Criteria
Define upfront what to exclude:
- Paywalled content without accessible alternatives
- Outdated information (define threshold per topic)
- Sources with clear bias without disclosure
- Unverifiable claims
- SEO-optimized content farms

### 5. Alternative Options Over Single Answers
Always provide context for recommendations:
- "Option A is best for [scenario X]"
- "Option B is preferable when [condition Y]"
- Include at least 3 alternatives for major decisions

## Execution Workflow

### Phase 1: Scope Definition
1. Parse user request into specific research questions
2. Identify topic domain and time sensitivity
3. Set exclusion criteria for this research
4. Define success criteria (what would be a complete answer?)

### Phase 2: Parallel Search Execution
Execute searches simultaneously across multiple angles:
```
Search 1: "[topic] comprehensive guide"
Search 2: "[topic] [alternative term]"
Search 3: "[topic] problems limitations criticism"
Search 4: site:[authoritative-domain] [topic]
Search 5: "[topic] 2024 2025" (recent)
Search 6: "[topic] vs [alternative]" OR "alternatives to [topic]"
```

### Phase 3: Source Validation
For each potential source:
1. Check publication date
2. Verify author/organization credibility
3. Look for corroborating sources
4. Confirm accessibility

### Phase 4: Synthesis
1. Consolidate findings by theme
2. Note consensus vs. conflicting views
3. Identify gaps in available information
4. Highlight highest-confidence findings

### Phase 5: Output Formatting
Structure results with:
- Executive summary (key findings in 3-5 bullets)
- Detailed findings by subtopic
- Confidence levels for each finding
- Source citations with accessibility notes
- Identified gaps and suggested follow-up

## Search Query Optimization

### Effective Query Patterns
```
"[exact phrase]"           - Force exact match
site:domain.com [topic]    - Domain-specific search
[topic] -[exclude term]    - Exclude unwanted results
[topic] filetype:pdf       - Find documents
[topic] after:2024-01-01   - Time filtering
[topic] OR [synonym]       - Broaden search
```

### Domain-Specific Searches
- **Tech**: site:github.com, site:stackoverflow.com, site:arxiv.org
- **Business**: site:bloomberg.com, site:reuters.com, site:crunchbase.com
- **Academic**: site:scholar.google.com, site:researchgate.net
- **Government**: site:gov, site:edu

## Output Standards

### Required Elements
1. **Summary**: 3-5 bullet executive summary
2. **Methodology**: Brief description of search approach
3. **Findings**: Organized by theme with confidence indicators
4. **Sources**: All sources with accessibility status
5. **Limitations**: What couldn't be determined
6. **Follow-up**: Suggested next steps if needed

### Confidence Indicators
- **High**: Multiple authoritative sources agree
- **Medium**: Single authoritative source or multiple secondary sources
- **Low**: Limited sources or conflicting information
- **Unverified**: Single source, needs corroboration

## References

For detailed execution steps and advanced patterns, see:
- [references/EXECUTE.md](references/EXECUTE.md) - Complete execution guide with examples
