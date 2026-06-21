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

## Agent Skill Frameworks

Pre-built skill packages installable into AI coding assistants for SEO and GEO capabilities without building from scratch. Evaluated on sub-skill count, platform compatibility, GEO/AEO depth, and automation surface area.

- [claude-seo](https://github.com/AgriciDaniel/claude-seo) - Python, 9.9K stars, MIT. The most complete SEO agent skill package available: 25+ sub-skills and 18 specialist agents covering technical SEO, E-E-A-T, Schema.org (30+ types), GEO/AEO via `/seo geo`, backlinks, local SEO, semantic clustering, e-commerce, and international SEO; SPA-aware via Playwright; optional Firecrawl and DataForSEO MCP extensions.

- [seo-geo-claude-skills](https://github.com/aaron-he-zhu/seo-geo-claude-skills) - Python, 2.3K stars. Twenty SEO and GEO skills for Claude Code, Cursor, Codex, and 35+ AI agent platforms, providing the broadest platform compatibility in this list for teams requiring cross-platform deployment.

- [codex-seo](https://github.com/AgriciDaniel/codex-seo) - Python, 322 stars. Twenty-six end-to-end workflows and 24 TOML agents designed for autonomous operation in Codex and Claude Code CI environments, the best option for teams running Codex CLI in automated pipelines.

- [Agentic-SEO-Skill](https://github.com/Bhanunamikaze/Agentic-SEO-Skill) - Python. Sixteen specialized sub-skills, 10 specialist agents, and 88 optional utility scripts functioning as evidence collectors, producing structured reports and action plans; compatible with Claude Code, Codex, Cursor, Windsurf, and Cline.

- [recomby-geo](https://github.com/ViryaZheng/recomby-geo) - Python, 522 stars. GEO-specific skills package combined with curated agent configuration lists across multiple platforms, optimized for teams prioritizing GEO depth over broad technical SEO coverage.

- [NotFair](https://github.com/nowork-studio/NotFair) - TypeScript, 3K stars. Claude Code skills covering SEO, GEO, Google Ads, and Meta Ads via agent-native commands, useful for teams wanting cross-channel marketing automation including AI search visibility in a single skill package.

- [web-analytics-agent-skill](https://github.com/SeoToolkit/web-analytics-agent-skill) - Python, 19 stars. Connects GSC, GA4, and Bing Webmaster APIs directly to Claude Code and compatible agents, surfacing keyword performance, crawl errors, click data, and analytics metrics in natural language as an essential data-layer skill for any automated SEO workflow.

---

## Content Generation & Research Automation

Agentic systems automating content creation, competitive research, and gap analysis as components of AEO workflows. The highest-value automation loop: identify a citation gap, generate content targeting it, measure citation rate, iterate.

- [seobuild-onpage](https://github.com/gbessoni/seobuild-onpage) - Python, 218 stars, MIT. Full page generation pipeline that pulls live SERP data, analyzes top competitors, extracts People Also Ask data, identifies content gaps, and writes structured pages with FAQ sections and schema markup; runs a 55-point post-generation quality checklist and explicitly targets AI Overviews and Perplexity citation as output success criteria.

- [Lil'Log: LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/) - Lilian Weng's canonical post on agent architecture covering planning, memory, tool use, and action modules; the conceptual framework for designing agentic AEO pipelines where citation monitoring, content rewriting, and live re-testing are autonomous agent behaviors rather than manual workflows.

- [gpt-researcher](https://github.com/assafelovic/gpt-researcher) - Python, 27,900 stars. Autonomous deep research agent that plans queries, searches, scrapes, and synthesizes cited reports; the standard infrastructure for producing citation-worthy original research assets, which the empirical evidence consistently shows dominates AI citation rates over reformatted commodity content.

- [firecrawl](https://github.com/firecrawl/firecrawl) - TypeScript, 30,000+ stars. Converts any URL to clean LLM-ready Markdown or structured JSON with dynamic page support, batch crawling, and site mapping; the de facto standard infrastructure layer for competitive content intelligence, knowledge base construction, and SERP data extraction in multi-agent SEO pipelines.

- [Discovered Labs: Heading Optimizer](https://discoveredlabs.com/tools/heading-optimizer) - Free tool. Generates 10+ heading variants, ranks them via pairwise tournament comparison using log probabilities with beam search refinement, and outputs a Pareto-optimal ranked list trading AI citation preference against Flesch-Kincaid readability - the agent-native alternative to rule-based heading scoring.

- [Reverse-Engineering the GPT-5 Tokenizer for AEO/GEO](https://metehan.ai/blog/reverse-engineering-the-gpt-5-tokenizer-aeo-geo) - 200,019 token analysis of o200k_base: plain prose achieves 2.2x better retrieval density than markdown tables (5.9 vs. 2.7 chars/token); numerals, common abbreviations, and answer-first structure maximize context-window efficiency; directly informs the output format configuration for agentic content generation pipelines to maximize citation yield per token.

- [CITABLE: The AEO Content Framework](https://discoveredlabs.com/blog/citable-the-aeo-content-framework-we-use-to-get-b2b-brands-cited-by-ai) - Seven-dimension scoring model used by the AEO Content Evaluator: Clear entity and structure, Intent architecture (primary and adjacent query coverage), Third-party validation, Answer grounding (verifiable sourced facts), Block-structured for RAG (200-400 word sections), Latest and consistent, Entity graph and schema (JSON-LD); the practitioner reference for what agentic content generation agents should optimize toward before scoring with the evaluator.

- [Discovered Labs: AEO Content Evaluator](https://discoveredlabs.com/tools/aeo-content-evaluator) - Free tool. Scores pasted content up to 15,000 characters across seven CITABLE dimensions - Semantic Clarity, Citation & Answer Analysis, RAG Optimization, Third-party Validation, Intent Architecture, Freshness & Consistency, and AI Detection - outputting an overall readiness score, strength/weakness breakdown, and prioritized improvement recommendations; the per-document scoring step in agentic content optimization loops where agents draft, score, rewrite, and re-score in sequence.

- [Discovered Labs: LLM Eval Calculator](https://discoveredlabs.com/tools/llm-eval-calculator) - Free tool. Calculates the statistically required sample size for measuring brand mention rates across AI platforms; uniquely accounts for both between-question variance (different prompts yield different mention rates) and within-question variance (LLM stochasticity across runs of the same prompt); outputs unique prompts required, total API calls, and time estimates given a confidence level (90/95/99%), margin of error (0.5-5%), and resamples per prompt K (1-20) - the planning tool for agents running their own AEO measurement studies.

---

## Agentic Browsing & AI Crawler Compatibility

Configuring sites so AI agents can effectively navigate, parse, and use content. This is the upstream requirement: if an agent cannot cleanly extract your content, no optimization downstream compensates for it.

- [WebMCP W3C Community Group Draft](https://github.com/webmachinelearning/webmcp) - Spec authored by Microsoft (Brandon Walderman) and Google (Khushal Sagar, Dominic Farolino) defining `navigator.modelContext.registerTool()` as a browser-native JS API for exposing web app tools to AI agents with zero server infrastructure; tools execute in page context sharing user auth; browser mediates all calls with security architecture covering tool poisoning, output injection, over-parameterization, and cross-origin leakage. Status: CG-DRAFT with core API defined, service worker background tools and declarative HTML form tools in the roadmap. Discovered Labs has conducted a full technical analysis of the spec and its AEO implications.

- [Discovered Labs: Agentic Browsing Checker](https://discoveredlabs.com/tools/agentic-browsing-checker) - Free tool. Runs 19 checks across the OPERABLE framework covering WebMCP tool registration, form annotation coverage, accessibility tree quality, tool schema JSON validity, llms.txt content validation, risky auto-submit detection, ARIA live region coverage, button keyboard operability, and AI crawler robots.txt access; uses Puppeteer, Lighthouse, and WebMCP W3C draft compliance testing to deliver a scored report by email in 45–90 seconds.

- [GPTBot Crawled 60,000 Pages 30,000+ Times in 12 Hours](https://metehan.ai/blog/i-built-a-60-000-page-ai-website-for-10-gptbot-crawled-it-30-000-times-in-12-hours) - Controlled experiment documenting GPTBot's 470x higher crawl volume versus Googlebot on a brand-new domain: 29,000+ requests within 12 hours at ~1 req/sec with zero backlinks or authority signals; confirms AI crawlers operate on an independent discovery and crawl budget entirely disconnected from traditional search signals.

- [Crawling 65,000 Pages via HTTP Headers](https://metehan.ai/blog/http-headers-internal-links) - Encoding link graphs and heading hierarchies as base64url JSON in custom response headers (`X-Internal-Links`, `X-Headings`) delivers 8.6x faster crawl speeds with edge caching and no HTML parsing; found 41.9% of pages were orphaned - unreachable by agent navigation - a structural issue agentic site audits should check before any content-level optimization.

- [Discovered Labs: AI Assist Widget](https://discoveredlabs.com/tools/ai-assist-widget) - Free tool. Single script tag (<5KB minified, zero external dependencies, CSS scoped with unique IDs) that adds a floating bar of buttons opening ChatGPT, Claude, Perplexity, Grok, or Gemini in a new tab with the current page URL pre-filled in the prompt; configurable engine selection, position, colors, labels, and default prompt; no tracking, analytics, or external requests; the lightweight implementation for increasing content exposure to AI agents without modifying page architecture.

- [agentic-seo](https://github.com/addyosmani/agentic-seo) - JavaScript, 244 stars. AEO audit CLI specifically designed for agentic contexts with 10 checks across five categories including a Token Economics category that directly addresses the per-token cost to an agent of parsing your content; scaffolds missing AEO infrastructure files including AGENTS.md, skill.md, and llms.txt.

- [agent-seo](https://github.com/pontiggia/agent-seo) - TypeScript. Middleware that detects 17+ AI crawler user agents including GPTBot and ClaudeBot and serves clean Markdown instead of HTML, with adapters for Next.js, Express, Fastify, and Hono as the server-side content negotiation layer for frameworks wanting to serve optimally structured content to AI agents.

- [dualmark](https://github.com/dodopayments/dualmark) - TypeScript, 91 stars, Apache 2.0 with patent grant. Dual-serves clean Markdown to AI bots and polished HTML to humans from the same URL via HTTP content negotiation, with six framework adapters (Next.js, Astro, SvelteKit, Cloudflare Workers, Deno Deploy, Vercel Edge), CLI conformance scoring, and automatic llms.txt generation.

- [MCP vs RAG vs NLWeb vs HTML](https://arxiv.org/abs/2511.23281) - The architectural decision guide for how to expose content to agents, establishing that RAG-compatible APIs and semantic data feeds outperform plain HTML for AI agent discoverability on both accuracy and efficiency dimensions.

- [EcoGEO: Trajectory-Aware Evidence Ecosystems](https://arxiv.org/abs/2605.12887) - The most advanced concept in this section: trajectory-aware coordination of navigation and support pages to guide agent browsing trajectories, relevant for large content operations thinking about site architecture from an agent navigation perspective rather than per-page optimization.

---

## Prompt Engineering for AI Visibility

How to think about prompts for both sides of the AEO equation: the queries AI engines internally generate (which your content must match) and the prompts driving automated GEO optimization pipelines.

- [DAIR.AI Prompt Engineering Guide](https://www.promptingguide.ai) - Elvis Saravia's comprehensive open guide covering prompting techniques, RAG architectures, chain-of-thought, agentic patterns, and LLM evaluation methodology; the practical engineering reference for designing prompts that drive automated content optimization loops and AI visibility measurement pipelines reliably.

- [LiveBrowseComp: Are Search Agents Searching, or Just Verifying?](https://arxiv.org/abs/2605.28721) - Establishes that optimizing for the language a model would naturally use to query your subject area matters as much as keyword targeting, because ~45% of agent queries are internally generated from model hypotheses rather than derived from retrieved evidence.

- [MAGEO: Multi-Agent GEO via Reusable Strategy Learning](https://arxiv.org/abs/2604.19516) - Engine-specific and query-type-specific prompt strategies are necessary; MAGEO's preference modeling is the research basis for why single-strategy prompt optimization systematically fails across Gemini, ChatGPT, and Perplexity - what lifts citation on one engine can suppress it on another.

- [FeatGEO: Feature-Level Multi-Objective Optimization](https://arxiv.org/abs/2604.19113) - Demonstrates that planning structural, content, and linguistic feature configurations before generating text - rather than editing post hoc - produces stronger citation improvement because document-level properties influence citation behavior more strongly than token-level edits.

- [Iterate Until Retrieved: Factual Nugget Optimization](https://arxiv.org/abs/2605.25641) - Iterative optimization of entry format and phrasing substantially improves discoverability in production RAG pipelines even when semantic content is correct, establishing that the framing of a factual claim determines whether it is retrieved, not just its information content.

- [Discovered Labs: Reddit Threads Finder](https://discoveredlabs.com/tools/reddit-threads-finder) - Free tool. Identifies which Reddit threads are currently cited in ChatGPT and Gemini responses for a given domain with opportunity and relevance scoring; the output directly informs which community language patterns and framing structures to incorporate into owned content to match AI engine citation preferences.

- [Uncertainty-Aware Web-Conditioned Fact-Checking](https://arxiv.org/abs/2604.11036) - Provides the framework for identifying what to write about, not just how to write it: target atomic claims where LLMs have training gaps, because those are where live-web retrieval is guaranteed and citation opportunity is highest.

---

## Continuous Learning & Future Optimization Surfaces

Emerging surfaces and long-horizon opportunities. These are not fully operational optimization channels yet, but engineers designing systems now should account for them.

- [Discovered Labs: AI Visibility Tracker](https://discoveredlabs.com/technology/ai-visibility-tracker) - Managed monitoring platform testing real buyer questions sourced from Google Search Console, support tickets, call transcripts, and subreddits across ChatGPT, Claude, Perplexity, Google AI Overviews, and Gemini every five days; tracks citation rate, mention rate, average ranking position, share of voice, citation source breakdown (Reddit, reviews, YouTube, blogs), brand perception scoring, and competitor benchmarking; statistically rigorous - each prompt runs multiple times with 95% credible intervals and movements only register after three consecutive significant runs; the production monitoring layer for teams running continuous agentic AEO loops.

- [ALAS: Autonomous Learning Agent for Self-Updating Language Models](https://arxiv.org/abs/2508.15805) - Demonstrates an autonomous pipeline continuously fine-tuning LLMs on post-cutoff web content with citations, raising post-cutoff accuracy from 15% to 90% on evolving topics; appearing as a cited source in continuous-learning pipelines - not just live search retrieval - is an emerging optimization surface where high-quality, stable, citable content gets embedded into model weights over time.

- [Retrieval Collapses When AI Pollutes the Web](https://arxiv.org/abs/2602.16136) - With 67% synthetic content in a retrieval pool, over 80% of surfaced results become AI-generated while measured accuracy stays flat; original human-authored content with unique provenance signals becomes increasingly scarce and therefore more likely to be prioritized by quality-aware rankers that penalize synthetic homogeneity.

- [Prism-Reranker](https://arxiv.org/abs/2604.23734) - Next-generation rerankers assessing contribution and extracting evidence passages represent the reranking evolution to design for: content that explicitly articulates its unique value - not just its topic coverage - will be systematically preferred over topic-matching content that makes no distinct contribution claim.

- [EcoGEO: Trajectory-Aware Evidence Ecosystems](https://arxiv.org/abs/2605.12887) - Site architecture designed for agent navigation patterns - not just human usability or traditional crawl efficiency - is an optimization surface with no equivalent in traditional SEO; trajectory-aware coordination of navigation and support pages is the earliest production-ready framework for environment-level agentic optimization.

- [SearchLLM: Aligning LLMs with Searcher Preferences](https://arxiv.org/abs/2603.10473) - Multi-dimensional reward modeling (accuracy, comprehensiveness, engagement) deployed at scale on RedNote shows that generative search engines are simultaneously optimizing across multiple quality dimensions, meaning content ranked well on all three will be systematically preferred over content maximizing any single signal.

---

## Related Lists

- [awesome-aeo-seo](https://github.com/discoveredlabs/awesome-aeo-seo) - Flagship list covering AEO, GEO, and SEO foundations, architecture, research, measurement, and content strategy.
- [awesome-aeo-seo-tools](https://github.com/discoveredlabs/awesome-aeo-seo-tools) - Free and open-source tools for AEO auditing, monitoring, technical configuration, and content evaluation.

---

## License

[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) - Discovered Labs, 2026. To the extent possible under law, the contributors have waived all copyright and related rights to this work.
