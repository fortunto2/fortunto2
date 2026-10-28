### Hi, I'm Rustam Salavatov

I build AI agents in Rust, and on-device AI products on top of them. Agents that run 104 tasks in parallel need zero GC, and tools that process video and audio on the device need real performance. Python for glue and ML pipelines, Swift for the iPhone, Rust for everything that ships.

Every build and every event is written up at [rustman.org](https://rustman.org). Founder of [SuperDuperAI](https://superduperai.co).

#### Building now

| Product | What | Links |
|---------|------|-------|
| **Life2Film** | iPhone app that edits the videos already on your phone into a film cut to the beat. Offline, no account, nothing uploaded. A Rust video-analysis core (11 crates) under a Swift app | [App Store](https://apps.apple.com/app/id6761656888?pt=128499174&ct=github-profile&mt=8) · [life2film.com](https://life2film.com) · Mac: `brew install --cask fortunto2/tap/life2film-studio` |
| **Air Signal** | Live air quality and outdoor comfort for 62,261 cities. Community PM2.5 sensors merged with model data in one Rust core, compiled to WASM for the web | [airsignal.app](https://airsignal.app) · core: [airq](https://github.com/fortunto2/airq) |
| **Crew Air** | Hour-by-hour shift plans for outdoor crews under California's wildfire-smoke (§5141.1) and heat (§3395) rules. Built at the Stanford Cleantech Hackathon | [airsignal.app/crew](https://airsignal.app/crew) |
| **SuperDuper DSP** | 36 free, MIT-licensed audio plugins in Rust (CLAP, VST3, AU). An AI agent can read what every plugin hears and turn every knob over MCP | [download](https://github.com/fortunto2/superduper-dsp/releases/latest) · [repo](https://github.com/fortunto2/superduper-dsp) |

Life2Film started in 2014 as a website, and that first version reached 1M+ users. That version is gone; the app is a rewrite from scratch, mobile first. The iPhone apps for Air Signal and Crew Air are on their way to the App Store. Nine of my iPhone apps are live: [App Store](https://apps.apple.com/us/developer/id1872989980).

#### Why Rust for agents

Most agent frameworks are Python wrappers around API calls. That works until you need parallel tool execution, sub-second response times, or WASM deployment. I build the infrastructure layer in Rust: the LLM client, the agent loop, the tools. Then concrete agents inherit the stack and add domain logic.

#### The crate tree

```
openai-oxide            LLM client (caching, WebSockets, structured outputs)
  └─ sgr-agent          Agent framework (structured CoT, function calling, providers)
       ├─ sgr-agent-core     Tool trait, FileBackend trait, AgentContext
       ├─ sgr-agent-tools    File tools (read, write, smart search, JS eval, apply_patch...)
       ├─ sgr-agent-ml       ONNX embeddings, centroid classifier, adaptive k-NN
       └─ agents built on top:
            ├─ agent-bit      Competition agent (BitGN PAC1, 74/104)
            ├─ rust-code      Terminal coding agent (TUI, MCP, skills)
            └─ supervox-agent Voice agent (live translate, post-call analysis)
```

[openai-oxide](https://github.com/fortunto2/openai-oxide) is the foundation. Persistent WebSockets, SIMD JSON, hedged requests. Published on [crates.io](https://crates.io/crates/openai-oxide) and [PyPI](https://pypi.org/project/openai-oxide/), and for Node as `npm install openai-oxide`.

[sgr-agent](https://github.com/fortunto2/rust-code/tree/master/crates/sgr-agent) sits on top. Two-phase function calling (reasoning then action), provider routing, parallel tool execution. The `FileBackend` trait means the same tools work over RPC, local filesystem, or in-memory mocks.

[sgr-agent-tools](https://github.com/fortunto2/rust-code/tree/master/crates/sgr-agent-tools) is the reusable toolkit: smart search (fuzzy + Levenshtein), batch read, JS eval via Boa engine, Codex-compatible diffs.

[sgr-agent-ml](https://github.com/fortunto2/rust-code/tree/master/crates/sgr-agent-ml) handles on-device ML: ONNX bi-encoder embeddings, cosine-similarity classifier, adaptive k-NN with persistence. Powers agent-bit's security detection (MiniLM + DeBERTa NLI) without API calls.

#### Agents built on this

| Agent | What | Result |
|-------|------|--------|
| [agent-bit](https://github.com/fortunto2/agent-bit) | PAC1 competition: CRM workspace, security detection, 15 skills, ONNX classifiers | 74/104 (GPT-5.4), up from 17 on competition day |
| [rust-code](https://github.com/fortunto2/rust-code) | Terminal coding agent: TUI, tmux tasks, MCP, fuzzy search | `brew install fortunto2/tap/rust-code` |

Both share the same stack. agent-bit adds ONNX security classifiers and a pipeline state machine. rust-code adds TUI and filesystem integration. The framework handles the common 80%.

More about the architecture: [How I spent $250+ on an AI agent competition](https://rustman.org/posts/pac1-competition-retrospective/)

#### Other Rust tools

| Project | What | Install |
|---------|------|---------|
| [solograph](https://github.com/fortunto2/solograph) | MCP server for a portfolio of projects: symbol search across every repo, full-text search over agent sessions and a knowledge base. One Rust binary, 12 tools, SQLite FTS5, no vectors | `make rust-install` |
| [airq](https://github.com/fortunto2/airq) | Air quality CLI. Sensor + model merge, EPA NowCast, WASM core behind airsignal.app | `brew install fortunto2/tap/airq` |
| [visa-photo](https://github.com/fortunto2/visa-photo) | Visa and passport photos for 12 countries. Background removal, runs in the browser, Dioxus desktop | `brew install fortunto2/tap/visa-photo` |
| [supervox](https://github.com/fortunto2/supervox) | Voice toolkit. STT, VAD, TTS, mic capture | `cargo add voxkit` |
| [OpenWok](https://github.com/fortunto2/OpenWok) | Open-source food delivery. Dioxus fullstack, QR payments, privacy-first | |

#### Claude Code tooling

| Project | What |
|---------|------|
| [solo-factory](https://github.com/fortunto2/solo-factory) | Claude Code plugin. 57 skills, 2 agents, 12 stack templates: research, validate, build, deploy, launch |
| [seo-cli](https://github.com/fortunto2/seo-cli) | SEO CLI. Google Search Console, Bing, Yandex, IndexNow |
| [invoice-pdf-crm](https://github.com/fortunto2/invoice-pdf-crm) | File-based CRM. PDF invoices, letters, company cards |

#### Hackathons

Eight in 23 days in California in October 2026: six as a builder, one as a judge, one as an observer. Each one is written up, the misses included. [All of them](https://rustman.org/wiki/hackathons/).

| When | Event | What I did | Links |
|------|-------|------------|-------|
| 25 Oct 2026 | Cal Hacks 13.0, San Francisco | Judge for Technical Complexity, 652 projects | [write-up](https://rustman.org/posts/cal-hacks-13-2026/) |
| 25 Oct 2026 | Dell x NVIDIA AI Hackathon, San Francisco | Observer: eight always-on agents on one GB10 | [write-up](https://rustman.org/posts/dell-nvidia-gb10-hackathon-2026/) |
| 23-24 Oct 2026 | Stanford Cleantech Hackathon | Crew Air, team of two. Missed the finals | [write-up](https://rustman.org/posts/stanford-cleantech-hackathon-2026/) · [repo](https://github.com/fortunto2/stanford-cleantech-2026) |
| 22 Oct 2026 | super{set} hackathon, San Francisco | ConsentShield: your consent decides what leaves the screen for a cloud agent | [write-up](https://rustman.org/posts/superset-hackathon-consentshield-2026/) · [repo](https://github.com/fortunto2/consentshield) |
| 21 Oct 2026 | OC AI Builders Lightning Hackathon, Irvine | BroCheck: a 50-minute build where a decision model, not an LLM, judges two photos | [write-up](https://rustman.org/posts/oc-lightning-hackathon-brocheck-2026/) · [repo](https://github.com/fortunto2/mama-approves) |
| 9 Oct 2026 | Cyberdefense Hackathon, San Francisco | **3rd place overall of 78.** ScreenShield: DLP for screen recordings, one Rust binary | [write-up](https://rustman.org/posts/cyberdefense-hackathon-screenshield-2026/) · [repo](https://github.com/fortunto2/screenshield) |
| 4 Oct 2026 | Build Personal Agents Hack (Neon), San Francisco | Life2Film Director: an agent that learns your editing taste | [write-up](https://rustman.org/posts/neon-personal-agents-life2film-director-2026/) · [repo](https://github.com/fortunto2/tech-week-agent) |
| 3-4 Oct 2026 | Hack-Nation 7, Palo Alto | AI Apprentice: a voice agent that asks an expert why, team of two | [write-up](https://rustman.org/posts/hack-nation-ai-apprentice-2026/) · [repo](https://github.com/fortunto2/ai-apprentice) |
| Apr 2026 | BitGN PAC1 | agent-bit: 17/104 on the day, 74/104 two days later | [write-up](https://rustman.org/posts/pac1-competition-retrospective/) · [repo](https://github.com/fortunto2/agent-bit) |
| Nov 2025 | Supabase Hackathon, Lisbon | **2nd place.** SuperStoryboard: text to storyboard, live sync into Figma | [write-up](https://rustman.org/projects/project-superstoryboard/) · [repo](https://github.com/fortunto2/superstoryboard) |

#### Where I write

- [rustman.org](https://rustman.org): every build and event, in English
- [LinkedIn](https://www.linkedin.com/in/rustam-salavatov/)
- YouTube [@SuperDuperStartup](https://www.youtube.com/@SuperDuperStartup): building startups with AI agents, in Russian
- YouTube [@salavatov](https://www.youtube.com/@salavatov): life, trips and the talks I record
- Telegram [@deeplearning_ru](https://t.me/deeplearning_ru): AI news and my own builds, in Russian
- Telegram [@life2film](https://t.me/life2film): my personal channel, in Russian

#### Stack

Rust (agents, tools, WASM) | Python (ML, scrapers, CLI) | TypeScript (web) | Swift (iOS) | Kotlin (Android)
