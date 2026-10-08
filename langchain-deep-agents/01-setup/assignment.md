---
slug: setup
id: jx5ibqsplizl
type: challenge
title: Setup
teaser: Enter your LangSmith API key and confirm the sandbox reaches the model.
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
      <p class="sp-sub">Connect LangSmith and confirm the sandbox can reach the model.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Enter your LangSmith API key in the notebook, call the model once, then verify the connection independently.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Trace</b> — the record of one run: every model call, tool call and step, with inputs, outputs and timing.</li>
          <li><b>Tracing project</b> — the named container traces are written to. Yours is unique to this sandbox, so your runs stay separate from other attendees'.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>Checks read your traces (LangSmith, or a local log without a key) or the sandbox, so they pass only if your code ran.</p>
      </div>
    </div>
tabs:
- id: xxos20fj1qsh
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
Connect the sandbox to LangSmith and confirm the model answers. Every run
you make is traced to your own project, and the Checks in this track read
those traces to confirm your code ran.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Enter your LangSmith API key</h2>

Get one at [smith.langchain.com](https://smith.langchain.com) → Settings → API
Keys (starts with `lsv2_`; any region works). It's required for Challenges 8,
10, 11, 12 and 14, and powers web search from Challenge 3. Press Enter to skip
it otherwise.

Run the first cell, paste your key, and optionally add your initials to name
your tracing project.

**Result:** `✅ Key verified and stored. Tracing is ON.`

---

<h2 style="color: #7FC8FF;">Step 2: Call the model</h2>

Run the next two cells. They print your configuration, then send one message
to Claude on Amazon Bedrock.

> The first call in a new sandbox can be slow while AWS finishes enabling
> access. If it errors, run the cell again.

**Result:** the model's reply.

---

<h2 style="color: #7FC8FF;">Step 3: Verify the connection</h2>

Run the last cell. It re-checks your key from a fresh process.

**Result:** a column of ✓ lines, ending in `tracing enabled: true`.

---

<h2 style="color: #7FC8FF;">Step 4: Check</h2>

Click **Check**. It confirms your key is stored, the model answered, and the connection works.

---

Next: Challenge 2 compares `create_agent()` and `create_deep_agent()`.
