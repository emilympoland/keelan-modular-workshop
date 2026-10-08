# {{TRACK_TITLE}}

**Platform:** Instruqt
**Audience:** {{PRIMARY_AUDIENCE}}
**Duration:** {{APPROX_DURATION}}
**Difficulty:** {{DIFFICULTY}}
**Outcome:** {{LEARNER_OUTCOME}} <!-- one-sentence completion payoff -->
**Pausable:** {{PAUSABLE_STATEMENT}} <!-- omit for short tracks -->

> **Build log:** decisions, known-good state, and unresolved items for this track
> are tracked in [`MEMORY.md`](./MEMORY.md). Read it before editing.

---

## Track overview

{{ONE_TO_TWO_PARAGRAPH_DESCRIPTION}}

<!-- Cover: what the learner walks away able to do, what real product/workflow it
     mirrors, and anything special about the sandbox. -->

---

## Directory structure

```
{{TRACK_SLUG}}/
├── track.yml                  # Track metadata, timing, lab config
├── config.yml                 # Sandbox infrastructure
├── MEMORY.md                  # Track build log
├── track_scripts/             # Track-level lifecycle scripts (if any)
├── assets/                    # Icons and images
├── 01-{{CHALLENGE_SLUG}}/
│   ├── assignment.md
│   ├── setup-{{HOSTNAME}}     # (optional)
│   ├── check-{{HOSTNAME}}     # exit 0 = pass
│   └── solve-{{HOSTNAME}}     # idempotent auto-solve (optional)
└── NN-{{CHALLENGE_SLUG}}/
```

---

## Sandbox architecture

<!-- Diagram the runtime layout: hosts, what runs on each, exposed ports, and how
     the learner reaches them. -->

### Hosts

| Host | Image | Sizing | Role |
|------|-------|--------|------|
| `{{HOSTNAME_1}}` | `{{IMAGE_1}}` | {{SIZING_1}} | {{ROLE_1}} |

### Tabs exposed to the learner

| Tab title | Type | Points to | Notes |
|-----------|------|-----------|-------|
| {{TAB_TITLE}} | {{TAB_TYPE}} | {{TAB_TARGET}} | {{TAB_NOTES}} |

---

## Challenge map

| # | Title | Core skill | Check logic |
|---|-------|------------|-------------|
| 1 | {{CHALLENGE_1_TITLE}} | {{CORE_SKILL}} | {{CHECK_CRITERIA}} |

<!-- Check-logic column: what the check script actually validates, stated in the
     same language as the learner-facing failure message. -->

---

## Deploying the track

### Prerequisites

- Instruqt CLI installed and authenticated (`instruqt auth login`)
- Member of the `{{INSTRUQT_TEAM_SLUG}}` Instruqt organization

### Push to Instruqt

```bash
cd {{TRACK_SLUG}}/
instruqt track validate
instruqt track push
instruqt track open
```

---

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| Push fails validation on a tab `url:` | Non-HTTPS `url:` | Use HTTPS, or a `service` tab with `hostname` + `port` |
| Push fails: `invalid byte sequence for encoding "UTF8": 0x00` | Null-byte artifact from Windows editing | Strip null bytes from the affected files and re-push |
| Push fails: "Entity not found" after clean validation | A declared secret or resource missing in the target org | Create it in the org, or remove the unused reference |
| Embedded `service` tab refuses to render | Upstream sends `X-Frame-Options` / CSP headers that block iframing | Override headers on the `service` tab or front the upstream with a header-stripping proxy |
| {{SYMPTOM}} | {{CAUSE}} | {{FIX}} |

---

## Related resources

- [Instruqt documentation](https://docs.instruqt.com/)
- {{PRODUCT_LINK}} <!-- optional: vendor/product docs -->

---

## Build log

All build decisions, known-good state, and unresolved items live in
[`MEMORY.md`](./MEMORY.md) at the track root. Keep this README reconciled with
`MEMORY.md` — if they diverge, the README is wrong until proven otherwise.
