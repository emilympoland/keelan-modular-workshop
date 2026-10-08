---
slug: middleware-and-hitl
id: gedmvqyxvkt5
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
- id: ye1zbwgcequg
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
Middleware runs around every model and tool call without changes to agent
code. `interrupt_on` pauses a specific tool call for human-in-the-loop (HITL)
approval before it runs.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Read the middleware</h2>

Run the setup cell, then the cell that defines two middleware:

![Middleware wraps every model and tool call](../assets/deepAgentMiddleware.png)

- **`compliance_rules`** — `wrap_model_call`, injects rules into every model call.
- **`audit_trail`** — `wrap_tool_call`, logs every tool call to `audit_log`.

Deep agents use the same mechanism internally to stay within the context
window:

![Offloading large tool inputs to the filesystem](../assets/offloading-inputs.png)
![Offloading large tool results to the filesystem](../assets/offloading-results.png)
![Summarizing conversation history](../assets/langchain-summarization.png)

**Result:** `middleware defined: compliance_rules, audit_trail`

---

<h2 style="color: #7FC8FF;">Step 2: Update your agent</h2>

Your `create_deep_agent()` call from Challenge 4 is below, with two new
commented-out blocks.

![Pausing a tool call for human approval](../assets/deepAgentHITL.png)

> **Uncomment both blocks.** `middleware=[compliance_rules, audit_trail],`
> adds the rules and auditing. `interrupt_on={...}` pauses before
> `write_file` or `edit_file` runs.

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 3: Run it, then approve</h2>

Run the **"Run it"** cell. The agent pauses before `write_file`. Then run
**"Approve and continue"** to let it through.

Each cell prints a trace link first: the paused run, then the resumed run
with the approved `write_file`.

**Result:** `Approved!` followed by the agent's reply, then the audit log,
including a `write_file  success` entry.

---

<h2 style="color: #7FC8FF;">Step 4: Check</h2>

Click **Check**. It confirms in LangSmith that `write_file` to `/test.md` ran
after your approval, and that the compliance rules reached the model.

---

Next: Challenge 6 moves the system prompt into `AGENTS.md` and adds a skill.
