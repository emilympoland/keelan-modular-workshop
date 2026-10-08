---
slug: backends-and-memory
id: fgvwyotaxsui
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
- id: kniy6k0is0zd
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
An agent's files normally last for one conversation. Routing `/memories/` to
a persistent store lets it save a fact in one thread and recall it in
another.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Wire in the store</h2>

Uncomment `backend=` and `store=`, and swap in the `system_prompt` shown above
the cell so the agent uses `/memories/`.

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 2: Save, then recall</h2>

Run the two thread cells. The first saves a fact; the second, a new thread,
recalls it.

**Result (example):** `Thread 2: Your favorite programming language is Python…`

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It reads the store and confirms the fact was saved.

---

Next: Challenge 5 adds rules, auditing, and human approval.
