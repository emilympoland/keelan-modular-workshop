# Build Plan — `langchain-deep-agents-workshop-ext`

Date: 2026-09-30
Status: Slice 1 complete and pushed (challenges 01-setup, 02-your-first-deep-agent), proven
green via `instruqt track test`, reviewed and approved by the user. Continuing with Slice 2
(challenge 3: Custom Tools & Subagents, on Tavily per the AgentCore no-go below). Full detail
in the track's `MEMORY.md`.
Source notebook: `modular-workshops/modules/01_deep_agents.ipynb` (internally titled "Module 2:
Deep Agents", commit pinned via this repo's `WORKSHOP_COMMIT` convention)

## Track

- **Working name / final slug:** `langchain-deep-agents-workshop-ext`
- **Title:** requested as `[AH] Create a Deep Agent Workshop`, which reads like a personal
  draft label (initials prefix), not a customer-facing title. Proposed polished title:
  **"Deep Agents 201 — From `create_agent` to a Complete Deep Agent"** (mirrors the sibling
  track's "LangGraph 201 — Build a Research Agent from Scratch"). Open until confirmed.
- **Audience:** same as sibling tracks — LangChain customers upskilling on agent-building,
  intermediate level, assumes LangGraph/tool-use fundamentals.
- **Learner outcome:** understand what `create_deep_agent()` gives for free vs.
  `create_agent()`, and build one continuously-accumulating deep agent — across 6 content
  challenges — that ends at the source notebook's exact "complete agent" state (10 kwargs:
  `model, tools, subagents, backend, store, middleware, checkpointer, interrupt_on, memory,
  skills`).

## Acceptance criteria / Definition of Done

- **Learner:** completes each challenge by running/editing notebook cells; every Check
  verifies real behavior — a live LangSmith trace (web search actually invoked, subagent
  delegation happened, HITL interrupt was approved) or a real-disk artifact (virtual files
  extracted out of agent state, matching the existing `langchain-deep-agents-langsmith-ext`
  convention) — never just "a cell ran."
- **Author:** all 7 challenges (Setup + 6 content sections) push and go green via
  `./scripts/track-test.sh`; notebooks generated via `gen_notebooks.py` heredoc only, never
  committed `.ipynb`; each section's `create_deep_agent` cell is its own cell showing the full
  accumulated kwarg list with this section's new line(s) commented out; all 7 source images
  ported into both notebook markdown and the Instruqt challenge; the web-search tool choice
  (AgentCore vs. Tavily) is settled by a fail-fast spike, not mid-build debugging.

## Challenge breakdown (7 total)

| # | Slug | Notebook section(s) | Check |
|---|---|---|---|
| 01 | `setup` | Setup cell | LangSmith key verified + Bedrock model warm + (web-search dependency reachable — AgentCore Gateway or Tavily key, per slice-0 outcome) |
| 02 | `your-first-deep-agent` | 1.1 | `/haiku.txt` extracted to real disk, non-empty |
| 03 | `custom-tools-and-subagents` | 1.2 + 1.3 | LangSmith trace shows a real web-search span nested under the `research-agent` subagent's `task()` call |
| 04 | `backends-and-memory` | 1.4 | second thread reads back a fact saved to `/memories/` by the first — real cross-thread persistence, not just a file on disk |
| 05 | `middleware-and-hitl` | 1.5 + 1.6 | audit log has an entry *and* the HITL interrupt was actually paused-then-approved (checked via trace, not just local state) |
| 06 | `agents-and-skills` | 1.7 | agent output reflects the `/AGENTS.md` identity and the LinkedIn skill's format rules |
| 07 | `the-complete-agent` | 1.8 | all of the above, combined, on the final 10-kwarg agent |

## Web search tool — RESOLVED: Tavily + learner getpass (2026-09-30)

The AgentCore spike was run end-to-end in a real throwaway Instruqt sandbox and hit a
hard **no-go**: AWS blocked `iam:GetRole` (needed to create the Gateway's execution role)
with an **explicit deny in an Organizations Service Control Policy** on the `langchain`
team's AWS org. An SCP deny overrides any IAM identity policy, including a custom
`iam_policy:` in `config.yml` — this is not a permissions gap fixable from track config.
Per the fail-fast instruction, immediately pivoted to **Tavily + learner getpass** (the
originally-recommended path, already proven in the langgraph sibling track). No further
AgentCore attempt. See `MEMORY.md` for the full verdict and log.

**Practical effect on this plan:**
- Setup (challenge 01) needs a Tavily getpass prompt added back in, alongside LangSmith.
- `config.yml` drops the AgentCore IAM policy entirely; back to the plain
  `AmazonBedrockLimitedAccess` managed-policy pattern, no custom `iam_policy:`.
- Challenge 03's check (verify a real web-search span in the LangSmith trace) is unaffected
  in design — same trace-verification approach works regardless of which tool is behind it.

## Web search tool — original fail-fast decision (2026-09-30, superseded above)

User wants to attempt **Bedrock AgentCore Web Search** first (genuinely keyless on Bedrock —
no third-party API key, uses Amazon's own index) but **fail fast**: if a standalone spike
shows it's broken, blocked, or not feasible within this repo's per-sandbox provisioning flow,
drop it immediately and build section 2 on **Tavily + learner getpass** instead (the
originally-recommended, already-proven path in this repo).

- This is a go/no-go gate, not a debugging commitment — no sunk-cost attempt to force
  AgentCore to work.
- **Slice 0, before anything else gets built:** stand up an AgentCore Gateway by hand (or via
  a throwaway script) and confirm `langchain_aws.tools.create_web_search_toolkit()` returns a
  working tool that can actually be invoked and traced. This requires a new IAM policy for
  `bedrock-agentcore:*` actions (the existing `AmazonBedrockLimitedAccess` managed policy used
  by the sibling tracks doesn't cover Gateway/AgentCore actions), and is region-locked to
  `us-east-1` / `eu-west-1` / `ap-northeast-1`.
- **If AgentCore fails the spike:** fall back to Tavily + learner getpass immediately. This
  means Setup (challenge 01) needs a Tavily getpass prompt added back in — it was dropped from
  the current plan only on the assumption AgentCore would eliminate that need.
- Either way, section 3's check (verify a real, non-fallback web-search span in the LangSmith
  trace) works the same regardless of which tool is behind it.

## Infra (reusing `langchain-langgraph-langsmith-ext`, the newest sibling track)

- Single `workstation` VM (Ubuntu 24.04, 8GB/2cpu), `aws_accounts` scoped to Bedrock — plus,
  if AgentCore survives the spike, a new custom IAM policy for `bedrock-agentcore:*` (new
  ground, not copy-paste from any existing track).
- No `config.yml` `secrets:` block if AgentCore works (learner-supplied LangSmith key via
  getpass only); a `TAVILY_API_KEY` secret or getpass prompt gets added back if the fallback
  fires.
- JupyterLab+nginx+systemd bootstrap, `uv sync`, and the `gen_notebooks.py` heredoc
  (`md()`/`code()`/`write()` helpers, confirmed escaping rules: a literal `\n` in cell code
  becomes `\\n` in the generator; a cell containing `"""` needs `'''` as the generator's outer
  quotes) — lifted wholesale from the langgraph track (read via
  `git show adlc-101-slice-0:langchain-langgraph-langsmith-ext/...`, since that track isn't
  merged to `main` yet — its full build lives on that branch, not on disk here).
- `ChatBedrockConverse` model wiring (`us.anthropic.claude-sonnet-4-6`, region from env) —
  identical block already proven in both sibling tracks.
- Virtual-file-to-real-disk extraction pattern (agent's `StateBackend` files are in-memory
  only; a notebook cell must pull `result["files"][...]` out to real disk before
  `check-workstation` can read it) — lifted from `langchain-deep-agents-langsmith-ext`'s
  challenge 2 (`/report.md` → `/root/workshop/output/report.md`).

## Directory tree

```
langchain-deep-agents-workshop-ext/
  01-setup/  02-your-first-deep-agent/  ...  07-the-complete-agent/
    (assignment.md, card.yml, check-workstation, solve-workstation)
  assets/  (langchain-icon.png + the 7 source PNGs, for in-assignment.md embedding)
  track_scripts/  (setup-workstation, cleanup-workstation)
  docs/  (this file, and future plan/handoff docs)
  track.yml
  config.yml
  MEMORY.md
```

Tabs: every challenge gets a `Notebook` tab (port 8888, path to that challenge's own generated
`.ipynb`); challenge 01 also gets a `Terminal` tab for a `verify-connection`-style CLI check,
matching the langgraph track's Setup challenge.

## Source notebook structure (verbatim section headers)

Internally titled "Module 2: Deep Agents" (filename says `01_`, a repo-wide numbering quirk —
don't port "Module N" language into the track). Eight sections under one `# Part 1: Deep
Agents`, all mapped onto the 6-challenge breakdown above:

- `1.1 Your First Deep Agent` → challenge 02
- `1.2 Custom Tools` + `1.3 Subagents: Isolated Delegation` → challenge 03 (combined)
- `1.4 Backends & Memory` → challenge 04
- `1.5 Middleware: Pluggable Behavior` + `1.6 HITL: Tool-Level Approval` → challenge 05 (combined)
- `1.7 AGENTS.md & Skills` → challenge 06
- `1.8 The Complete Agent` → challenge 07

Full verbatim code for every section (every `create_deep_agent` call, every custom tool,
backend, middleware, and skill definition) is captured in this session's research — see
`MEMORY.md` research notes before re-deriving any of it.

## Images (7 total — all need porting into this track's `assets/`)

All currently referenced from the source notebook as relative `../images/...` paths; none of
this repo's existing tracks have ever put an image inside an `assignment.md` body (only
`track.yml`'s icon uses an image) — **this is a build-phase verification item**, not yet
confirmed how Instruqt serves an inline asset in the challenge body.

- `deepAgentsDiag.png` — section 1.1
- `deepAgentSubagents.png` — section 1.3
- `deepAgentMiddleware.png`, `Offloading Inputs LangChain.png`, `Offloading Results
  LangChain.png`, `LangChain Summarization.png` — section 1.5 (four images, each paired with
  its own paragraph — needs inline interleaving, not a single image block)
- `deepAgentHITL.png` — section 1.6

## Flagged risks

1. **AgentCore Gateway provisioning** — addressed above via the fail-fast spike/fallback plan.
2. **Image-in-assignment.md has no precedent in this repo** — will verify against Instruqt
   docs how an inline asset actually renders in the challenge body during the build phase.
3. **Linearization deviation from upstream.** The source notebook's `create_deep_agent` calls
   don't naturally accumulate — sections 1.5 and 1.6 each reset to a fresh, separately-named
   agent variable that doesn't carry forward 1.4's `backend`/`store`; only 1.8 combines
   everything. This plan deliberately linearizes into one continuously-growing agent across
   all 6 challenges, per the user's explicit requirement that each section's new kwarg line(s)
   be commented out for the learner to uncomment. Flagging as an explicit, intentional
   deviation from upstream, not a silent one.

## Delivery

Iterative: build and push 1-2 challenges at a time so each can be previewed in the Instruqt UI
before continuing. Proposed order:
- **Slice 0:** AgentCore Gateway spike (standalone, go/no-go — not part of the track yet).
- **Slice 1:** track scaffold (`track.yml`, `config.yml`, `track_scripts/`, `MEMORY.md`,
  `assets/`) + challenge 01 (Setup) + challenge 02 (Your First Deep Agent).
- **Slice 2 onward:** remaining challenges, 1-2 per slice, per the user's stated preference.
