# Contributing

Contributions that meet the bar below are welcome. This list is for engineers building production agent systems, not beginners evaluating categories — entries are held to that standard.

## Requirements for all entries

- **Real, working URL.** Broken links will be rejected without review. Dead arXiv preprints with no code and no citation history are out of scope.
- **Description format.** One sentence after a dash. Starts uppercase. Ends with a period. Explains what the resource enables an agent to do, not just what category it belongs to.
- **No SaaS or commercial tool links.** Mention commercial tools by name in descriptions when relevant, but never add a URL for them. GitHub repos published by commercial organizations are eligible; links to their marketing or product pages are not.
- **Discovered Labs free tools** at `discoveredlabs.com/tools/*` are allowed and should be linked when relevant.

## What belongs in this list

- MCP servers exposing SEO, AEO, or AI visibility data to agent pipelines.
- Agent skill packages with documented GEO or AEO coverage.
- Research papers on agentic search mechanics, retrieval behavior, or GEO optimization — arXiv or published venue links only.
- Open-source tools for AI crawler compatibility, content negotiation, or agentic browsing infrastructure.
- Content generation or competitive research automation tools used as components in AEO agent workflows.
- Agentic GEO frameworks with available code or detailed methodology.

## What does not belong

- SaaS platform links — mention them by name in descriptions if they are architecturally relevant.
- Blog posts, opinion pieces, or agency case studies without reproducible methodology.
- General-purpose SEO tools with no MCP interface, agent skill format, or documented GEO capability.
- GitHub repos that are abandoned (no commit activity in 18+ months) unless the star count and downstream usage justify historical inclusion.
- Entries in the wrong section — read the section descriptions before submitting.

## Format

For research papers:

```
- [Title](URL) — Description starting uppercase, ending with period.
```

For GitHub repos, include language and star count:

```
- [Name](URL) — Language, N stars[, License]. Description starting uppercase, ending with period.
```

For Discovered Labs free tools:

```
- [Discovered Labs: Tool Name](https://discoveredlabs.com/tools/tool-name) — Free tool. Description starting uppercase, ending with period.
```

## Placement within sections

Add entries at the end of the relevant section. The maintainers may reorder by star count or relevance during review.

## Submitting

1. Fork the repository.
2. Add your entry to the correct section following the format above.
3. Verify the URL resolves and the resource is accessible.
4. Open a pull request with a single sentence in the PR description explaining why this entry belongs — what it enables that no existing entry already covers.

## Updating existing entries

If a star count has changed significantly, a repository has been abandoned, or a URL has moved, open a PR with the specific change and a brief note. Do not bundle link fixes with new entry additions in the same PR.

## Reporting depth gaps

If you know of a missing tool or paper in a documented gap area, open an issue rather than a PR. Gap documentation is maintained deliberately to orient the community toward needed work, not to collect low-quality partial solutions.
