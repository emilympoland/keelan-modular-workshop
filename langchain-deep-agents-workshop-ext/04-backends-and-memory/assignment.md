---
slug: backends-and-memory
id: 3olo4ofzlhux
type: challenge
title: Backends & Memory
teaser: Add a persistent store, save a fact in one thread, and read it from another.
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
      <div class="sp-badge">Challenge 4</div>
      <p class="sp-title">04 - Backends &amp; <span>Memory</span></p>
      <p class="sp-sub">Give your agent memory that persists across threads.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Add a persistent store, then read a fact saved in one thread from another.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>CompositeBackend</b> — routes file paths to different backends, e.g. /memories/ to a persistent store while everything else stays ephemeral.</li>
          <li><b>StoreBackend</b> — backed by a LangGraph Store (SQLite here) and scoped by a namespace. Set it per user, per assistant, or shared.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>The Check reads the saved row from SQLite, so the model's claim alone doesn't pass.</p>
      </div>
    </div>
tabs:
- id: 7ntmcdlcaoth
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/backends-and-memory/tree/instruqt/04_backends_and_memory.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
`StateBackend` keeps files for one thread. `CompositeBackend` routes
`/memories/` to a `StoreBackend`, which persists across threads.

Requires the Tavily key from Challenge 1.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Read the backend definition</h2>

Run the setup cell, then the cell that builds `backend` and `store`. This cell
is the new piece in this challenge.

- **`open_memory_store()`** opens a local SQLite file as a LangGraph `Store`.
- **`CompositeBackend`** routes `/memories/` to that store; everything else
  stays in ephemeral `StateBackend`.
- **`namespace=lambda rt: ("memories", "shared")`** sets the namespace.
  Agents that use the same store and namespace share memories. Scope it per
  user for isolation.

**Result:** `backend and store ready`

---

<h2 style="color: #7FC8FF;">Step 2: Update your agent</h2>

Your `create_deep_agent()` call from Challenge 3 is below, with two new
commented-out lines.

> **Uncomment `backend=backend,` and `store=store,`.** **Then replace the
> `system_prompt` value** with this, so the agent reads and writes
> `/memories/`:
>
> ```python
> system_prompt=(
>     "You are a helpful assistant. Save important facts to /memories/ for future reference. "
>     "ALWAYS check /memories files before answering any questions to ensure you don't miss relevant information."
> ),
> ```

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 3: Save a memory, then check a different thread</h2>

Run the two thread cells in order. Thread 1 asks the agent to save a fact to
`/memories/preferences.md`. Thread 2 uses a new `thread_id`. It shares only
`/memories/` with Thread 1.

Each thread cell prints a trace link first. In Thread 2's trace, look for the
agent reading `/memories/`.

**Result (example):** `Thread 2: Your favorite programming language is Python…`

---

<h2 style="color: #7FC8FF;">Step 4: Check</h2>

Click **Check**. It reads the SQLite file and confirms the saved fact is in
the store.

---

Next: Challenge 5 adds custom middleware and human-in-the-loop approval.
