# Track Memory — langchain-deep-agents-workshop-ext (working name)

## Concept
"[AH] Create a Deep Agent Workshop" — a new Instruqt track built from LangChain's
`modular-workshops/modules/01_deep_agents.ipynb`. Section-by-section progression through
`create_agent` → `create_deep_agent`, teaching each capability the `deepagents` package adds
(custom tools, sub agents, backends/memory, middleware/HITL, skills), culminating in the
complete agent. Sibling to `langchain-langgraph-langsmith-ext` (built from `02_langgraph.ipynb`).

## Research Notes
- **AgentCore Web Search viability (researched 2026-09-30):** Real, GA product — "Web Search
  Tool on Amazon Bedrock AgentCore" (announced June 2026), a built-in connector target on the
  AgentCore Gateway, exposed via MCP (`connectorId: "web-search"`). Distinct from the
  AgentCore Browser Tool (form-filling sandbox) and Knowledge Bases web crawler (fixed-URL
  ingestion) — neither of those is a live search replacement. No third-party API key (uses
  Amazon's own index, not a Tavily/Brave/Google proxy) — but requires standing up a new
  AgentCore **Gateway** resource with a scoped IAM role (`bedrock-agentcore:InvokeWebSearch`),
  region-limited to `us-east-1`/`eu-west-1`/`ap-northeast-1`, priced at $7/1k queries +
  $0.005/1k Gateway invocations. LangChain-native today via `langchain-aws`:
  `from langchain_aws.tools import create_web_search_toolkit` →
  `create_web_search_toolkit(region=..., gateway_id=...)` returns a ready `StructuredTool` —
  no custom boto3 wrapper needed.
  Sources: [AWS AgentCore docs](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-connector-web-search-tool.html),
  [AWS ML blog](https://aws.amazon.com/blogs/machine-learning/introducing-web-search-on-amazon-bedrock-agentcore/),
  [langchain-aws PR #1280](https://github.com/langchain-ai/langchain-aws/pull/1280).

- **`langchain-deep-agents-langsmith-ext` deep-dive (2026-09-30, read-only reference):**
  Uses `from deepagents import create_deep_agent` (not `create_agent`), `ChatBedrockConverse`
  (`langchain_aws`) with `model_id=os.environ.get("BEDROCK_MODEL_SONNET", "us.anthropic.claude-sonnet-4-6")`,
  region from `AWS_DEFAULT_REGION`/`us-east-1` default — directly reusable Bedrock wiring.
  `checkpointer=MemorySaver()` from `langgraph.checkpoint.memory`, thread ids via
  `langsmith.uuid7`. One custom tool (`tavily_search`, `@tool(parse_docstring=True)` wrapping
  a `resilient_tavily_search` helper that lives upstream in modular-workshops, not in this
  repo) and one subagent dict (`research-agent`, capped at 3 searches) — matches the source
  notebook's 1.2/1.3 shape closely. `config.yml` declares `TAVILY_API_KEY` + `LANGSMITH_API_KEY`
  as `secrets:`, single `workstation` VM (Ubuntu 24.04, 2 vCPU/8GB), one `aws_accounts` entry
  scoped to `bedrock` service only (`AmazonBedrockLimitedAccess` managed policy) — a clean,
  minimal, reusable topology. No standalone `gen_notebooks.py` file — notebook generation is
  a heredoc baked directly into `track_scripts/setup-workstation` (stdlib `json.dump`, no
  nbformat), confirming the "generate at provision time, never commit .ipynb" pattern this
  repo's CLAUDE.md describes. Only one asset in the whole track: `assets/langchain-icon.png`
  (the track icon) — no images in any assignment.md or notebook markdown, so no precedent
  here for the image-embedding convention our new track needs (must get that from the
  langgraph track instead). No MEMORY.md exists in this track. Full agent-assembly code and
  config.yml captured verbatim in this session's subagent transcript if needed later.

## Design Decisions
- Working folder: `langchain-deep-agents-workshop-ext` (user chose the
  `langchain-*-langsmith-ext`-style name over a shorter alternative, for naming consistency
  with the two existing tracks).
- Git branch: `deep-agents-workshop`, cut from `main` (current branch `adlc-101-slice-0`
  belonged to unrelated work).
- Cross-track reuse scan: approved, running against the repo per REUSE-FINDER.md.
- Section names must mirror the source notebook's own headings verbatim, not be reworded.
- Every `create_deep_agent` call lives in its own cell; each section's cell shows the full
  accumulated kwargs from all prior sections, with this section's new line(s) commented out
  so the learner uncomments to progress (diff-in-context teaching pattern).
- Any image rendered in the source notebook must also appear in the corresponding Instruqt
  challenge.
- Section 1 ("Your first deep agent" — exact notebook name TBD from research) opens with
  plain `create_agent` failing the task "Write a haiku about AI to /haiku.txt, then read it
  back to me" (no file-write tool), then has the learner paste in the `create_deep_agent`
  version. Check: `/haiku.txt` has text.
- Section 2 ("Custom tools & sub agents" — combines notebook's 1.2 + 1.3): web search tool
  needs a keyless-on-Bedrock replacement for `tavily_search`. Candidate: AWS Bedrock
  AgentCore web search — viability under research (open question below until agent reports
  back). Learner uncomments `"tools": [web_search]` and `subagents=[research_subagent]`.
  Check: verify the web search tool was actually invoked, via LangSmith trace per this repo's
  established check pattern.
- Section 3 ("Backends & memory"): split into two cells — one defining the DB backend +
  store, one for `create_deep_agent`. Learner uncomments `backend=backend` and `store=store`,
  and copies a prompt from the instructions into the notebook.
- Delivery: build iteratively, 1-2 sections at a time, pushing after each so the user can
  preview in the Instruqt UI via `./scripts/track-test.sh`.

- **Source notebook structure (parsed 2026-09-30, real JSON via `curl`+`python json`, not
  WebFetch — WebFetch hallucinated a wrong numbering scheme on the first attempt, discard it):**
  Internally titled "Module 2: Deep Agents" despite the `01_` filename prefix (repo-wide
  numbering quirk, `.env.example` confirms it's consistently called Module 2 elsewhere too —
  avoid porting "Module N" language into the track). 8 top-level sections under one
  `# Part 1: Deep Agents`, all `1.x`, verbatim:
  `1.1 Your First Deep Agent` / `1.2 Custom Tools` / `1.3 Subagents: Isolated Delegation` /
  `1.4 Backends & Memory` / `1.5 Middleware: Pluggable Behavior` / `1.6 HITL: Tool-Level
  Approval` / `1.7 AGENTS.md & Skills` / `1.8 The Complete Agent`. This maps cleanly onto the
  user's 6-challenge plan: challenge 2 = 1.2+1.3 combined, challenge 4 = 1.5+1.6 combined,
  others 1:1.
  **Full verbatim code for every section, plus `utils/models.py` and `utils/search.py`
  contents, captured in this session's subagent transcript — pull from there when writing
  `gen_notebooks.py`, don't re-fetch.**
  - **Images (7 total, all `<img>` tags with relative `../images/...` paths, none resolve
    as-is in Instruqt — must vendor into track assets):** `deepAgentsDiag.png` (1.1),
    `deepAgentSubagents.png` (1.3), `deepAgentMiddleware.png` + `Offloading Inputs
    LangChain.png` + `Offloading Results LangChain.png` + `LangChain Summarization.png`
    (all 4 in the single 1.5 markdown cell, each paired with its own paragraph — 1.5 needs
    inline interleaving, not a single image block), `deepAgentHITL.png` (1.6). No images in
    Setup, 1.2, 1.4, 1.7, 1.8. The existing `langchain-deep-agents-langsmith-ext` reference
    track has ZERO precedent for this (only a track-icon asset, no in-body images) — the
    image-embedding convention must come from `langchain-langgraph-langsmith-ext` instead
    (pending that agent's report).
  - **Model config in source:** notebook imports a prebuilt `model` from `utils/models.py`
    (repo root). As-written/uncommented, it's OpenAI direct (`openai:gpt-5.6-luna`) — not
    what we run. A commented Bedrock block exists: `bedrock_converse:anthropic.claude-sonnet-4-20250514-v1:0`,
    and critically requires dropping the `api_key=os.environ[...]` kwarg entirely (Bedrock
    auths via AWS credential chain, not a key) — use the already-proven `ChatBedrockConverse`
    wiring from `langchain-deep-agents-langsmith-ext/utils/models.py` instead of this block
    (different model id pin: that track uses `us.anthropic.claude-sonnet-4-6`, cross-region
    inference profile — prefer that over the notebook's hardcoded `claude-sonnet-4-20250514-v1:0`).
  - **Tavily fallback behavior worth knowing:** `resilient_tavily_search` (in `utils/search.py`)
    retries on failure/empty results with linear backoff, then degrades to a topic-matched
    canned response prefixed `[fallback content; live search unavailable...]` rather than
    crashing. If we keep Tavily (see AgentCore recommendation above), a check verifying "web
    search was actually invoked" should distinguish a real Tavily call from this graceful
    fallback firing silently — a LangSmith trace check (per user's stated check design) would
    naturally do this if it asserts a genuine external HTTP span, not just that the tool node
    ran.
  - **⚠️ Design fork to resolve before building (see Open Questions): the source notebook's
    `create_deep_agent()` calls do NOT monotonically accumulate kwargs section-to-section.**
    1.1→1.2→1.3→1.4 do chain (`model,system_prompt,checkpointer` → `+tools` → `+subagents` →
    `+backend,+store`), but 1.5 (`agent_with_middleware`) and 1.6 (`agent_with_hitl`) are each
    a **fresh, separately-named agent variable** that only adds `middleware` / `interrupt_on`
    on top of the *1.3* state — they do NOT carry forward 1.4's `backend`/`store`. 1.7 is
    another fresh `agent` with `subagents`+`memory`+`skills` but no backend/middleware/HITL.
    Only 1.8 (`complete_agent`) combines literally everything (10 kwargs total). This is a
    real conflict with the user's stated requirement that "every new line item from the
    previous section should be commented out so the user uncomments it to continue" — that
    requirement only makes sense if the track's `create_deep_agent` cell accumulates
    continuously across all 6 challenges, which means **deliberately deviating from the
    source notebook's section-local branching** in favor of one continuously-growing agent
    that reaches 1.8's exact end state by the final challenge. Recommend: linearize (one
    accumulating agent), since it's what the user's structural requirement implies and what
    section 1.8 already shows as the intended end state — flagged to user for confirmation,
    not yet decided.

- **`langchain-langgraph-langsmith-ext` infra deep-dive (2026-09-30):** lives on git branch
  `adlc-101-slice-0` (not `main` — read via `git show adlc-101-slice-0:<path>`, never checked
  out). `config.yml`: single `workstation` VM (Ubuntu 24.04, 8GB/2cpu), one `aws_accounts`
  entry scoped to `bedrock` only, **no `secrets:` block at all** — deliberate: this track
  switched to **learner-supplied getpass credentials** (LangSmith + Tavily keys typed in
  during challenge 1, via a `lab_credentials.py` helper) instead of shared platform secrets,
  specifically so each learner's traces land in their own workspace. This repo's own
  MEMORY.md convention ("getpass credential decision") confirms this was a deliberate
  reversal from an earlier shared-secret design. `track.yml` confirms no invented
  `id`/`checksum` on a fresh scaffold. `ChatBedrockConverse` wiring identical to the
  deep-agents-langsmith-ext track's (`us.anthropic.claude-sonnet-4-6` / haiku judge model,
  region from env). `gen_notebooks.py` heredoc (setup-workstation lines ~886-2029, delimiter
  `'GENEOF'`): `md()`/`code()`/`write()` helpers, real confirmed escaping examples — literal
  `\n` in a notebook cell's own source becomes `\\n` in the generator's Python string, and
  any cell containing a `"""`-delimited string switches that `code(...)` call to `'''` outer
  quotes (repeats at every such cell). **Image convention: images embedded in notebook
  markdown are NOT copied into the track's own `assets/` — they're read straight from the
  pinned upstream `modular-workshops` clone's own `images/` dir** (`../images/foo.png`
  relative path, verified present by a `test -s` assertion in setup-workstation). track-level
  `assets/` holds only `langchain-icon.png` for `track.yml`'s `icon:` field.
  **Critically: zero images appear in any `assignment.md` or splash card in this track** —
  splash cards are pure styled HTML/text (`card.yml` + shared `card_template.html`, rendered
  by repo-root `scripts/cards.py`), no `<img>` anywhere. **This means there is no existing
  in-repo precedent for "image inside assignment.md" — our track's requirement to surface
  notebook images in the Instruqt challenge too is novel and needs verification of how
  Instruqt actually serves an inline image in assignment.md body (likely needs the PNG
  copied into the track's own `assets/` and referenced by an Instruqt-servable asset path,
  not a sandbox-relative path) — flagged as a build-phase verify item, not yet confirmed
  against Instruqt docs.** Check pattern (`check_supervisor.py` example): numbered exit
  codes per failure mode, `select=[...]` field allow-lists on `list_runs`, `is_root=True` +
  `limit=50` for root-run search then a second unlimited/paginated fetch scoped to one
  `trace_id` for the nested-span assertion, never prints raw exception text, treats
  `LangSmithNotFoundError` as sys.exit(2) "nothing traced yet" rather than a hard error.
  `MEMORY.md` skeleton: `# MEMORY — <slug>` header, then `## Design decisions` (table),
  `## Open questions`, `## Architecture notes`, `## Runtime-verify items`, `## Session history`,
  `## Deferred work`, dated `##` entries appended per session — strictly append-only, later
  entries flag reversals rather than editing old ones.

## Design Decisions (resolved, pending final user confirmation on web-search tool)
- **Credential model: reuse the langgraph track's getpass pattern** (`lab_credentials.py` —
  learner types in their own LangSmith + Tavily keys in challenge 1; tested via this repo's
  `SOLVE_*` env-var override in `track-test.sh`), NOT the older shared-`secrets:`-block
  pattern from `langchain-deep-agents-langsmith-ext`. Rationale: it's the more recent,
  deliberately-chosen convention in this repo (a documented reversal away from shared
  secrets), and it sidesteps provisioning any new shared credential for this track.
  **This also weighs directly on the web-search-tool decision below** — since getpass
  already solves "learner needs their own free key, nothing shared to provision," the
  original motivation for avoiding Tavily (wanting to dodge key management) is largely
  already solved by convention, independent of whether AgentCore is used.
- **Web search tool: Bedrock AgentCore Web Search first choice, Tavily+getpass fail-fast
  fallback (decided 2026-09-30).** User wants to attempt AgentCore (genuinely keyless on
  Bedrock) but fail fast — if the Gateway spike (slice 0) shows it's broken, blocked, or not
  feasible within this repo's per-sandbox provisioning flow, drop it immediately and build
  section 2 on Tavily + learner getpass instead (the originally-recommended, proven path).
  No sunk-cost attempt to force AgentCore to work — the spike is a go/no-go gate, not a
  debugging commitment. If it falls back to Tavily, Setup (challenge 1) needs a Tavily getpass
  prompt added back in (it was dropped from the plan on the assumption AgentCore would
  eliminate that need).
- **Branch base:** kept `deep-agents-workshop` cut from `main` rather than rebasing onto
  `adlc-101-slice-0` (that branch is another in-flight task's work — reading its files via
  `git show` is enough for a read-only reference, no need to entangle history).

- Title confirmed 2026-09-30: "Deep Agents 201 — From `create_agent` to a Complete Deep
  Agent" (polished version, not the literal `[AH] Create a Deep Agent Workshop` draft label).

- **Instruqt `iam_policy:` field + AgentCore IAM actions (researched 2026-09-30):**
  Instruqt's `config.yml` `aws_accounts` supports an inline custom IAM policy via
  `iam_policy: |` (JSON document), usable alongside `managed_policies:` — confirmed in
  Instruqt's own docs (`docs.instruqt.com/sandboxes/cloud-accounts/aws-accounts/aws-iam-policies`).
  AWS's own managed policy `BedrockAgentCoreFullAccess` covers Gateway/target CRUD +
  `InvokeGateway`/`InvokeWebSearch`, but explicitly excludes `iam:CreateRole` and scopes
  `iam:PassRole` to `*BedrockAgentCore*`-named roles — insufficient alone since the sandbox
  must create the Gateway's own execution role at setup time. Used a custom `iam_policy:`
  instead (see `config.yml`) granting the full Gateway/target CRUD set +
  `InvokeGateway`/`InvokeWebSearch` + `iam:CreateRole,PutRolePolicy,GetRole,PassRole`,
  Resource `*` (fine for a throwaway spike, would scope down for the real track).

## Slice 0 — AgentCore spike VERDICT: NO-GO (2026-09-30)
Ran the spike end-to-end in a real throwaway Instruqt sandbox (`instruqt track test`,
track id `rdizg7akavw3`, participant `g2lgketj3np6`). Result: **blocked by an explicit deny
in an AWS Organizations Service Control Policy** on the `iam:GetRole` call needed to create
the Gateway's execution role:
```
AccessDenied ... User: arn:aws:iam::048088571984:user/apjg9ytihl6d7xe1 is not authorized to
perform: iam:GetRole on resource: role deep-agents-workshop-spike-gateway-role with an
explicit deny in a service control policy: arn:aws:organizations::259...
```
An SCP deny overrides any IAM identity policy (including our custom `iam_policy:` in
config.yml) — this is an org-level guardrail on the `langchain` team's AWS org, not a
permissions gap fixable from track config. **Per the user's explicit fail-fast instruction,
immediately pivoting to Tavily + learner getpass** (the originally-recommended path) — no
further attempt to work around the SCP. No AgentCore resources were actually created (failed
before Gateway creation), so nothing to clean up on the AWS side. The sandbox itself
(`--keep-running`) will self-reclaim via `idle_timeout: 900` — no CLI command exists to force-
stop it early (checked `instruqt track test/sandbox --help`), so no action needed there.
Design doc `docs/2026-09-30-build-plan.md` and the challenge/infra plan both need updating to
drop AgentCore entirely and build section 2/challenge 3 (`custom-tools-and-subagents`) on
Tavily instead, with a Tavily getpass prompt added to Setup (challenge 01).

## Slice 0 — AgentCore spike (historical — superseded by verdict above)
- Scaffolded a minimal, throwaway `track.yml`/`config.yml`/single challenge
  (`01-agentcore-spike/`) + `track_scripts/setup-workstation` (SPIKE VERSION — no
  JupyterLab/notebooks, just: bootstrap wait, uv venv with
  `boto3 langchain-aws bedrock-agentcore`, resolve Bedrock AWS creds, run a guarded
  Python script that attempts Gateway create + `create_web_search_toolkit` + one
  invocation, writes PASS/FAIL to `/opt/instruqt-workshop/agentcore-spike-result.txt`).
  Also vendored `scripts/track-test.sh`, `cards.py`, `preview.py` onto this branch
  (current-content copy, not a cherry-pick — see Session History) since they only
  existed on the unmerged `adlc-101-slice-0` branch and `main` doesn't have them.
- This whole scaffold is throwaway and will be replaced/adjusted once the spike verdict
  is in — do not treat `01-agentcore-spike/` as real track content.

## Slice 1 — real track scaffold + challenges 1-2 (built 2026-09-30)
Built for real, replacing the spike scaffold entirely:
- `track.yml`/`config.yml`: dropped AgentCore, back to plain `AmazonBedrockLimitedAccess` +
  no `secrets:` block (learner getpass, matching langgraph track). `LANGSMITH_PROJECT_BASE:
  instruqt-deep-agents`. `WORKSHOP_COMMIT` reuses the same pin as the sibling tracks
  (`06a934f270730f7af4cbf4bd1fb7320fd9fce718`) — verified directly (not assumed) that this
  commit has `images/deepAgentsDiag.png` and `deepagents>=0.7`/`langchain-aws>=1.0.0` in
  `pyproject.toml`.
- `track_scripts/setup-workstation` built by transforming the langgraph track's reference
  script (via `git show adlc-101-slice-0:...`): kept bootstrap/packages/identity/clone/uv/
  credentials/model-wiring/JupyterLab+nginx/warmup verbatim; dropped all langgraph-curriculum
  content (challenge-3 prompt/research-agent block, challenges 2/4/5/6 check+fill helpers);
  replaced `gen_notebooks.py`'s `write()` calls with our own `01_setup.ipynb` +
  `02_your_first_deep_agent.ipynb`; extended `lab_state.py` with a new `extract_file()` helper
  (pulls a virtual file out of a deep agent result onto real disk, since `create_deep_agent`'s
  filesystem tools are in-memory `StateBackend` only — the check can't read agent state
  directly). Added `fill_challenge2_blank.py` (same by-cell-tag pattern as the sibling's
  challenge 4) since challenge 2 has one blanked cell.
  **Caught and fixed 3 bugs during the build, logged so they're not silently lost:**
  (1) two deletion passes over the reference script used substring matching on heredoc
  delimiters, which matched the OPENING `<<'XEOF'` line instead of the bare closing line,
  corrupting the file (stray unquoted Python inside bash) — fixed with exact bare-line
  matching; (2) missed a whole langgraph-specific block (challenge 3's prompt/research-agent
  content, ~330 lines) sitting between the orientation-notebook comment and `lab_state.py`,
  outside my first deletion range — found via `bash -n` catching the corruption downstream
  and re-traced; (3) an early notebook draft extracted `/haiku.txt` from the wrong `result`
  variable (the new-thread/empty one, reassigned by a later upstream cell) rather than the
  same-thread one — fixed by placing the extraction cell immediately after the same-thread
  re-read, before the new-thread demo cell reassigns `result`.
- **Design correction from the original plan:** the user's original spec was under-specified
  on HOW `create_agent` failing should be demonstrated. Corrected to: challenge 2 opens with a
  pre-filled `create_agent()` cell that runs the haiku task and prints
  `files in result: None` (no filesystem), immediately followed by upstream's real 1.1
  markdown (image + bullets), then ONE blanked cell (tag
  `instruqt-blank-first-deep-agent`) where the learner pastes the exact `create_deep_agent`
  code given in the assignment instructions — matching this repo's established
  blank-cell-by-tag convention (`langchain-langgraph-langsmith-ext`'s challenge 4), not
  something invented fresh. This means **the Setup cell uses an absolute path
  (`Path("/root/workshop")`)**, not upstream's cwd-relative `Path().resolve().parent` —
  required because `solve-workstation` fills the blank into a `/tmp` copy and nbconvert
  runs the kernel in that copy's own directory (same rationale already documented in the
  sibling track for its own blanked-cell challenge).
- Verified end-to-end locally before pushing: extracted and `ast.parse`'d every code cell of
  both generated notebooks (including after running the real `gen_notebooks.py` and the real
  `fill_challenge2_blank.py` against a temp copy), `bash -n` on every script, YAML-parsed
  every frontmatter/config file, confirmed the blanked cell's tag survives generation and the
  fill produces syntactically valid, logically-ordered code.
- Reused verbatim from `langchain-langgraph-langsmith-ext` (git branch `adlc-101-slice-0`):
  `card_template.html`, the `cards.py`-based splash-card workflow (note: `--track` defaults to
  the langgraph track's slug — always pass `--track langchain-deep-agents-workshop-ext`
  explicitly), `cleanup-workstation` verbatim (same getpass credential model so the same
  "don't touch LangSmith, learner owns it" rationale applies unchanged), challenge 1's
  check/solve-workstation structure, challenge 2's fill-by-tag solve pattern.
- **First real-sandbox test run (2026-09-30): challenge 1 fully green (setup -> check fail
  -> solve -> check pass). Challenge 2's solve FAILED**: `ModuleNotFoundError: No module
  named 'lab_state'` on the `extract_file` cell, when nbconvert executed the filled notebook
  from `/tmp` (per `cd /tmp` in solve-workstation). Root cause: the Setup cell only inserted
  `/root/workshop` into `sys.path`, not `/root/workshop/instruqt` where `lab_state.py`
  actually lives — normal learner execution never hits this (Jupyter's kernel cwd defaults to
  the notebook's own directory, so `/opt/workshop/instruqt` is implicitly importable), but
  nbconvert-from-/tmp has no such implicit cwd. This is exactly why the sibling track's
  challenge 4 inserts BOTH `/root/workshop` and `/root/workshop/instruqt` — I'd copied that
  challenge's absolute-path rationale but missed the second path. Fixed: challenge 2's Setup
  cell now inserts both paths, matching challenge 4's exact pattern. Re-generated and
  `ast.parse`'d both notebooks again locally before re-pushing and re-running the test.
- **Re-test after the fix (2026-09-30): `Track test succeeded` — both challenges fully green**
  (setup -> check-fail -> solve -> check-pass, for both `setup` and
  `your-first-deep-agent`). Slice 1 is done and proven against a real sandbox, not just
  locally validated. Ready to preview in the Instruqt UI.
- **User reviewed the live preview (2026-09-30) and gave two fixes, both applied and
  re-pushed:** (1) removed challenge 1's "Step 4: Let the platform prove it" paragraph from
  `01-setup/assignment.md` — redundant since Steps 2-3 already demonstrate the verification;
  `check-workstation` is unchanged, it still runs the same checks behind the scenes, only the
  explicit "now click Check" instructional text was cut. (2) standing rule going forward:
  **every `create_agent(...)`/`create_deep_agent(...)` call must be broken across multiple
  lines** (one kwarg per line), never a single-line call — fixed the one offending instance
  (challenge 2's plain `create_agent` demo). Re-validated (syntax + notebook generation +
  fill-script) after both fixes; skipped a full sandbox re-test since both changes are
  non-functional (prose cut, code reformatted with identical AST) — flagged this judgment call
  rather than silently skipping.

## Slice 2 — challenge 3: Custom Tools & Subagents (built 2026-09-30)
- Back on Tavily (per the slice-0 AgentCore no-go): reused upstream's exact `tavily_search`
  (wrapping `resilient_tavily_search` from `utils/search.py`) and `research_subagent` dict
  verbatim, split into their own cells per the "own cell, not mixed with other code" rule.
- Same uncomment-by-tag mechanic as challenge 2's paste-in-blank, but simpler: the
  `create_deep_agent` cell is pre-filled with the full accumulated call from challenge 2,
  with `# tools=[tavily_search],` and `# subagents=[research_subagent],` commented out —
  tag `instruqt-uncomment-tools-subagents`, filled by `fill_challenge3_blank.py` (same
  by-tag-replace pattern as challenge 2's filler).
- Check (`check_custom_tools_subagents.py`) verifies a real, non-fallback `tavily_search`
  tool span exists anywhere in the named trace (`run_name="custom-tools-and-subagents"`) —
  deliberately NOT requiring it be nested under the subagent's `task()` delegation (unlike
  the langgraph sibling's stricter supervisor check), since the user's own spec was "the
  check can be that the web search tool was used," and requiring nesting would make the
  check brittle to legitimate model behavior variance (the model may search directly instead
  of delegating, since both `tools=` and `subagents=` are active simultaneously here).
  Verified the `task()` delegation tool's real LangSmith trace shape by reading
  `deepagents`' own source (`middleware/subagents.py`) rather than assuming — confirmed it's
  a `StructuredTool` literally named `"task"` that invokes the subagent's compiled graph
  inline, so a real delegation nests as expected if the model chooses to delegate.
- **Caught 3 more cross-notebook-scope bugs before ever running the real sandbox test** (each
  notebook is a separate kernel, so nothing carries over from a prior challenge's notebook):
  (1) `print_files` was only defined in challenge 2's notebook — redefined it in challenge 3
  too (kept as a visible notebook cell, not hidden behind an import, since upstream presents
  it as notebook content); (2) `from deepagents import create_deep_agent` was never imported
  in challenge 3 (it was buried inside challenge 2's blanked-cell solution) — added to
  challenge 3's Setup cell; (3) invented a nonexistent "Terminal tab + verify-X command" step
  for challenge 3 by pattern-matching challenge 1 too eagerly — the actual sibling-track
  precedent for a live-trace check (challenge 5, supervisor-and-subagent) has no Terminal tab
  at all, just "click Check"; fixed to match.
- **Images requirement resolved:** researched and confirmed Instruqt's actual mechanism —
  plain markdown `![alt](../assets/file.png)`, track-root `assets/` folder, uploaded and
  rewritten to a fully-qualified URL on `instruqt track push` (confirmed via
  `docs.instruqt.com/reference/cli/assets` + real public Instruqt track repos on GitHub, not
  guessed). Copied `deepAgentsDiag.png` and `deepAgentSubagents.png` from the pinned upstream
  commit into `assets/`. **Retrofitted challenge 2's assignment.md**, which had shipped
  without its image — this was a real gap from slice 1, now closed.
- **Real sandbox test (2026-09-30): `Track test succeeded` on the first run** — all three
  challenges fully green (setup -> check-fail -> solve -> check-pass, each). No bugs this
  round; the local static validation (syntax, notebook generation, fill-script simulation)
  caught everything before push this time.

## Slice 3 — challenge 4: Backends & Memory (built 2026-09-30)
- Split upstream cell 19 into its planned two cells (DB/backend definition, then
  `create_deep_agent` alone) plus separate thread-1/thread-2 invoke cells, per the
  "own cell, not mixed with other code" rule.
- Two authoring tasks in one blanked/tagged cell (`instruqt-fill-backend-memory`): uncomment
  `backend=backend,`/`store=store,`, AND replace the `system_prompt` value with the exact
  text given in the assignment (a genuine paste-in, not just an uncomment) — matches the
  user's explicit request to have the learner copy a prompt from the instructions.
- **Verified the real persistence key/namespace scheme from `deepagents`' own source**
  (`backends/store.py`, `backends/composite.py`) rather than assuming: `CompositeBackend`
  strips the `/memories/` prefix before delegating, so `StoreBackend.write()` receives e.g.
  `/preferences.md` as the key, stored via `store.put(("memories","shared"), key, {"content":
  ..., "encoding": ...})`. The check (`check_backends_memory.py`) opens the same SQLite file
  directly with `SqliteStore` and searches the `("memories","shared")` namespace for any item
  whose content mentions the saved fact — reading the actual persistence layer, not trusting
  a LangSmith trace or the model's own claim.
- **Caught the DB-path bug before ever running it**, by reasoning forward from the earlier
  `sys.path` bug rather than waiting to hit it again: upstream's `MEMORY_DB = "deep_agents_memory.db"`
  is cwd-relative, which would resolve to `/tmp` (wrong) when solve executes from there.
  Made it absolute (`/root/workshop/instruqt/deep_agents_memory.db`) and updated the Setup
  cell's cleanup glob to match. Also added a `mark_done` sentinel to the notebook's last
  cell — initially forgot it (same class of miss as challenge 3), caught in local validation
  this time, not in a failed sandbox run.
- **Real sandbox test (2026-09-30): `Track test succeeded` on the first run** — all four
  challenges fully green. Second slice in a row with no bugs surfacing only at sandbox time;
  reasoning forward from prior bugs (absolute paths, sentinels, cross-notebook scope) during
  local validation is catching them earlier.

## Slice 4 — challenge 5: Middleware & HITL (built 2026-09-30)
- Combines upstream 1.5 (middleware) + 1.6 (HITL) into one accumulating agent, same
  linearization approach as challenges 2-4: one blanked cell
  (`instruqt-uncomment-middleware-hitl`) with `middleware=[compliance_rules, audit_trail],`
  and `interrupt_on={"write_file": True, "edit_file": True},` both commented out.
- 5 images needed for this single challenge (`deepAgentMiddleware.png`, 3 context-management
  diagrams, `deepAgentHITL.png`) — copied from the pinned commit; renamed the two upstream
  filenames that contain spaces (`Offloading Inputs LangChain.png` ->
  `offloading-inputs.png`, etc.) for our track's own `assets/` copies used in assignment.md,
  while the NOTEBOOK markdown keeps the original spaced filenames verbatim (`<img
  src="../images/Offloading Inputs LangChain.png">`) since that's what actually exists in the
  pinned upstream repo's own `images/` dir — two independent asset pipelines, not a
  contradiction.
- Check design (`check_middleware_hitl.py`) reads two real files an Instruqt-added final cell
  writes: `/test.md` extracted to disk (proves the approved write really executed) and
  `audit_log` dumped to JSON (proves `audit_trail` middleware really wrapped it) — no
  LangSmith dependency needed, pure local file state, consistent with challenge 4's
  direct-persistence-layer approach.
  **Known, disclosed limitation:** this check cannot strictly distinguish "HITL was
  exercised" from "only middleware was uncommented, so write_file just ran immediately" —
  both produce the same two files. A stricter check would need to detect the actual
  pause-then-resume, e.g. via a LangSmith trace showing an interrupted/resumed run. Accepted
  as good-enough given no explicit check spec was given for this challenge and the primary
  failure mode (nothing uncommented at all) is still caught correctly.
- HITL resume cell uses the general list-based decision form (`{"type": "approve"} for _ in
  pending`) rather than upstream section 1.6's own single-decision shorthand — matches what
  upstream's own section 1.8 "complete agent" already does, a deliberate adoption of
  upstream's more-correct later pattern rather than a fidelity break.
- Reused the now-established absolute-path fixes (sys.path two-path pattern, absolute
  `MEMORY_DB`) from the start this time — no bugs caught in local validation this round,
  everything reused correctly on the first pass.
- **First real sandbox test (2026-09-30): challenges 1-4 green, challenge 5's solve FAILED**:
  `AttributeError: 'compliance_rules' object has no attribute '__name__'` — my own added
  debug print (`print("middleware defined:", compliance_rules.__name__, audit_trail.__name__)`)
  wrongly assumed `@wrap_model_call`/`@wrap_tool_call` return plain functions; they return
  middleware instances with no `__name__`. Not upstream content — my own addition, and the
  only cell in this challenge that wasn't copied from a verified source. Fixed by dropping the
  introspection (`print("middleware defined: compliance_rules, audit_trail")`, static text).
  Lesson: the bugs so far have consistently been in content *I* wrote (sentinels, debug
  prints, path assumptions), never in the verbatim-upstream cells — worth extra scrutiny on
  exactly those non-upstream additions before pushing, not just on the mechanically-adapted
  scaffolding. (Also saw an unrelated, self-healing "Bedrock credential preflight" cold-start
  retry in the log — not a bug, expected AWS access-enablement latency already handled by the
  existing warmup-retry logic inherited from the sibling track.)
  Re-validated locally, re-pushed, re-tested.
- **Re-test (2026-09-30): `Track test succeeded`** — all five challenges fully green.

## Slice 5 — challenge 6: Agents & Skills (built 2026-09-30)
- Upstream's cell 27 (1.7) is a FRESH agent that drops backend/store/middleware/interrupt_on
  entirely, unlike our accumulated design. Kept the linearization going anyway (consistent
  with every prior challenge): this challenge's blanked/tagged cell (
  `instruqt-uncomment-agents-skills`) both COMMENTS OUT `system_prompt=(...)` (redundant once
  `AGENTS.md` is active — upstream's own stated reason) AND uncomments `memory=["/AGENTS.md"]`
  + `skills=["/skills/"]` — the first challenge where the authoring task is a comment-out AND
  an uncomment together, not just one or the other. Verified `system_prompt` +
  `memory=["/AGENTS.md"]` together would NOT actually error (checked `deepagents`' own
  `graph.py` — they'd just concatenate) before deciding this was a stylistic/pedagogical
  choice to make explicit, not a technical requirement.
- Since `interrupt_on` stays active from challenge 5's accumulation and `AGENTS.md`'s own
  workflow text says "Save to /final_report.md," the run can still pause for HITL approval —
  reused the same guarded `if result.get("__interrupt__"): ... approve` pattern from
  challenge 5 defensively, folded into one cell this time since HITL isn't this challenge's
  teaching focus.
- Fixed a real upstream inconsistency rather than reproducing it: cell 27's own
  `config = {"configurable": {"thread_id": uuid7()}}` omits `str(...)` around `uuid7()`
  (every other cell in the whole notebook wraps it) — used `str(uuid7())` for consistency
  and to avoid an unproven risk of a raw UUID object breaking checkpointer/store serialization.
- Check design reuses upstream's own pattern from section 1.8 (noted in earlier research:
  "filters out the two seed paths" when printing files) rather than inventing something new:
  the final cell extracts every file in `result["files"]` except the two seeded paths
  (`/AGENTS.md`, `/skills/linkedin-post/SKILL.md`) onto real disk; the check is a plain bash
  `find` for any non-empty file in that directory — no dedicated Python check script needed,
  simplest check in the track so far.
- **First real sandbox test (2026-09-30): challenges 1-5 green, challenge 6's check FAILED**
  (not an error — `nbconvert` completed cleanly, but no output file existed). Root cause:
  exactly the risk flagged during design — the model answered the LinkedIn-post request
  conversationally without calling `write_file`, since the literal user prompt never asked
  for a file, only `AGENTS.md`'s buried workflow step did, and the model didn't reliably
  follow it. Fixed by making the prompt explicit: "...write a LinkedIn post about it **and
  save it to /final_report.md**...". This is a deliberate, disclosed deviation from upstream's
  exact prompt wording — upstream's own demo had no automated check depending on file output,
  so it could afford that ambiguity; ours can't. Re-validated locally, re-pushing before
  re-testing.

## Slice 6 — challenge 7: The Complete Agent (built 2026-09-30) — FINAL CHALLENGE
- Combines every kwarg from challenges 2-6 with nothing commented out anymore — the
  accumulation's actual endpoint. Upstream's cell 30 is unusually deterministic for a change
  (explicit file paths: "write the final report to /final_report.md, and save key takeaways
  to /memories/research_notes.md") — avoids the exact ambiguity bug just fixed in challenge 6.
  Reused verbatim.
- The one blank: the HITL approval **while**-loop (upstream's own more-general pattern,
  already adopted once before in challenge 5's resume cell) — this task makes two paused
  writes (report + memory), so a single `if` (challenge 5's version) isn't enough. This is
  the most substantial "write real logic yourself" blank in the track, scaffolded with an
  explicit comment-block showing the shape (matching the sibling track's own house style for
  a non-trivial blank).
- Check (`check_complete_agent.py`) reads all three persistence surfaces directly and
  separately: extracted `/final_report.md` (disk), `write_file` entry (audit log JSON), and
  an exact-key SQLite store lookup for `/research_notes.md` (reusing challenge 4's verified
  key-stripping scheme) — a genuine "everything worked together" assertion, not a single
  proxy signal.
- **Caught two real bugs in local validation before ever touching a sandbox** (not from a
  failed test this time — the user asked to skip the automated full-track test for this final
  slice and run it manually themselves instead): (1) `agents_md`/`linkedin_skill` use
  unescaped `"""` inside a Setup cell whose own outer delimiter was also `"""`, terminating
  it early and corrupting everything after — fixed by escaping, matching the pattern already
  used elsewhere in the same cell for the `tavily_search` docstring; (2) an f-string
  expression (`entry[\'timestamp\']`) had backslash-escaped single quotes that don't need
  escaping at all inside a `'''`-delimited generator string, and are flatly invalid inside an
  f-string's `{}` part — removed the unnecessary escaping. Both caught by running
  `gen_notebooks.py` locally and `ast.parse`-ing every generated cell, the same validation
  loop used throughout the track.
- **Per user's explicit request: stopped running `instruqt track test` automatically after
  this slice.** Pushed the track; the user will run the full end-to-end test themselves in
  the Instruqt UI. Track has 7 challenges total, matching the build plan exactly.

## Post-slice-6 correction: removed the Terminal tab entirely (2026-09-30)
- User flagged that Challenge 1 still had a Terminal tab + `verify-connection` step, which
  they'd intended to be covered by the earlier "remove the redundant step" feedback (that
  feedback had actually only removed the closing "click Check" paragraph — a real
  misunderstanding on my part vs. their intent, not something they'd re-requested from
  scratch). Clarified via question: user wants **zero terminal actions anywhere** in the
  track — either drop the verification or fold it into the notebook.
- **Kept the verification's actual value** (an authenticated LangSmith API call from a fresh
  process, proving the key persisted to `.env` correctly — not just that it's sitting in the
  kernel's in-memory `os.environ`, which the in-notebook model-call cell alone wouldn't
  necessarily catch if tracing failed silently in the background) **but moved it into a
  notebook cell**: `subprocess.run([sys.executable, "/opt/instruqt-workshop/verify_connection.py"],
  check=True)`. `verify_connection.py` itself, `check-workstation`'s `[Check 5/5]`, and
  `solve-workstation`'s call to the same `/usr/local/bin/verify-connection` wrapper are all
  unchanged — only how the learner triggers it changed (notebook cell, not Terminal tab).
- Removed the Terminal tab from `01-setup/assignment.md` (confirmed via grep it was the
  *only* challenge using one), rewrote Step 3's instructions and wording throughout
  (`assignment.md`, `card.yml`, `check-workstation`'s fail messages, `solve-workstation`'s
  comments) to stop referencing a terminal. Caught one stray leftover reference ("scroll past
  on the way to the Terminal tab") in Step 2's callout during a final grep sweep.
- **Runtime-verify item, not yet confirmed:** `subprocess.run` output display inside a Jupyter
  cell — reasoned through (Jupyter typically redirects the kernel's OS-level stdout FD, which
  child processes inherit, so `verify_connection.py`'s prints should appear in the cell
  output) but not empirically tested. Per the user's standing instruction to skip automated
  testing for now, this will surface during their manual end-to-end run if wrong.

## Tone pass on challenge 1, per `/notebook-tone` (2026-09-30)
User gave 5 specific lines in `01-setup/assignment.md` violating notebook-tone rules (hype
words "can't even"/"fully-featured", a stock from/to AI arc, dramatic "proves", stacked
reassurance sentences, narrative Result lines, an em-dash teaser closer) with exact
replacement text for each — applied verbatim. Scoped to challenge 1 only, as asked; the same
patterns (closer line "✅ ... In **Challenge N** you'll...", narrative "Result:" phrasing)
likely recur in challenges 2-7 since I wrote them with the same instincts — flagged to user,
not yet addressed elsewhere pending their call on scope.

## Checks moved from disk extraction to traces (2026-10-01)
- **Supersedes** the extraction-cell design in slices 1, 4, 5 and 6 above. The Ch2/5/6/7
  "copy to disk" cells, `lab_state.extract_file()`, every per-challenge `/tmp/.*-ok`
  sentinel (Ch2-7) and `check_middleware_hitl.py` are gone. Ch1 keeps its sentinels.
- New shared `check_trace.py` (OPS_DIR): newest successful `write_file` tool run to
  `--path` in the learner's project; `--after-resume` requires its trace root input to
  be `Command(resume=...)` (proves HITL); `--prompt-contains` requires text in the trace's
  llm-run inputs (proves `middleware=` via "## Compliance Rules", `memory=` via
  "# Research Assistant", `skills=` via "linkedin-post"). Verified locally that deep-agent
  traces carry these (fake model + `collect_runs`); not yet verified against live LangSmith.
- `audit_trail` is not checkable from the trace (it only appends to a Python list); it is
  uncommented on the same line as `compliance_rules`, so the compliance marker covers it.
- Ch7 still reads `/memories/research_notes.md` from SQLite (`check_complete_agent.py`,
  now SQLite-only). Ch4 unchanged (SQLite).
- Consequence: Ch2/5/6/7 Checks now need a real LangSmith key, same as Ch3 already did;
  `instruqt track test` needs `SOLVE_LANGSMITH_API_KEY` for them to pass.

## Splash cards are hand-edited HTML (2026-10-02)
- Removed the 7 `card.yml` files and `card_template.html`. Splash cards now live only as HTML
  in each `assignment.md` `notes:` block, same as `langchain-deep-agents-langsmith-ext`; edit
  wording there directly and keep the `<style>` block intact. Entries above that mention
  `card.yml` / `scripts/cards.py` / `preview.py` describe the earlier workflow.

## Open Questions
- Exact section headings/numbering in `01_deep_agents.ipynb` (verbatim) — research pending.
- Is Bedrock AgentCore web search a real, keyless, LangChain-callable tool, or should the
  track fall back to shared-Tavily-key infra (this repo already has `SOLVE_*` key injection
  via gitignored root `.env` for exactly this kind of shared credential) — research pending.
- Exact reusable infra from `langchain-langgraph-langsmith-ext` (config.yml topology,
  gen_notebooks.py heredoc pattern, check-against-LangSmith-API retry pattern, image
  embedding convention) — research pending.
- Whether `langchain-deep-agents-langsmith-ext` (existing, different-curriculum track) has
  directly reusable Bedrock model config / create_deep_agent invocation code — research
  pending.
- Middleware & HITL (section 4), Agents & Skills (section 5), and complete agent (section 6)
  content/checks not yet discussed in detail with the user — deferred to design conversation
  once sections 1-3 are locked in, given the user's iterative build preference.

## Session History
- 2026-09-30: Skill launched. Grounding complete (repo root confirmed, CLAUDE.md read).
  User chose working folder name `langchain-deep-agents-workshop-ext` and approved running
  the cross-track reuse scan. Branch `deep-agents-workshop` cut from `main`. Working folder
  and this MEMORY.md created. Four research agents launched in parallel: (1) langgraph track
  infra deep-dive, (2) deep-agents reference track deep-dive, (3) source notebook structure
  parse, (4) Bedrock AgentCore web search viability research.

## Deferred Work
- **LangSmith SDK migration, due before 2027-01-31** (logged 2026-10-01). `Client.list_runs()`
  and `Client.read_run()` are deprecated (cloud deprecation from end of July 2026, removal
  31 Jan 2027). Both `check_trace.py` and `check_custom_tools_subagents.py` depend on them.
  The replacement `client.runs.query()` / `client.runs.retrieve()` is async-only, needs
  `project_ids` (UUID, via `aread_project`), uppercase `selects`, `min_start_time`, and
  langsmith >=0.10.15 (sandbox pins 0.10.15 via modular-workshops @ WORKSHOP_COMMIT).
  Open: confirm `runs.query` supports the name filter (`write_file`) and trace-id lookup
  the Checks use; also check whether `read_project()` (used by `lab_trace.py`) is deprecated.
  Re-run the live LangSmith test after migrating.
  Guide: https://docs.langchain.com/langsmith/smithdb-sdk-migration-query-runs
