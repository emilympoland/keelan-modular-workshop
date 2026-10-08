---
slug: setup
id: h5chbphc8ajr
type: challenge
title: Setup
teaser: Enter your API keys and confirm the sandbox reaches LangSmith and the model.
notes:
- type: text
  contents: |-
    <style>
      .sp { background:#030710; border-radius:6px; padding:36px 28px 28px; text-align:center; font-family:'Inter','Helvetica Neue',Arial,sans-serif; }
      .sp-badge { display:inline-block; font-family:Consolas,monospace; font-size:12px; text-transform:uppercase; letter-spacing:.1em; color:#030710; background:#7FC8FF; border-radius:999px; padding:4px 14px; margin-bottom:16px; }
      .sp-title { font-weight:300; font-size:38px; letter-spacing:-0.03em; color:#E5F4FF; margin:0 0 10px; }
      .sp-title span { color:#7FC8FF; }
      .sp-sub { font-size:16px; color:#99D3FF; max-width:760px; margin:0 auto 22px; }
      .sp-card { background:#161F34; border:1px solid #2F4B68; border-radius:6px; padding:18px 22px; text-align:left; color:#CCE9FF; font-size:14.5px; max-width:960px; margin:0 auto; }
      .sp-card b { color:#E5F4FF; }
      .sp-k { font-family:Consolas,monospace; font-size:12px; text-transform:uppercase; letter-spacing:.08em; color:#7FC8FF; }
      .sp-card ul { margin:6px 0 14px; padding-left:20px; }
      .sp-card li { margin:5px 0; }
    </style>
    <div class="sp">
      <div class="sp-badge">Challenge 1</div>
      <p class="sp-title">01 - Setup</p>
      <p class="sp-sub">Connect your accounts and confirm the sandbox can reach LangSmith and the model.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Enter your API keys in the notebook, call the model once, then verify the connection independently.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Trace</b> — the record of one run: every model call, tool call and step, with inputs, outputs and timing.</li>
          <li><b>Tracing project</b> — the named container traces are written to. Yours is unique to this sandbox, so your runs stay separate from other attendees'.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>Checks read your LangSmith traces or the sandbox, so they pass only if your code ran.</p>
      </div>
    </div>
tabs:
- id: jqiod2bgquk7
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/setup/tree/instruqt/01_setup.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: basic
timelimit: 0
enhanced_loading: null
---
You'll start with `create_agent()` and build up to a research agent with
planning, file tools, and subagents.

This challenge configures your keys and checks the environment.

---

<h2 style="color: #7FC8FF;">Step 1: Enter your API keys</h2>

- **LangSmith** — required to see your agent's traces. [smith.langchain.com](https://smith.langchain.com) → Settings → API Keys. Starts with `lsv2_`. Keys from any LangSmith region (US, EU, APAC, AWS US) work. The cell detects which one.
- **Tavily** — web search. Optional now, required from Challenge 3 on: the Checks fail without it. [app.tavily.com](https://app.tavily.com). Starts with `tvly-`. Press Enter to skip.
- **Short name or initials** — optional, names your tracing project. e.g. `jdoe`.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

Run the first code cell (click it, then `Shift+Enter`) and paste your keys when
prompted. Input is masked; keys stay in this sandbox.

**Result:** `✅ Keys verified and stored. Tracing is ON.`

---

<h2 style="color: #7FC8FF;">Step 2: Confirm the environment</h2>

Run the next two cells in order.

The first prints your configuration:

```text,nocopy
tracing enabled : true
tracing project : instruqt-deep-agents-jdoe-a1b2c3d4
langsmith host  : api.smith.langchain.com
model           : us.anthropic.claude-sonnet-4-6
aws region      : us-east-1
langsmith key   : present (masked)
tavily key      : present (masked)
```

The second cell sends one message to **Claude on Amazon Bedrock** and prints
the reply.

> **Wait for the model's answer before moving on.** The first call in a fresh
> sandbox can be slow while AWS finishes enabling access. If it errors, run the
> cell again. The Check looks for this cell having run.

**Result:** model reply printed; one trace logged to your project.

---

<h2 style="color: #7FC8FF;">Step 3: Verify the LangSmith connection</h2>

Run the next cell. It re-checks your LangSmith connection from a fresh
process. This confirms the key is saved to disk and authenticates outside the
kernel.

Expected output:

```text,nocopy
── LangSmith connection check ─────────────────────
✓ API key present (value masked)
✓ api.smith.langchain.com authenticated (US region)
✓ tracing project: instruqt-deep-agents-jdoe-a1b2c3d4
✓ tracing enabled: true
───────────────────────────────────────────────────
```

Every Check in this track reads LangSmith or files on disk, not cell output.

---

Next: Challenge 2 compares `create_agent()` and `create_deep_agent()` on file
operations.
