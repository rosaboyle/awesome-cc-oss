# Awesome Claude Code OSS [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of resources related to the Claude Code source code leak (March 31, 2026) — the incident where Anthropic accidentally shipped a 59.8 MB source map file (`cli.js.map`) in version 2.1.88 of the `@anthropic-ai/claude-code` npm package, exposing ~1,900 files and 512,000+ lines of proprietary TypeScript source code.

---

## Contents

- [What Happened](#what-happened)
- [Source Code Archives](#source-code-archives)
- [Clean-Room Reimplementations](#clean-room-reimplementations)
- [Technical Breakdowns & Analysis](#technical-breakdowns--analysis)
- [Key Discoveries from the Leak](#key-discoveries-from-the-leak)
- [News Coverage](#news-coverage)
- [Expert & Community Reactions](#expert--community-reactions)
  - [X / Twitter](#x--twitter)
  - [Reddit Threads](#reddit-threads)
  - [Hacker News](#hacker-news)
- [Blog Posts & Deep Dives](#blog-posts--deep-dives)
- [Videos & Podcasts](#videos--podcasts)
- [Security & Legal Implications](#security--legal-implications)
- [Related Prior Incidents](#related-prior-incidents)
- [Further Reading](#further-reading)

---

## What Happened

On **March 31, 2026**, security researcher **Chaofan Shou** ([@Fried_rice](https://x.com/Fried_rice) on X), an intern at Solayer Labs, discovered that the entire source code of **Claude Code** — Anthropic's flagship AI coding CLI — was publicly accessible via a source map file (`.map`) bundled into the published npm package `@anthropic-ai/claude-code` v2.1.88.

- The `.map` file was **59.8 MB** and contained the full, readable original TypeScript source
- **~1,900 files** and **512,000+ lines of code** were exposed
- The leak was caused by a missing `.npmignore` rule or bundler misconfiguration (Bun generates source maps by default)
- Anthropic scrambled to remove the package, but the code was already archived and forked **41,500+ times** on GitHub within hours
- This was the **second time** Claude Code source was leaked — the first was in **February 2025**

---

## Source Code Archives

- [**Kuberwastaken/claude-code**](https://github.com/Kuberwastaken/claude-code) — The most popular archive with a full technical breakdown README (now converted to a clean-room Rust reimplementation)
- [**instructkr/claw-code**](https://github.com/instructkr/claw-code) — "The fastest repo in history to surpass..." — Clean-room Python rewrite capturing architectural patterns

---

## Clean-Room Reimplementations

- [**Kuberwastaken/claude-code (Rust)**](https://github.com/Kuberwastaken/claude-code) — Rust port of Claude Code's behavior, clean-room reimplementation
- [**instructkr/claw-code (Python)**](https://github.com/instructkr/claw-code) — Python rewrite by Sigrid Jin (top Claude API consumer, featured in WSJ), capturing the agent harness architecture
- [**JackChen-me/open-multi-agent**](https://github.com/JackChen-me/open-multi-agent) — MIT-licensed TypeScript ~8,000-line clean-room multi-agent SDK inspired by the leak; runs in-process unlike `claude-agent-sdk`

---

## Technical Breakdowns & Analysis

- [**Kuberwastaken's Breakdown**](https://github.com/Kuberwastaken/claude-code) — Comprehensive README covering every major system: BUDDY, KAIROS, Dream, Undercover Mode, Coordinator Mode, tool registry, and more
- [**Kuber's Blog Post**](https://kuber.studio/blog/AI/Claude-Code's-Entire-Source-Code-Got-Leaked-via-a-Sourcemap-in-npm,-Let's-Talk-About-it) — Same breakdown with better reading UX
- [**alex000kim — "The Claude Code Source Leak: fake tools, frustration regexes..."**](https://alex000kim.com/posts/2026-03-31-claude-code-source-leak/) — Deep dive into fake tools, frustration detection, and anti-distillation measures
- [**dev.to — Gabriel Anhaia**](https://dev.to/gabrielanhaia/claude-codes-entire-source-code-was-just-leaked-via-npm-source-maps-heres-whats-inside-cjo) — "1,900 files. 512,000+ lines. Everything."
- [**apiyi.com — Interpretation of the Claude Code source code leak**](https://help.apiyi.com/en/claude-code-source-leak-march-2026-impact-ai-agent-industry-en.html) — Impact analysis on the AI agent industry

---

## Key Discoveries from the Leak

Notable features and systems found in the leaked source:

- **Undercover Mode** — System that hides Anthropic identity when employees use Claude Code on public/open-source repos. Prompt: *"You are operating UNDERCOVER... Do not blow your cover."*
- **BUDDY** — A full Tamagotchi-style companion pet system with gacha mechanics, 18 species, shiny variants, procedurally generated stats, and "soul descriptions"
- **KAIROS** — "Always-On Claude" — a persistent, proactive assistant mode that watches and acts without user input (gated behind `PROACTIVE` feature flag)
- **Dream System** — Background memory consolidation engine where Claude literally "dreams" — has a three-gate trigger and four phases
- **ULTRAPLAN** — 30-minute remote planning session system
- **Multi-Agent Orchestration / Coordinator Mode** — Full multi-agent system spawning parallel workers, activated via `CLAUDE_CODE_COORDINATOR_MODE=1`
- **Agent Teams / Swarm** — In-process and process-based teammates with tmux/iTerm2 panes (feature gate: `tengu_amber_flint`)
- **40+ Tool Registry** — Complete tool system with risk classification (LOW/MEDIUM/HIGH), ML-based auto-approval, and YOLO classifier
- **Penguin Mode** — Internal codename for "Fast Mode"
- **Upcoming Models** — References to **Capybara** (new model family, v2, with 1M context), **Opus 4.7**, and **Sonnet 4.8**
- **Internal Codename: Tengu** — Claude Code's internal project codename, appearing hundreds of times as prefix for feature flags
- **Anti-Distillation Measures** — Fake tools and frustration detection regexes to prevent model distillation
- **Hidden Beta Headers** — Unreleased API features including `redact-thinking`, `afk-mode`, `advisor-tool`, `task-budgets`, and more
- **Supply Chain Attack** — Between 00:21–03:29 UTC on March 31, malicious `axios` versions (1.14.1, 0.30.4) containing a RAT were distributed to users who installed during the window

---

## News Coverage

- [**VentureBeat** — "Claude Code's source code appears to have leaked: here's what we know"](https://venturebeat.com/technology/claude-codes-source-code-appears-to-have-leaked-heres-what-we-know)
- [**Fortune** — "Anthropic leaks its own AI coding tool's source code in second security lapse"](https://fortune.com/2026/03/31/anthropic-source-code-claude-code-data-leak-second-security-lapse-days-after-accidentally-revealing-mythos/)
- [**CNBC** — "Anthropic leaks part of Claude Code's internal source code"](https://www.cnbc.com/2026/03/31/anthropic-leak-claude-code-internal-source.html)
- [**The Register** — "Anthropic accidentally exposes Claude Code source code"](https://www.theregister.com/2026/03/31/anthropic_claude_code_source_code/)
- [**NDTV** — "Anthropic's AI Coding Tool Leaks Its Own Source Code For The Second Time In A Year"](https://www.ndtv.com/science/anthropics-ai-coding-tool-leaks-its-own-source-code-for-the-second-time-in-a-year-11291517)
- [**NDTV Profit** — "Anthropic Source Code Leaked For Second Time In A Week"](https://www.ndtvprofit.com/technology/anthropic-source-code-leaked-for-second-time-in-a-week-heres-what-it-means-11294259)
- [**Decrypt** — "Anthropic Accidentally Leaked Claude Code's Source — The Internet Is Keeping It Forever"](https://decrypt.co/362917/anthropic-accidentally-leaked-claude-code-source-internet-keeping-forever)
- [**Cybernews** — "Full source code for Anthropic's Claude Code leaks"](https://cybernews.com/security/anthropic-claude-code-source-leak/)
- [**BleepingComputer** — "Claude Code source code accidentally leaked in NPM package"](https://www.bleepingcomputer.com/news/artificial-intelligence/claude-code-source-code-accidentally-leaked-in-npm-package/)
- [**Bitcoin News** — "Anthropic Source Code Leak 2026: Claude Code CLI Exposed via npm Source Map Error"](https://news.bitcoin.com/anthropic-source-code-leak-2026-claude-code-cli-exposed-via-npm-source-map-error/)
- [**Piunika Web** — "Anthropic's Claude Code source appears to have been leaked via npm registry"](https://piunikaweb.com/2026/03/31/anthropic-claude-code-source-leaked-npm-registry/)
- [**MLQ.ai** — "Anthropic's Claude Code Exposes Source Code Through Packaging Error for Second Time"](https://mlq.ai/news/anthropics-claude-code-exposes-source-code-through-packaging-error-for-second-time/)
- [**NDTV Feature** — "'2026 Just Got Crazy': Internet Erupts After Anthropic's Claude Source Code Leak"](https://www.ndtv.com/feature/2026-just-got-crazy-internet-erupts-after-anthropics-claude-source-code-leak-shakes-ai-industry-11294628)

---

## Expert & Community Reactions

### X / Twitter

- [**@Fried_rice (Chaofan Shou)** — Original discovery post](https://x.com/Fried_rice) — The tweet that started it all, with a direct link to the full `src.zip`
- [**@zivdotcat (dev)** — "babe wake up. Claude Code is finally open source"](https://x.com/zivdotcat) — Viral tweet capturing community sentiment

### Reddit Threads

- [**r/LocalLLaMA** — "Claude code source code has been leaked via a .map file"](https://www.reddit.com/r/LocalLLaMA/comments/1s8ijfb/claude_code_source_code_has_been_leaked_via_a_map/) — 1,719 upvotes, 331 comments
- [**r/LocalLLaMA** — "Claude Code's source just leaked — I extracted its multi-agent architecture"](https://www.reddit.com/r/LocalLLaMA/comments/1s8xj2e/claude_codes_source_just_leaked_i_extracted_its/) — 136 upvotes, discussion on clean-room legality
- [**r/ClaudeAI** — "Claude Code's source code just leaked — so I had..."](https://www.reddit.com/r/ClaudeAI/comments/1s8xfwt/claude_codes_source_code_just_leaked_so_i_had/) — 228 upvotes, debate on AI-generated code copyright
- [**r/ClaudeAI** — "I dug through Claude Code's leaked source and..."](https://www.reddit.com/r/ClaudeAI/comments/1s8lkkm/i_dug_through_claude_codes_leaked_source_and/) — 2,019 upvotes, 291 comments — top comment: *"Makes me think my work code is too high quality lmao"*
- [**r/ClaudeAI** — "Claude code source code has been leaked via a .map file"](https://www.reddit.com/r/ClaudeAI/comments/1s8ifm6/claude_code_source_code_has_been_leaked_via_a_map/) — *"Looks like someone at Anthropic vibed a little too hard"*
- [**r/singularity** — "Claude code source code has been leaked"](https://www.reddit.com/r/singularity/comments/1s8izpi/claude_code_source_code_has_been_leaked_via_a_map/) — 565 upvotes, 169 comments

### Hacker News

- [**HN Discussion** — "The Claude Code Source Leak"](https://news.ycombinator.com/item?id=47586778) — alex000kim's analysis on the front page

---

## Blog Posts & Deep Dives

- [**alex000kim** — "The Claude Code Source Leak: fake tools, frustration regexes..."](https://alex000kim.com/posts/2026-03-31-claude-code-source-leak/) — Covers anti-distillation, frustration detection, fake tools, and the Bun bug causing 250K wasted API calls/day
- [**Kuber.studio** — Full technical breakdown](https://kuber.studio/blog/AI/Claude-Code's-Entire-Source-Code-Got-Leaked-via-a-Sourcemap-in-npm,-Let's-Talk-About-it)
- [**dev.to / Gabriel Anhaia** — Source maps deep dive](https://dev.to/gabrielanhaia/claude-codes-entire-source-code-was-just-leaked-via-npm-source-maps-heres-whats-inside-cjo)
- [**apiyi.com** — Impact on the AI agent industry](https://help.apiyi.com/en/claude-code-source-leak-march-2026-impact-ai-agent-industry-en.html)

---

## Videos & Podcasts

> 🚧 Section will be updated as video essays and podcast episodes are released covering the incident.

---

## Security & Legal Implications

- **DMCA Takedowns** — Anthropic issued a barrage of DMCA takedown notices across GitHub; many mirrors and forks were removed but the code had already spread
- **Clean-Room Defense** — Multiple developers argued their rewrites are "clean-room" implementations, constituting new creative works
- **AI Copyright Paradox** — Reddit debate: if Anthropic claims Claude wrote its own code, can AI-generated code be copyrighted?
- **Supply Chain Risk** — Malicious `axios` packages (v1.14.1, v0.30.4) containing a RAT were distributed during the leak window (00:21–03:29 UTC, March 31)
- **No User Data Exposed** — Anthropic confirmed no user data was in the leak; core Claude models were unaffected
- **Anthropic's Statement** — Acknowledged "human error" to The Register; no broader post-mortem published as of April 1, 2026

---

## Related Prior Incidents

- **February 2025** — An early version of Claude Code accidentally exposed its original source code in a similar packaging breach
- **Late March 2026** — Anthropic accidentally revealed details about the internal "Mythos" project days before this leak

---

## Further Reading

- [npm package: `@anthropic-ai/claude-code`](https://www.npmjs.com/package/@anthropic-ai/claude-code)
- [Bun Bundler Source Map Bug (oven-sh/bun#28001)](https://github.com/oven-sh/bun/issues/28001) — Related Bun bug filed March 11, 2026
- [Anthropic Official Website](https://www.anthropic.com/)
- [Claude Code Product Page](https://www.anthropic.com/claude-code)

---

## Contributing

Contributions welcome! Please open an issue or submit a PR if you find new resources, articles, analysis, or community reactions related to the Claude Code source leak.

---

## Disclaimer

This repository is a curated collection of **links and references** for educational and archival purposes. It does not host or distribute any proprietary source code. All linked content is publicly available on the internet.

---

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the authors have waived all copyright and related rights to this work.
