# REUSE-FINDER — Cross-Track Reuse Scan (companion procedure)

Authoritative procedure for the skill's cross-track reuse scan (Discovery, opt-in,
user-approved). The skill reads this file in full at the moment the scan is approved —
never from a cached summary.

**What it is:** a lightweight scan that compares the track currently being designed
against the repository's existing tracks and assets, and returns short
"you're close to track X" pointers — the specific existing assets (infrastructure,
patterns, lifecycle scripts, templates, snippets, implementation approaches) worth
reusing before building from scratch. Its purpose is to recommend reuse opportunities,
not duplicate existing work.

**What it is NOT:** an exhaustive diff, a quality audit, or a deliverable. It surfaces
a handful of strong pointers and stops. If there is no close precedent, that is a
valid — and useful — result.

## When to run

- During Discovery, opt-in, only after the user approves. Offer it early — before an
  infrastructure pattern is locked in — so findings shape the design.
- Skip it for one-off designs, or when the user already knows their reference.
- It is a design input: run it (or skip it) before the Build Plan is assembled.

## Inputs — confirm with the user before scanning

1. **Corpus.** Default is the repository root confirmed at grounding. Accept any other
   path the user points at. If the path is not reachable, say so plainly and ask — do
   not silently scan nothing.
2. **Dimension(s) to match on.** The user picks one or more of the five below.
   Don't default to all five — matching on everything dilutes the pointers.
3. **What is known so far about the new track.** Pull from the track's `MEMORY.md`
   (concept, design decisions, open questions) if populated; otherwise ask for a
   one-line description (product/vendor, rough infrastructure idea, approximate
   challenge count).

## The five reuse dimensions

For each dimension: what it compares, the cheap signal to read, and the asset a match
points at.

1. **Infrastructure topology** — the `config.yml` shape: container vs VM vs
   cloud-account vs sandbox preset; single- vs multi-host; which cloud.
   *Signal:* `config.yml` resource blocks (and any infrastructure-pattern guidance in
   the repository's `CLAUDE.md`). *Reuse:* the `config.yml` skeleton and
   `track_scripts/` layout.
2. **Image / preset** — the base image(s) or `sandbox_preset`.
   *Signal:* `config.yml` `image:` / `sandbox_preset:` fields and image namespace.
   *Reuse:* the image-specific setup steps (bootstrap waits, preset hostnames,
   pre-baked tooling).
3. **Lifecycle-script pattern** — the mechanism the setup uses (browser IDE, remote
   desktop, in-VM Kubernetes, reverse proxy, tunnels, model servers, etc.).
   *Signal:* grep `track_scripts/` and per-challenge `setup-*` for telltale commands.
   *Reuse:* the actual setup/check/solve script.
4. **Challenge structure** — challenge count and flow, challenge vs quiz types, tab
   layouts, and the check/solve approach.
   *Signal:* number of numbered directories, `assignment.md` frontmatter (`type`,
   `tabs`), presence and shape of `check-*`/`solve-*`. *Reuse:* the challenge
   scaffolding and `assignment.md` structure.
5. **Product / vendor domain** — another track for the same product or vendor.
   *Signal:* `track.yml` `owner` / `tags` / `title`, the image namespace, and tool
   names in `assignment.md`. *Reuse:* domain content and conventions (and likely
   several of the above for free).

## Method — the scan

Keep it cheap. The corpus can be hundreds of tracks; do not read every file.

1. **Confirm inputs** (corpus, dimension(s), the new-track description).
2. **Enumerate candidates** — list the track directories under the corpus (each has a
   `track.yml`). Use file listing and targeted search, not full reads.
3. **Read only the cheap signal** for the chosen dimension(s): `track.yml` and
   `config.yml` for dimensions 1, 2, 4, 5; add a targeted grep of
   `track_scripts/`/`setup-*` for dimension 3. Read-only throughout.
4. **Score similarity** per dimension with a simple heuristic — same infra pattern,
   same image/preset, same setup mechanism, same product/vendor. A ranking aid, not a
   precise metric; don't over-engineer it.
5. **Rank and take the top few** (about three) matches per chosen dimension.
6. **Write one pointer per match** (see output format).

## Output format

A short list of pointers — not a report. One line each:

> **<track-slug>** — close on **<dimension>**: <one phrase on why>. Reuse its
> **<specific asset>** (`<path or filename>`).

Keep it to the strongest few. If nothing matches well on a chosen dimension, say so
plainly — "no close precedent on <dimension>; design fresh" — rather than forcing
weak matches.

## Logging

- Raw matches → the track's `MEMORY.md` research notes.
- Any match that actually shapes the build → design decisions, with a one-liner.
- A session-history line noting the scan ran, the dimension(s), and whether anything
  was adopted.

## Guardrails

- **Read-only on the corpus.** Never modify any track or asset other than the track
  being built.
- **Lightweight by design.** Surface pointers and stop. No deep diffs, no quality
  grading.
- **Don't force matches.** "No precedent" is a legitimate, valuable result — it tells
  the user this design is genuinely new.
- **Confirm the corpus before scanning.** If it is unreachable, say so and ask rather
  than reporting an empty scan as "no matches."
- **It's a design aid, not a gate.** The user decides what to actually reuse; the
  finder only points.
