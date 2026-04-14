### Hi, I'm Rustam

I build AI agents in Rust. Not because it's trendy, but because agents that run 104 tasks in parallel need zero-GC, and tools that process video/audio on device need real performance. Python for glue and ML pipelines, Rust for everything that ships.

Founder of [SuperDuperAI](https://superduperai.co) | Blog: [rustman.org](https://rustman.org)

#### Why Rust for agents

Most agent frameworks are Python wrappers around API calls. That works until you need parallel tool execution, sub-second response times, or WASM deployment. I build the infrastructure layer in Rust: the LLM client, the agent loop, the tools. Then concrete agents inherit the stack and add domain logic.

#### The crate tree

```
openai-oxide          LLM client (caching, WebSockets, structured outputs)
  └─ sgr-agent        Agent framework (structured CoT, function calling, providers)
       ├─ sgr-agent-core    Tool trait, FileBackend trait, AgentContext
       ├─ sgr-agent-tools   14 reusable tools (read, write, search, eval, apply_patch...)
       └─ agents built on top:
            ├─ agent-bit     Competition agent (PAC1 benchmark, 74/104)
            └─ rust-code     Terminal coding agent (TUI, MCP, skills)
```

[openai-oxide](https://github.com/fortunto2/openai-oxide) is the foundation. Persistent WebSockets, SIMD JSON, hedged requests. Published on [crates.io](https://crates.io/crates/openai-oxide), [npm](https://www.npmjs.com/package/openai-oxide), [PyPI](https://pypi.org/project/openai-oxide/).

[sgr-agent](https://github.com/fortunto2/rust-code/tree/master/crates/sgr-agent) sits on top. Two-phase function calling (reasoning then action), provider routing, parallel tool execution. The `FileBackend` trait means the same tools work over RPC, local filesystem, or in-memory mocks.

[sgr-agent-tools](https://github.com/fortunto2/rust-code/tree/master/crates/sgr-agent-tools) is the reusable toolkit: smart search (fuzzy + Levenshtein), batch read, JS eval via Boa engine, Codex-compatible diffs. Any new agent gets these for free.

#### Agents built on this

| Agent | What | Score |
|-------|------|-------|
| [agent-bit](https://github.com/fortunto2/agent-bit) | PAC1 competition: CRM workspace, security detection, 15 skills, ONNX classifiers | 74/104 (GPT-5.4) |
| [rust-code](https://github.com/fortunto2/rust-code) | Terminal coding agent: TUI, tmux tasks, MCP, fuzzy search | daily driver |

Both share the same `openai-oxide` + `sgr-agent` + `sgr-agent-tools` stack. agent-bit adds ONNX security classifiers and a pipeline state machine. rust-code adds TUI and filesystem integration. The framework handles the common 80%.

More about the architecture: [How I spent $250+ on an AI agent competition](https://rustman.org/posts/pac1-competition-retrospective/)

#### Other Rust tools

| Project | What | Install |
|---------|------|---------|
| [airq](https://github.com/fortunto2/airq) | Air quality CLI. Sensor + model merge, WASM core | `brew install fortunto2/tap/airq` |
| [visa-photo](https://github.com/fortunto2/visa-photo) | Biometric visa photos. AI background removal, Dioxus desktop | `brew install fortunto2/tap/visa-photo` |
| [supervox](https://github.com/fortunto2/supervox) | Voice toolkit. STT, VAD, TTS, mic capture | `cargo add voxkit` |

#### Python: agent infra & tools

| Project | What |
|---------|------|
| [solo-factory](https://github.com/fortunto2/solo-factory) | Claude Code plugin. 27 skills, 3 agents, full startup pipeline |
| [solograph](https://github.com/fortunto2/solograph) | Code intelligence MCP server. FalkorDB + tree-sitter, KB search, session history |
| [seo-cli](https://github.com/fortunto2/seo-cli) | SEO CLI. Google Search Console, Bing, Yandex, IndexNow |
| [invoice-pdf-crm](https://github.com/fortunto2/invoice-pdf-crm) | File-based CRM. PDF invoices, letters, company cards |

#### Open source contributions

- [rust-genai#177](https://github.com/jeremychone/rust-genai/pull/177) -- HTTP performance (gzip, TCP_NODELAY, HTTP/2)
- [async-openai#532](https://github.com/64bit/async-openai/pull/532) -- HTTP performance

#### Stack

Rust (agents, tools, WASM) | Python (ML, MCP servers, CLI) | TypeScript (web) | Swift (iOS) | Kotlin (Android)
