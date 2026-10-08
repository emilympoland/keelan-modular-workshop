---
slug: your-first-deep-agent
id: j07nwgucaiqg
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
- id: 9vmrxtnkihef
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
`create_agent()` runs a model and the tools you pass it in a loop. It has no
built-in tools. `create_deep_agent()` adds middleware for filesystem tools,
planning, and subagents.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Run the setup cell</h2>

Imports the model and LangGraph helpers, and deletes any memory store left
from a previous run. Challenge 4 builds that store.

**Result:** `Ready`

---

<h2 style="color: #7FC8FF;">Step 2: Run the plain agent</h2>

Run the cell under **"Try the plain agent first."** It asks a bare
`create_agent()` to write a haiku to `/haiku.txt`, then read it back.

**Result:** the model replies in text, but prints `files in result: None`.
`create_agent()` has only the tools you pass it, and this call passes none.

Open the **View the trace** link at the end of the output. The trace shows one
model call and no tool calls. `run_id` in the `invoke()` config names the
trace, so the link points straight at it.

---

<h2 style="color: #7FC8FF;">Step 3: Write the deep agent</h2>

Read **1.1 Your First Deep Agent**. It lists the built-in tools and
middleware: filesystem tools, `write_todos`, `task()`, and more.

![create_deep_agent() built-in capabilities](../assets/deepAgentsDiag.png)

The next cell is blank. **Paste in the code from the instructions above it**:

```python
from deepagents import create_deep_agent

agent = create_deep_agent(
    model=model,
    system_prompt="You are a helpful assistant.",
    checkpointer=MemorySaver(),
)
agent
```

> **You write only this cell.**

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 4: Give it the same task</h2>

Run the remaining cells in order. The agent writes `/haiku.txt`, the next cell
prints its virtual filesystem, and the agent reads the file back in the same
thread.

Open the trace link after the first run. Compare it with the plain agent's
trace: this one includes filesystem tool calls, such as `write_file`.

The last cell starts a new thread, which can't see the file. `StateBackend`,
the default, keeps files for one thread only.

---

<h2 style="color: #7FC8FF;">Step 5: Check</h2>

Click **Check**. It finds the agent's `write_file` call to `/haiku.txt` in
your LangSmith traces.

---

Next: Challenge 3 adds a custom tool and a subagent.
