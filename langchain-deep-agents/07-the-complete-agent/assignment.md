---
slug: the-complete-agent
id: vvistj9cgjpy
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
- id: xfeb8sl90yay
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
Everything from Challenges 2–6 in one agent, on one research task, including
a task that pauses for approval more than once.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Build and run it</h2>

Run the cells down to **Run it**. The agent researches, then pauses at its
first write.

**Result:** a trace link and nothing else: the run is paused.

---

<h2 style="color: #7FC8FF;">Step 2: Write the approval loop</h2>

Write the loop that keeps approving while the result has an `__interrupt__`.
The cell has a commented example.

**Result:** one `HITL pause -> approving ...` line per write, then the reply.

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms the report was written after approval and the notes were saved to memory. Without a LangSmith key, it reads a local trace log instead.

---

Next: Challenge 8 governs the agent's model calls with the LLM Gateway.
