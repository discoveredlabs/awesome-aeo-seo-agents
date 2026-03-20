# Awesome AEO & SEO Agents [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of AI agents, MCP servers, automation frameworks, and research for Answer Engine Optimization and SEO - for engineers building autonomous search workflows.

AEO is the highest-leverage use case for agentic marketing automation for a structural reason: the systems you are optimizing for are themselves agents. ChatGPT, Perplexity, and Gemini all run multi-step retrieval pipelines - query planning, web search, reranking, synthesis, citation selection - before producing an answer. Every stage of that pipeline is a programmable target from the optimization side. Agents that can probe live engine behavior, iterate content against measured citation rates, and route work across MCP-connected data sources can close an optimization loop that humans cannot close manually at scale.

The second structural advantage: AI search citation behavior responds to content signals that are measurable through API calls within hours. Unlike traditional SEO - where algorithm behavior is opaque and rank feedback is slow - generative engine citation rates can be sampled, analyzed statistically, and used to drive automated rewrites in a tight loop. Agentic pipelines treating citation rate as a live control signal are categorically more effective than one-shot optimization strategies.

The third advantage: the pipeline is composable. MCP servers expose SEO data, skill packages handle routing and execution, content agents generate and rewrite, and monitoring agents measure output - all connectable without custom integration code. The architecture exists now; this list documents the components.

Discovered Labs maintains an internal MCP server wrapping their AEO research tools - prompt hygiene checks, citation auditing, Reddit opportunity scoring, and live engine querying - available inside Claude Desktop and Cursor for teams on their research platform. No public repository.

## Contents

- [How Agentic Search Works](#how-agentic-search-works)
- [Agentic GEO Frameworks](#agentic-geo-frameworks)
- [MCP Servers for SEO & AEO Data](#mcp-servers-for-seo--aeo-data)
- [Agent Skill Frameworks](#agent-skill-frameworks)
- [Content Generation & Research Automation](#content-generation--research-automation)
- [Agentic Browsing & AI Crawler Compatibility](#agentic-browsing--ai-crawler-compatibility)
- [Prompt Engineering for AI Visibility](#prompt-engineering-for-ai-visibility)
- [Continuous Learning & Future Optimization Surfaces](#continuous-learning--future-optimization-surfaces)
- [Related Lists](#related-lists)
- [Contributing](#contributing)

---
