# CLAUDE.md

This file is the knowledge base for this Instruqt track repository. It is the primary
authority for repository-specific conventions, patterns, gotchas, and standards. It is
a knowledge base, not a configuration file — no feature flags or runtime settings.

It grows over time: new guidance is added through the Knowledge Review at the end of
each track build (with explicit approval), or by direct edits.

House style for entries: one bullet per pattern, opening with a **bold lead** that
names the pattern, followed by the how and the why, closing with a citation of the
evidence track(s) it was observed in. When a bullet accumulates several distinct
concerns, split them into indented sub-bullets under a shared lead so the file stays
scannable and greppable.

## Table of Contents

<!-- Navigation map — entries are the exact section headings below. Regenerate this
     list whenever sections are added, renamed, or removed. -->

- Repository Overview
- Track Structure
- Key Configuration Files
  - track.yml
  - config.yml
  - assignment.md
- Script Conventions
- Common Infrastructure Patterns
- Decision Frameworks
- Anti-patterns
- Promoted Standards
- Working with Tracks
  - Scan-coverage ledger
- Track Memory Skeleton

## Repository Overview

<!-- Describe this repository: who owns it, what kinds of tracks it contains,
     and any organizational context a contributor should know. -->

## Track Structure

Every track is a top-level directory following the standard Instruqt layout:

    track-name/
    ├── track.yml              # Track metadata (title, description, timing)
    ├── config.yml             # Sandbox infrastructure definition
    ├── track_scripts/         # Track-level lifecycle scripts (optional)
    │   ├── setup-<hostname>
    │   └── cleanup-<hostname>
    ├── 01-challenge-name/     # Numbered challenge directories
    │   ├── assignment.md      # Instructions (YAML frontmatter + Markdown body)
    │   ├── setup-<hostname>   # Per-challenge setup (optional)
    │   ├── check-<hostname>   # Validation script (exit 0 = pass)
    │   ├── solve-<hostname>   # Auto-solve script (optional)
    │   └── cleanup-<hostname> # Per-challenge cleanup (optional)
    └── assets/                # Images and media referenced in assignments

Reference: https://docs.instruqt.com/

## Key Configuration Files

<!-- Field-level knowledge about each track file, one H3 per file. Record valid
     fields, accepted values, and observed conventions as they are learned. -->

### track.yml

<!-- Track identity, timing, tags, and lab_config / UI settings. -->

### config.yml

<!-- Sandbox infrastructure: containers, virtualmachines, virtualbrowsers, cloud
     accounts (aws_accounts / azure_subscriptions / gcp_projects), secrets.
     Record image names, sizing conventions, and per-resource gotchas. -->

### assignment.md

<!-- Challenge frontmatter (slug, type, tabs, notes, lab_config overrides) and
     Markdown body features (code-block modifiers, tab buttons, variable
     interpolation, body structure conventions). -->

## Script Conventions

<!-- Lifecycle-script knowledge (setup / check / solve / cleanup). Organize into
     themed H3 subsections as guidance accumulates — for example: script
     fundamentals; package installs & OS provisioning; check & solve patterns;
     environment variables & secrets; terminal & shell UX; Docker & Kubernetes
     idioms; wait/readiness/retry helpers; debugging & observability; plus one
     subsection per recurring vendor or product stack. Hard-won platform and
     tooling gotchas belong here too — or in Anti-patterns when they are
     "never do this" rules. -->

## Common Infrastructure Patterns

<!-- One row per sandbox topology, naming the tracks that implement it. This
     table grows into the repository's reuse map — the first place to look when
     scaffolding a new track. -->

| Pattern | Examples |
|---|---|

## Decision Frameworks

<!-- When two valid approaches exist, record the rule of thumb for choosing —
     one H3 per decision (e.g., sandbox preset vs custom config.yml; custom image
     vs setup-script generation; state-passing mechanism A vs B) with the
     trade-offs stated. -->

## Anti-patterns

<!-- Recurring mistakes worth heading off explicitly. One H3 per anti-pattern,
     phrased as "Don't ..." — state the failure mode, the observable symptom, and
     the fix, citing observed instances where possible. -->

## Promoted Standards

<!-- Advisory authoring standards promoted to required (blocking) for this repository.
     List each promoted standard explicitly. Empty means no promotions. -->

## Working with Tracks

<!-- Cross-cutting platform mechanics every contributor must know: platform-assigned
     IDs and checksums (never fabricate), challenge numbering rules, asset-upload
     behavior, encoding/null-byte sweeps, and any pre-push gates. -->

### Scan-coverage ledger

<!-- Staging inbox for repository-wide reconciliation runs: batch findings land
     here first, then fold into the topical sections above at the end of each
     run. Also records near-duplicate, variant, and localized track relationships
     so contributors know which tracks ship together. -->

## Track Memory Skeleton

Each track under construction carries a `MEMORY.md` — its implementation log. Seed new
track folders with this skeleton:

    # MEMORY — <track-name>

    Status: Discovery            <!-- Discovery → Design → Build → Live -->
    Working name: <working-name>
    Final slug: (TBD — rename folder when design is finalized)
    Owner: <owner>

    ## Track Concept

    ## Research Notes

    ## Design Decisions

    ## Open Questions

    ## Session History

    ## Deferred Work

    ## Track Architecture Notes
