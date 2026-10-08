---
name: instruqt-track-builder-reference
description: >-
  Build a new Instruqt track from idea through ready-to-push. Use when the user says
  "build a new track," "scaffold a new track," "create a new Instruqt track," "start a
  new track build," "let's design a track," or any similar request to begin a new
  Instruqt track from scratch. Drives a phased flow with explicit approval gates:
  grounding, discovery (with optional cross-track reuse scan and customer-branding
  extraction), build plan, build, audit, push, and knowledge review. Does NOT cover
  edits to existing tracks.
---

# Instruqt Track Builder (Reference Edition)

This skill guides the end-to-end creation of new Instruqt tracks within the user's track repository. The repository — not this skill — is the long-term source of truth: repository-specific knowledge lives in the repository's `CLAUDE.md`, per-track history lives in each track's `MEMORY.md`, and this skill remains stateless.

**Scope:** Create new tracks only. Existing tracks may be inspected as references but must never be modified.

## Operating Principles

- **The repository's `CLAUDE.md` is the primary authority** for repository-specific knowledge, including conventions, patterns, gotchas, and house rules. Read it in full at skill launch and re-read it whenever context drifts. If `CLAUDE.md` does not exist, offer to create it from the provided template before continuing. Consult the official Instruqt documentation (https://docs.instruqt.com/) only when the repository's `CLAUDE.md` is missing guidance or is incomplete on a specific topic.
- **`CLAUDE.md` is a knowledge base, not a configuration file.** Never store feature flags, toggles, or runtime settings in it.
- **The skill is stateless.** All durable project knowledge belongs in the repository (`CLAUDE.md`, track `MEMORY.md` files, and other repository assets), never in the skill or the conversation alone.
- **The repository is expected to evolve over time.** Each completed build should leave the repository in a better state than it was found, subject to explicit user approval.
- **Be a deterministic collaborator.** Never infer, assume, or invent requirements, behaviors, or defaults. When multiple approaches exist, present the available options with their tradeoffs and ask the user to decide. Do not continue past a decision point until the user has answered. If anything is ambiguous, stop and ask.
- **Approval gates are mandatory.** The build plan, audit, push, and knowledge review each require explicit user approval. Never create, modify, or delete repository files beyond an approval gate without that approval.
- **Never speculate.** Before asserting platform behavior, verify it against the repository's `CLAUDE.md`, existing repository assets, or the official Instruqt documentation. If none of these confirm the behavior, state that plainly.
- **Existing tracks are read-only references.** Never modify any track other than the one currently being built.

## Tiered Standards

Two tiers govern every validation performed by this skill:

- **Required (blocking):** Learner-facing correctness, cross-file consistency, accessibility, and security. Violations block progression until resolved.
- **Advisory (non-blocking):** Pedagogy and style guidance, such as prose depth, example sizing, and step structure. Surface these as recommendations. The repository's `CLAUDE.md` may promote any advisory standard to required; when it does, treat it as required.

## Phase 0 — Grounding

Runs first at every skill launch, before any other action.

**1. Resolve the repository root**, in strict priority order:

1. **Explicit path.** If the user has provided a repository path (in the request or earlier in the conversation), use it.
2. **Detect and confirm.** Otherwise, examine the current working folder for track-repository indicators — a `CLAUDE.md` at the root, directories containing a `track.yml`, or other repository-level signals. Do not hard-code detection to a single file or directory layout; the repository structure may evolve over time. If indicators are found, propose the detected root and ask the user to confirm it.
3. **Ask.** If detection finds no indicators, ask the user for the repository root.

Never silently assume a root. Do not proceed until the root is confirmed.

**2. Read the repository's `CLAUDE.md` in full.**

- If it exists, it is the primary authority for the rest of the session.
- If it does not exist, offer to create it, initialized from `references/CLAUDE-TEMPLATE.md` as a starting point. The template is intentionally minimal and may evolve independently of this skill — adapt the initialization to the repository at hand rather than requiring a verbatim copy. Write it only after the user approves.
- If the user declines creation, continue the session using the official Instruqt documentation as the knowledge source, and note that the Knowledge Review phase will re-offer creating `CLAUDE.md` before any learnings can be persisted.

