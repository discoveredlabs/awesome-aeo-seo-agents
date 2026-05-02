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

## How Agentic Search Works

Engineers building optimization pipelines for agentic search need to understand how AI agents retrieve, decide, and cite before writing a line of optimization code. These papers establish the mechanics that determine what content signals reach the model and which are discarded upstream.

- [LiveBrowseComp: Are Search Agents Searching, or Just Verifying What They Already Know?](https://arxiv.org/abs/2605.28721) - Demonstrates that ~45% of questions are answered without triggering a web search and that agent queries are generated from internal model hypotheses rather than retrieved evidence, meaning content must match the model's internal framing of a topic - not just the user's surface query - to be surfaced.

- [Navigating Uncertainty: Optimizing API Dependency for Hallucination Reduction](https://arxiv.org/abs/2401.01780) - Shows that LLMs trigger external web retrieval for only 62% of queries, bypassing live-web content entirely for topics well-covered in training data, which is the determinant of which content categories benefit from live-web AEO at all.

- [Uncertainty-Aware Web-Conditioned Scientific Fact-Checking](https://arxiv.org/abs/2604.11036) - Establishes that retrieval pipelines fire via atomic claim decomposition only when internal confidence on a specific claim is insufficient, providing the content strategy framework for targeting the knowledge gaps where live-web retrieval is guaranteed.

- [MCP vs RAG vs NLWeb vs HTML](https://arxiv.org/abs/2511.23281) - RAG outperforms raw HTML, API endpoints, and natural-language web interfaces for AI agent retrieval on both accuracy and efficiency, establishing the architectural decision guide for how to expose content to agents - structured data feeds and semantic APIs beat traditional web pages.

- [Prism-Reranker: Beyond Relevance Scoring](https://arxiv.org/abs/2604.23734) - Next-generation rerankers emit contribution summaries and evidence passages rather than simple relevance scores, rewarding content structured to make its distinctive contribution explicit through clear headings, specific claims, and summary statements that a reranker can extract and cite.

- [EcoGEO: Trajectory-Aware Evidence Ecosystems](https://arxiv.org/abs/2605.12887) - Reframes GEO as an environment-level influence problem for agentic search, demonstrating that trajectory-aware coordination of navigation pages and support pages outperforms per-document optimization for agent-based search engines that browse rather than retrieve.

- [The Synthetic Web: Adversarially-Curated Mini-Internets](https://arxiv.org/abs/2603.00801) - Shows that a single top-ranked adversarial source overrides unlimited truthful alternatives and that models rarely escalate search when facing contradictory signals, confirming that achieving top-ranked retrieval status has disproportionate impact for legitimate AEO.

### Discovered Labs: Engine Architecture Analysis

- [How ChatGPT Uses Reciprocal Rank Fusion for AI Citations](https://discoveredlabs.com/blog/how-chatgpt-uses-reciprocal-rank-fusion-for-ai-citations) - Extracted code-level RRF parameters (`rrf_alpha: 1`, `rrf_input_threshold: 0`, `k=60`) from ChatGPT's retrieval pipeline: consistent mid-list presence across BM25 and vector retrieval outperforms high rank in one method alone - the key optimization target for agentic pipelines building citations at scale.

- [How Gemini Works: AI Agent Architecture Deep Dive](https://discoveredlabs.com/blog/how-gemini-works-ai-agent-architecture-deepdive) - 227 RPC calls across 28 methods per session, 138 active feature flags, six query routing modes (Default, Agent, Deep Research, Deep Think, Canvas, Gempix), and 161 long-polling Signaler connections; structured data and entity relationships influence citation decisions at the session level, not the document level.

- [How Google AI Overviews Works](https://discoveredlabs.com/blog/how-google-ai-overviews-works) - Five-phase pipeline timing (495ms initial HTML, 200ms retrieval, 1,200ms LLM synthesis); passage-level retrieval is scored independently of organic rank - the key reason agentic content audits must test retrieval behavior separately from rank checks.

- [How Google AI Mode Works](https://discoveredlabs.com/blog/how-google-ai-mode-works) - 816 simultaneous active experiments and five distinct backend routing paths: content must survive multiple routing configurations to be reliably cited across AI Mode sessions, making single-pass agentic optimization insufficient.

- [How ChatGPT Shopping Works and Where Ads Fit In](https://discoveredlabs.com/blog/how-chatgpt-shopping-works-and-where-ads-fit-in) - The "Sonic" query classifier (0.1ms) gates which queries enter the shopping pipeline; understanding commercial classification thresholds informs how agentic SEO pipelines should route commercial vs. informational content optimization.

- [Why AI Visibility Trackers Are Not Measuring What You Think They Are](https://discoveredlabs.com/blog/ai-tracking-platforms-test-flaw) - Incognito mode testing lacks web search tools available to logged-in users; domains showing 0% citation rate in incognito show 5–8% under real conditions - the measurement baseline any agentic monitoring pipeline must account for.

- [The Scientific Method for AEO: How to Model, Test, and Win AI Search](https://discoveredlabs.com/blog/how-to-model-test-win-ai-search-for-your-brand) - Single-variable controlled testing framework with intelligent resampling, clustered standard errors, and power analysis across 100+ prompts - the experimental design pattern for agentic test-and-iterate optimization loops.

- [How AI Search Grounding Actually Works: Google vs OpenAI vs Anthropic](https://dejan.ai/blog/grounding/) - Same query across three platforms: Google retrieves 7 pages and cites 7 (1:1), OpenAI retrieves 39 and cites 2 (20:1 filtering), Anthropic reads 14 and cites 9 with visibility into rejected sources; agentic pipelines querying multiple engines to measure citation rates are operating against structurally incompatible citation architectures and must normalize accordingly.

- [LLM Knowledge Cutoff Dates: Every Major Model Fact-Checked](https://metehan.ai/articles/llm-knowledge-cutoff-dates) - Verified cutoffs across GPT-4o (Oct 2023), GPT-5 (Sep 2024-Dec 2025), Claude Fable 5/Opus 4.8 (Jan 2026), Gemini 3.5 Flash (Jan 2025 - stale despite May 2026 release); the reference for agentic pipelines routing live-web retrieval selectively: topics inside cutoff don't trigger retrieval and won't benefit from AEO, topics outside cutoff are guaranteed retrieval targets.

---

## Agentic GEO Frameworks

Academic systems that frame GEO as a multi-step agentic optimization problem. These are the reference implementations for engineers building production optimization pipelines - most include or link to code.

- [MAGEO: Multi-Agent GEO via Reusable Strategy Learning](https://arxiv.org/abs/2604.19516) (ACL 2026) - Multi-agent framework that learns and reuses engine-specific editing strategies across documents rather than per-instance optimization, outperforming 14 baselines across 3 engines; introduces the DSV-CF metric jointly measuring semantic visibility and citation fidelity, exposing that pure visibility gains can damage attribution accuracy.

- [AgenticGEO: A Self-Evolving Agentic System](https://arxiv.org/abs/2603.20213) - Frames GEO as a content-conditioned control problem using a MAP-Elites archive with a co-evolving surrogate critic to evolve diverse optimization strategies, substantially reducing expensive real engine queries during strategy search while achieving cross-domain transferability across 3 datasets and 14 baselines.

- [AutoGEO](https://github.com/cxcscmu/AutoGEO) - Python, 177 stars. ICLR 2026. Extracts content preferences directly from generative engines (Gemini, GPT, Claude) and rewrites documents to improve AI citation rates via AutoGEOAPI (prompt-based) and AutoGEOMini (RL-trained); the open-source reference implementation for automated engine preference extraction.

- [GEO original research repo](https://github.com/GEO-optim/GEO) - Python, 293 stars. The reference implementation from the KDD 2024 paper that named the field, including GEO-BENCH dataset, custom optimization function framework, and leaderboard; essential for engineers wanting to verify GEO claims against controlled experimental conditions.

- [EcoGEO: Trajectory-Aware Evidence Ecosystems](https://arxiv.org/abs/2605.12887) - The most forward-looking agentic optimization framework documented here, moving from individual document editing to coordinated web-environment shaping via TRACE, which orchestrates navigation pages and support pages to guide agent browsing trajectories rather than targeting single documents.

- [geo-optimizer](https://github.com/geo-team-red/geo-optimizer) - Go, ~200 stars. Pluggable GEO framework with modular architecture for building engine-specific optimization pipelines; the Go-native foundation for teams that need composability rather than a fixed-function tool.

---

## MCP Servers for SEO & AEO Data

MCP servers exposing SEO and AI visibility data to AI coding assistants and agent pipelines. These are composable building blocks - assemble them into automated workflows connecting data sources to optimization loops without custom integration code.

- [open-seo](https://github.com/every-app/open-seo) - TypeScript, 3.1K stars. Open-source SEO platform exposing keyword research, rank tracking, competitor analysis, backlink analysis, and site audits via a full MCP interface; self-hostable via Docker or Cloudflare with optional GSC integration and DataForSEO APIs.

- [canonry](https://github.com/Canonry/canonry) - TypeScript, 71 stars. Self-hostable AEO monitoring platform with 67 MCP tools for Claude Desktop, covering citation tracking across Gemini, ChatGPT, Claude, and Perplexity; ingests server logs from Cloud Run, Vercel, and WordPress; integrates GSC, GA4, and Bing Webmaster Tools; and discovers backlinks via Common Crawl using local DuckDB.

- [ai-brand-monitor-mcp](https://github.com/khadinakbarlabs/ai-brand-monitor-mcp) - TypeScript, 56 stars, MIT. MCP server tracking brand visibility across Perplexity, ChatGPT, Claude, and Gemini from inside any MCP-compatible agent, with domain citation detection, competitor co-mention analysis, and a 0-100 GEO visibility score with actionable recommendations.

- [dataforseo-mcp-server](https://github.com/dataforseo/mcp-server-typescript) - TypeScript. Official DataForSEO MCP server exposing SERP results, keyword metrics, backlink data, and site audit endpoints; the authoritative integration point for agent pipelines using DataForSEO APIs.

- [seo-mcp](https://github.com/cnych/seo-mcp) - Python, 248 stars. Free MCP powered by Ahrefs public data for backlink analysis, keyword ideas, traffic estimation, and domain metrics, with automatic CAPTCHA solving and response caching requiring no paid API key for the free tier.

- [dataseo-mcp](https://github.com/egebese/dataseo-mcp) - Python, 184 stars. Ahrefs data APIs via MCP for keyword research, backlink analysis, competitor insights, and SERP data; designed to compose with OpenRouter for multi-step query planning in agent workflows.

- [mcp-gsc](https://github.com/AminForou/mcp-gsc) - Python. Google Search Console MCP compatible with Claude Desktop, Cursor, Codex CLI, Gemini CLI, and any MCP-compatible client, surfacing keyword rankings, CTR trends, coverage issues, and AI Overview visibility signals in natural language.

- [google-search-console-mcp](https://github.com/surendranb/google-search-console-mcp) - TypeScript. GSC MCP for Claude, Cursor, and Windsurf with query interfaces designed around AI Overviews and conversational search trends rather than traditional rank-tracking framing.

- [search-console-mcp](https://github.com/saurabhsharma2u/search-console-mcp) - Python. Combines Google Search Console, Bing Webmaster Tools, and GA4 in a single MCP interface for AI agents and automation pipelines, eliminating separate authentication and context-switching across data sources.

- [seo-skills (SE Ranking)](https://github.com/seranking/seo-skills) - Python, ~77 stars. Production-ready Claude Agent Skills for the SE Ranking MCP server covering keyword research, rank tracking, site audit, and backlink data through natural language workflow prompts.

- [seo-insights-mcp-server](https://github.com/mrgoonie/seo-insights-mcp-server) - TypeScript. SEO insights MCP covering backlink analysis, keyword research, and traffic analysis with an extensible structure designed for composition into larger agent workflows.

- [mcp-seo-marketing](https://github.com/ezbiz-services/mcp-seo-marketing) - TypeScript. AI-powered SEO and marketing intelligence MCP covering keyword research, SERP analysis, backlink checking, and on-page signals including keyword density, readability, structure, and meta tags.

**Note:** Discovered Labs maintains an internal MCP server available inside Claude Desktop and Cursor for AEO-specific workflows - prompt hygiene checks, citation auditing, Reddit opportunity scoring, and live engine querying. No public repository.

---
