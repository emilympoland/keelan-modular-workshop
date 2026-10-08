---
slug: middleware-and-hitl
id: cu18tsqim5mz
type: challenge
title: Middleware & HITL
teaser: Inject rules into every model call, audit every tool call, and pause for approval
  before a write runs.
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
      <div class="sp-badge">Challenge 5</div>
      <p class="sp-title">05 - Middleware &amp; <span>HITL</span></p>
      <p class="sp-sub">Inject rules into every model call, audit every tool call, and pause for approval before a write runs.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Uncomment two blocks, then approve the paused write and check the audit log.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Middleware</b> — functions such as wrap_model_call and wrap_tool_call that run around every model or tool call. Use them to inject rules, audit, or intercept.</li>
          <li><b>interrupt_on</b> — pauses execution before a named tool runs, so a human can approve, edit, or reject it first.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>The Check needs a write_file that ran after your approval, with the compliance rules in the model's prompt.</p>
      </div>
    </div>
tabs:
- id: uxnemietmtfa
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/middleware-and-hitl/tree/instruqt/05_middleware_and_hitl.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
Middleware runs around every model and tool call, so rules and logging live
in one place instead of in every tool. Human-in-the-loop (HITL) pauses the
agent before risky actions so a person can approve them.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Add middleware and approval</h2>

![Middleware wraps every model and tool call](../assets/deepAgentMiddleware.png)

Uncomment the `middleware=[...]` and `interrupt_on={...}` blocks.

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 2: Run it, then approve</h2>

Run the agent. It pauses before `write_file`; approve it in the next cell.

**Result:** `Approved!`, the reply, and an audit log with `write_file  success`.

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms the write ran only after your approval. Without a LangSmith key, it reads a local trace log instead.

---

Next: Challenge 6 moves the instructions into a file and adds a skill.
