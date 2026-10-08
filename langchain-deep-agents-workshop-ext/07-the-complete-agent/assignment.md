---
slug: the-complete-agent
id: 14axkuj07dfc
type: challenge
title: The Complete Agent
teaser: 'All components together: tools, subagents, memory, middleware, HITL, AGENTS.md,
  and skills.'
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
      <div class="sp-badge">Challenge 7</div>
      <p class="sp-title">07 - The <span>Complete</span> Agent</p>
      <p class="sp-sub">All components from Challenges 2–6 in one agent.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Write the approval loop, then run one task that uses every component.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>__interrupt__</b> — the key in the result when HITL pauses. Each pause lists its action_requests.</li>
          <li><b>Approval loop</b> — a single 'if' handles one pause. A task with more than one write needs a 'while'.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>The Check needs the approved report write in your trace and the memory row in the store.</p>
      </div>
    </div>
tabs:
- id: fxc9jdpsuy9f
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/the-complete-agent/tree/instruqt/07_the_complete_agent.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
All components from Challenges 2–6 in one agent: tools, subagents, memory,
middleware, HITL, AGENTS.md, and skills. Every parameter is active.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Build the backend, store, and agent</h2>

Run the setup cell, then the backend/store cell, then the `create_deep_agent`
cell.

**Result:** `Complete agent created with:` followed by all six capabilities.

---

<h2 style="color: #7FC8FF;">Step 2: Run it</h2>

Run the **"Run it"** cell. The task asks for delegated research, a report at
`/final_report.md`, and notes at `/memories/research_notes.md`.
`interrupt_on` pauses both writes.

**Result:** a **View the trace** link, and nothing else. The run is paused at
a write. The trace shows the research up to that point, including any `task`
call to `research-agent`.

---

<h2 style="color: #7FC8FF;">Step 3: Write the approval loop yourself</h2>

**The next cell is blank.** This task writes at least two files, so a single
`if` like Challenge 5's may not clear every pause.

> **Write a `while result.get("__interrupt__")` loop.** Each pass: read
> `result["__interrupt__"][0].value["action_requests"]`, print each action's
> `name`, then resume with
> `Command(resume={"decisions": [{"type": "approve"} for _ in actions]})`.
> Keep looping until there's nothing left to approve.
>
> The cell has a commented example. Select its lines and press `Cmd+/`
> (`Ctrl+/` on Windows) to uncomment them.

💡 One pause can hold more than one action: HITL groups every gated call from
the same model turn.

**Result (example):** one `HITL pause -> approving write_file: ...` line per
approved write, then the final reply.

---

<h2 style="color: #7FC8FF;">Step 4: See everything together</h2>

Run the remaining cells: the files the agent wrote (seed files filtered
out), the audit log, and the recap table (one row per capability).

**Result:** `/final_report.md` in the file list, and a `write_file` entry (at
least) in the audit log.

---

<h2 style="color: #7FC8FF;">Step 5: Check</h2>

Click **Check**. It confirms in LangSmith that the approved `write_file` to
`/final_report.md` ran, and reads `/memories/research_notes.md` from the
SQLite store.

---

✅ Track complete. The agent uses custom tools, subagents, persistent memory,
middleware, human-in-the-loop approval, `AGENTS.md`, and skills.
