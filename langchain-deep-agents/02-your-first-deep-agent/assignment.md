---
slug: your-first-deep-agent
id: 8sh0ol7iyyt4
type: challenge
title: Your First Deep Agent
teaser: create_agent() has no built-in file tools. Run the same file task with create_deep_agent().
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
      <div class="sp-badge">Challenge 2</div>
      <p class="sp-title">02 - Your First <span>Deep Agent</span></p>
      <p class="sp-sub">create_agent() has no built-in file tools. create_deep_agent() includes them.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Run the file task with create_agent(), then with create_deep_agent().</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>StateBackend</b> — the default backend: an in-memory virtual filesystem that persists within a thread and is empty in a new one.</li>
          <li><b>Virtual filesystem</b> — files the agent writes live in agent state, not on disk. The trace records each write.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>The Check finds the agent's write_file call to /haiku.txt in your trace.</p>
      </div>
    </div>
tabs:
- id: ot8iqdq5q8k6
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/your-first-deep-agent/tree/instruqt/02_your_first_deep_agent.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: basic
timelimit: 0
enhanced_loading: null
---
`create_agent()` runs a model with only the tools you give it.
`create_deep_agent()` adds a virtual filesystem, a to-do planner, and
subagents, which is what long, multi-step work needs.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Try the plain agent</h2>

Ask a plain `create_agent()` to write a file. It has no file tools, so it
can't.

**Result:** a text reply and `files in result: None`.

---

<h2 style="color: #7FC8FF;">Step 2: Build the deep agent</h2>

![create_deep_agent() built-in capabilities](../assets/deepAgentsDiag.png)

Paste the `create_deep_agent()` call from the notebook into the blank cell,
then give it the same task.

**Result:** `/haiku.txt` in the agent's files, and `write_file` in the trace.

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms your deep agent wrote `/haiku.txt`. Without a LangSmith key, it reads a local trace log instead.

---

Next: Challenge 3 adds a custom tool and a subagent.