**3. Record fallback lookups.** Whenever guidance is missing or incomplete in `CLAUDE.md` and the official documentation (https://docs.instruqt.com/) is consulted instead, note the topic. Both learned guidance and documentation-derived guidance are staged as candidate `CLAUDE.md` additions for the Knowledge Review phase; nothing is written without explicit user approval.

## Launch Action — Create the Working Folder

Immediately after grounding, and before discovery begins:

1. Ask the user for a **working folder name** — kebab-case, no spaces. Explain that it is provisional: the folder is renamed to the approved final track slug during the design → build transition, with all internal references updated accordingly.
2. Create `<repo-root>/<working-name>/`.
3. Seed `<repo-root>/<working-name>/MEMORY.md` with the track memory skeleton. If a `MEMORY.md` already exists in the folder, reuse and continue updating it — never overwrite it. Use the skeleton defined in the repository's `CLAUDE.md` if one is present; otherwise use the skeleton in `references/CLAUDE-TEMPLATE.md`. The skeleton captures, at minimum: the track concept, research notes, design decisions, open questions, session history, and deferred work.
4. Confirm both were created, then move to Discovery.

**Track `MEMORY.md` update rule (applies for the rest of the flow):** `MEMORY.md` is created only for the track currently being built, and it is the track's implementation log. Keep it current automatically — record meaningful events as they happen, including decisions, research, questions, progress, generated files, audit results, pushes, and deferred work. No user approval is required for updates to the track's `MEMORY.md`; its purpose is to let any future session reconstruct the build history, current state, and outstanding work.

## Phase 1 — Discovery

Discovery is user-driven and has no fixed shape. The skill adapts to how the user wants to work.

**Fixed boundaries:**

- Grounding and the launch action have already happened.
- Discovery concludes only after the user explicitly approves moving to the Build Plan phase ("give me the plan," "let's plan this," or similar). Reaching a natural stopping point does not automatically advance the workflow.

**Adaptive behaviors:**

- If the user asks for suggestions, propose options grounded in the repository's `CLAUDE.md` and in existing repository assets — tracks, templates, examples, snippets, documentation, and other resources — that demonstrate the pattern under discussion. Cite real precedents, not abstract patterns. Prioritize the repository's `CLAUDE.md` and repository assets first; consult the official Instruqt documentation only when repository guidance is missing or incomplete, and say that is the source.
- If the user shares research, links, pasted content, or file references, read what is referenced before responding, and fold it into the design conversation.
- If the user wants approaches compared, run the comparison and present tradeoffs — then ask which to use.
- Log continuously to the track's `MEMORY.md` per the update rule: research to research notes, decisions to design decisions with a one-line rationale, unresolved items to open questions (moved out when resolved), and every meaningful event to the session history.
- If discovery runs long, offer a soft checkpoint — a summary of what has been captured so far — but don't force it.

### Optional Feature — Cross-Track Reuse Scan

**When to offer:** the repository contains existing tracks, and the new track's infrastructure or mechanics have not been locked in yet. Offer it early in discovery, with a skip option. Run it only after the user approves.

**On approval:** read `references/REUSE-FINDER.md` in full at that moment and execute it as written. The scan identifies reusable infrastructure, patterns, lifecycle scripts, assets, and implementation approaches in the repository's existing tracks, returning short "you're close to track X" pointers — its purpose is to recommend reuse opportunities, not duplicate existing work. Log raw matches to the track's `MEMORY.md` research notes; log any match that shapes the design to design decisions.

The scan is a design input: run it (or skip it) before the build plan is assembled.

### Optional Feature — Customer Branding Extraction

**When to offer:** the track is being built for a specific, named customer or prospect. Offer it as soon as the customer is unambiguously identified — never at launch, since it may not yet be known whether the track is customer-specific. If no customer is named during discovery, ask once near the end of discovery whether this is a customer-branded build. Run it only after the user approves.

**On approval:** read `references/BRANDING-EXTRACTOR.md` in full at that moment and execute it as written. Output lands in `<working-folder>/branding/`. Log to the track's `MEMORY.md`: the sources used, the retrieval date, and a design-decision entry recording that the kit exists.

**Guardrails:**

- Never start extraction until the customer name and website are confirmed with the user — a wrong-customer run is the most expensive failure this feature can produce.
- If a branding kit for the same customer already exists elsewhere in the repository, offer to copy it and refresh it incrementally rather than re-extracting from scratch. Reuse existing assets whenever practical; recreate only what is stale or missing.
- The extraction must complete (or be explicitly skipped) before the build plan, so the kit is a design input rather than an afterthought.

### Reference Repository Assets During Discovery

When uncertain how something is implemented, inspect the relevant repository assets before responding — existing tracks, templates, examples, snippets, documentation, and other resources. The repository's `CLAUDE.md` may name canonical examples; the broader repository is also fair game. Existing tracks remain read-only references, but they are only one type of repository asset.

## Authoring Standards

Applied to all learner-facing content generated in the Build phase, and enforced in the Audit phase per the Tiered Standards.

**Required (blocking):**

- **Tab navigation.** When a step requires the learner to act on a different challenge tab than the one in focus, provide a tab button for the transition. Never instruct a learner to manually hunt for a tab.
- **Tab relevance.** Every tab in a challenge must serve a purpose and be referenced by at least one learner action. No unused tabs.
- **Quiz integrity.** A quiz answer must not be disclosed, implied, or inferable from any learner-facing content that precedes the quiz.
- **Branding consistency.** When a track is customer-branded, apply the branding consistently across every challenge and all learner-facing content. Never mix default and customer styling unless the user asks for it.

**Advisory (non-blocking, unless the repository's `CLAUDE.md` promotes them):**

- **Example sizing.** Default to the smallest useful code example — one idea, one path, one success state per example.
- **Prose depth.** Surround examples with prose that teaches: orient the learner at the start of an assignment, explain each step's purpose before its code block, and describe concretely what success looks like. A step that is only a heading and a code fence is too thin.
- **Tell / Show / Do.** For each step: name what the learner is about to do and why (Tell), present the minimal example (Show), and give a clear action with a concrete success signal (Do).
- **Consolidated notes.** Where challenges use notes slides, prefer a single consolidated card per challenge over fragmented note elements.

## Phase 2 — Build Plan (hard stop)

When the user approves moving to this phase, assemble a build plan from everything captured in the track's `MEMORY.md` and the conversation. The plan must include:

- Track working name, title, audience, and learner outcome
- **Acceptance criteria / Definition of Done** — what constitutes a successful completed track from both the learner's perspective and the author's perspective. The audit validates against this later.
- Duration, difficulty, and timing/pause settings
- Chosen infrastructure pattern, with rationale and the repository asset(s) or documentation it is drawn from
- Reuse opportunities (if the reuse scan ran): which repository assets to lift — templates, patterns, snippets, examples, lifecycle scripts, documentation, or existing tracks — and exactly what
- Directory tree preview: `track.yml`, `config.yml`, `track_scripts/`, each challenge folder with its files, `assets/`
- Lifecycle scripts to be created (track-level and per-challenge), including hostname suffixes
- Tab layout per challenge (types, hosts, ports, paths)
- Branding kit status — present (state exactly where branding will be applied) or skipped (state why)
- The repository `CLAUDE.md` conventions that will be applied, and any gaps where official documentation will be the source
- **Source of truth for each major design decision** — repository asset, `CLAUDE.md`, official Instruqt documentation, or user decision — for traceability and future maintenance
- Flagged risks, unknowns, and open questions

**Hard stop.** Present the plan and wait for the user's explicit approval. Do not generate any track files past this gate without it. If final gaps remain (e.g., a timing setting), ask targeted questions — but only about genuinely unsettled items.

## Design → Build Transition

Triggered when the user approves the build plan and the final track slug is agreed.

1. Confirm the final slug with the user if not already stated.
2. Rename `<repo-root>/<working-name>/` to `<repo-root>/<final-slug>/`.
3. Update the track's `MEMORY.md`: record the rename, keep the working name for traceability, and update all internal references to the folder name.
4. Verify that all internal references, generated paths, and repository metadata remain consistent after the rename before continuing the build.
5. From this point forward, all generated artifacts use the final slug — never the working name.

## Phase 3 — Build

Scaffold the track in dependency order, applying the repository `CLAUDE.md` conventions exactly. Where `CLAUDE.md` is silent, follow the official Instruqt documentation and stage the doc-derived guidance for the Knowledge Review. Do not invent or improvise conventions.

**Reuse before generation:** before creating any new implementation, check for reusable repository assets that satisfy the requirement — templates, patterns, snippets, examples, lifecycle scripts, documentation, or existing tracks. Prefer reuse and adaptation over generating new content from scratch.

**Build order:**

1. Directory structure with sequential challenge numbering (`01-`, `02-`, …)
2. `track.yml` — do not fabricate platform-assigned IDs (omit `id` for new tracks; the platform assigns it on push) and do not manually set auto-generated fields such as `checksum`
3. `config.yml` — infrastructure resources matching the approved plan
4. `track_scripts/` — track-level lifecycle scripts
5. Each challenge directory: `assignment.md` (frontmatter + body per the Authoring Standards and repo conventions), and per-challenge `setup-*`, `check-*`, `solve-*` scripts as planned. Check scripts should emit actionable failure feedback; solve scripts should be idempotent and produce exactly the state the check validates.
6. The track's `MEMORY.md` architecture notes — populated as the build progresses with images, environment variables, and gotchas specific to this track. Keep these notes concise and implementation-focused; large reusable artifacts belong as repository assets, referenced from `MEMORY.md` rather than embedded in it.
7. `README.md` — generated from the repository's README template (the preferred source); fall back to `references/README-TEMPLATE.md` only if no repository template exists. Every placeholder replaced; facts reconciled with `track.yml`, `config.yml`, and `MEMORY.md`.

**Per-write validation hooks** (modular and extensible — the list below is a baseline, not exhaustive; additional validation passes may be added over time without changing the overall workflow):

- After every YAML write: parse it to confirm it loads.
- After every script write: syntax-check it (`bash -n` for shell scripts) and confirm re-running it would not break state.
- After every `assignment.md` write: verify code-block modifiers and tab references are valid; review prose against the advisory standards.
- After every README write: scan for unreplaced placeholders.

**Branding application (only when a branding kit exists and the plan says branding is applied):** apply the kit where the plan specified; keep the kit folder as the canonical source; copy into `assets/` only files the track actually references.

**Building against unverifiable surfaces (beta APIs, unreleased schemas, exact identifiers that cannot be confirmed before runtime):** guard every uncertain call so failure does not break setup; print the fallback values a human would need to complete the step manually; and log each unverified assumption as a runtime-verify item in the track's `MEMORY.md`. Unverified areas must be deliberate, logged decisions — never silent gaps.

Report each file written with a one-line confirmation, and report when an existing repository asset was intentionally reused or adapted instead of regenerated. Log meaningful build events to the track's `MEMORY.md` as they happen.

## Phase 4 — Audit (pre-push gate)

The audit is mandatory before any push. It is a **modular framework of independent passes**. The skill owns the audit framework and its passes — the baseline passes below ship with the skill and may be refined or extended in future skill releases. The repository's `CLAUDE.md` defines repository-specific rules and standards that are **evaluated by** the existing passes (including promoting advisory standards to required); repositories do not define new audit passes.

**Severity model.** Every finding is classified as:

- **Blocking** — violates a required standard (learner-facing correctness, cross-file consistency, accessibility, security) or a repo-promoted standard. Blocks progression until resolved.
- **Warning** — likely problem or unverified assumption; review recommended. Does not block.
- **Informational** — advisory observations (pedagogy, style, improvement opportunities). Does not block.

**Acceptance criteria as first-class input.** The Build Plan's acceptance criteria / Definition of Done is a primary audit input. Summarize it at the beginning of the audit report, before evaluating the implementation against it.

### Baseline passes

**Re-ground.** Re-read the repository's `CLAUDE.md`, the track's `MEMORY.md`, and the README template in full. Do not audit from memory of them.

**Conformance.** Walk every generated file. Trace each rule-bound element to its source of truth — the repository's `CLAUDE.md`, a repository asset, or the official documentation. Anything that cannot be traced → Warning.

**Decision & acceptance traceability.** Verify each design decision in the track's `MEMORY.md` is reflected in the implementation (contradicted → Blocking; partial → Warning). Verify the build satisfies the plan's acceptance criteria / Definition of Done — from both the learner's and the author's perspective (unmet criterion → Blocking). Implementation details absent from the design log → Warning.

**Cross-file consistency.** All Blocking when violated: `track.yml` slug matches the folder name; challenge order matches directory numbering; `config.yml` hostnames match script filename suffixes; every tab hostname resolves to a defined host; README has no unreplaced placeholders and its facts match `track.yml`/`config.yml`. Additionally: every `secrets:` entry declared in `config.yml` must have a corresponding secret in the target organization — the skill cannot see org secrets, so enumerate them and have the user confirm each exists (unconfirmed → Warning until confirmed).

**Verification sweep.** Verifies unsupported assumptions, fabricated identifiers, missing assets, and unverified platform behavior. Fabricated platform-assigned IDs → Blocking. Referenced files or assets that do not exist on disk → Blocking. Environment variables, CLI subcommands, or platform behaviors asserted without verification against `CLAUDE.md`, a repository asset, or official documentation → Warning. For builds against unverifiable surfaces: confirm every uncertain call is guarded, fallbacks are printed, and each assumption is logged as a runtime-verify item — ungated uncertainty → Warning.

**Security & secrets.** Credential leaks are Blocking: secrets, API keys, tokens, or private keys hardcoded in `config.yml` environment blocks or scripts (including base64-encoded — encoding is not encryption); any secret value in a file that ships with the track or is visible in the debug log. Do not auto-fix credential findings silently — moving a value to `secrets:` requires the user to set it in the platform; surface each with file and line. Hygiene issues are Warning: missing cleanup for costed or external resources, secrets echoed to logs, overly broad IAM, cloud variables referenced without a matching resource block.

**Check/solve robustness.** All findings Warning, ranked worst-first: (1) a check that can never fail — it validates nothing; call this out explicitly as almost always worth fixing; (2) a check that passes on partial state; (3) non-idempotent setup or solve; (4) a solve that assumes prior challenges ran while skipping is enabled; (5) thin checks where granular, actionable failure feedback would help.

**Authoring standards.** The Required standards (tab navigation, tab relevance, quiz integrity, branding consistency) → Blocking when violated. Advisory standards → Informational, unless promoted by the repository's `CLAUDE.md`. Authoring-standard violations require judgment about learner flow — propose each fix and confirm with the user before applying.

### Knowledge capture during the audit

When the audit identifies a recurring pattern, implementation improvement, or documentation gap, stage it as a candidate addition for the Knowledge Review. Never write directly to `CLAUDE.md` from the audit.

### Audit report

Produce a single report structured as:

1. **Summary** — acceptance criteria / Definition of Done recap, audit passes executed, findings by severity, auto-fixes applied, and any remaining user decisions.
2. **Detailed findings** — Blocking (itemized, with file and line references), Warning (itemized), Informational (summarized).

Append the report to the track's `MEMORY.md` with the date.

### Fix policy

- **Auto-fix first** where the fix is deterministic, reversible, and low-risk — objective corrections only, such as formatting, unreplaced placeholders, ordering mismatches where disk is the source of truth, syntax errors, and other unambiguous issues. Re-run the affected passes after fixing and update the report.
- **Surface for direction** where the fix is ambiguous (e.g., a hostname mismatch where either side could be wrong). Present the finding, a proposed resolution, and ask.
- **Never auto-fix without user approval** anything involving credentials, learner-facing behavior, architectural decisions, repository knowledge, or user design decisions.

**Hard gate:** the audit is not complete until every Blocking finding is resolved. Push commands are surfaced only after the report contains no Blocking findings.

## Phase 5 — Push & Runtime Tests

Entered only after the audit contains no Blocking findings.

### CLI execution rules

- **Auth retry.** If any `instruqt …` command fails with an authentication or token-refresh error, run `instruqt auth login` and retry before treating it as a real failure.
- **Verify before committing to a command.** Do not assume CLI subcommands or flags exist — verify against the repository's `CLAUDE.md`, the official documentation, or the CLI's own help output before using or recommending a command.

### Pre-push hygiene sweep

Immediately before every push (first push and every re-push):

- Sweep `track.yml`, `config.yml`, every `assignment.md`, and every lifecycle script for encoding artifacts — in particular null bytes (`0x00`), which files edited on Windows hosts can accumulate and which the push rejects. Strip if found.
- Run `instruqt track validate` and resolve anything it reports.

### Pre-push confirmation

Before surfacing push commands:

1. Display the current CLI configuration and active team.
2. Confirm with the user which organization the track is for. If it differs from the active team, switch and confirm. Note: the `owner:` field in `track.yml` — not the active team — determines the push destination; verify it matches the intended target.
3. Confirm declared resources exist in the target organization: every `secrets:` entry and any other externally provisioned reference. A missing one fails the push at resource resolution (commonly surfacing as "Entity not found" even when validation passed).

Only after these confirmations, surface the push commands (`instruqt track validate`, `instruqt track push`, `instruqt track open` from the track folder).

**Troubleshooting "Entity not found":** it occurs at resource resolution, after structural validation. Check in order: (1) a declared secret or referenced resource missing in the target org — the most likely cause when validation passed; (2) active team ≠ `owner:` (affects test/pull operations); (3) a brand-new unregistered track — verify before acting on this.

### Runtime tests (opt-in)

After a successful audit, offer:

- **Layer 1 — Static validation:** YAML parse, script syntax checks, cross-references, `instruqt track validate`. Local and fast.
- **Layer 2 — Solve/check parity:** for each challenge, run setup → check (expect fail) → solve → check (expect pass). Catches drift between solves and checks. Requires a push.
- **Layer 3 — End-to-end playthrough:** sequential walk through all challenges, confirming downstream setups handle prior state. Requires a push.
- **Skip** — surface push commands only.

The user picks. Log results to the track's `MEMORY.md`.

## Phase 6 — Knowledge Review

The final phase. Its purpose is to leave the repository better than it was found — subject entirely to user approval.

1. **Assemble candidates** staged throughout the session: documentation-derived guidance recorded during fallback lookups, patterns and gotchas learned during the build, items staged by the audit, repository conventions introduced or refined during the session, and any convention the user corrected or refined mid-flow.
2. **Filter for durability.** A candidate belongs in `CLAUDE.md` only if it is repository-general — useful beyond the track just built. Track-specific detail stays in the track's `MEMORY.md`.
3. **Present for review.** Show each candidate with its proposed wording and its proposed location in `CLAUDE.md`. The user approves, rejects, or edits each item individually.
4. **Write only what is approved.** Update `CLAUDE.md` with approved items only. If `CLAUDE.md` does not exist (creation was declined at grounding), re-offer creating it now; if declined again, the candidates are recorded in the track's `MEMORY.md` deferred-work section instead.
5. **Log the outcome** to the track's `MEMORY.md`: what was promoted, what was rejected, what was deferred.

## Mid-Flow Re-Grounding

If discovery or build runs long enough that context may be stale, re-read the repository's `CLAUDE.md` and the track's `MEMORY.md` before continuing. Do this without announcing it. Triggers: many turns since the last read; the user references a convention you are not certain you are applying; the user corrects a behavior; starting any new file generation.

## Output Discipline

- Write track files directly to the track folder in the repository — the user needs to see them where they live.
- Confirm each file write with a one-line acknowledgment, not a full read-back.
- When existing repository assets are intentionally reused or adapted instead of generated, report that reuse alongside newly created or modified files, so the final build summary accurately reflects both generated and reused work.
- Do not surface internal or session-temporary paths to the user; use repository-relative paths.
- After the Knowledge Review completes, close with a brief summary of what was built, what was reused, what was promoted to `CLAUDE.md`, and any outstanding deferred work. No verbose postamble.
