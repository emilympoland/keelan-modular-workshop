---
slug: custom-tools-and-subagents
id: dnlcve5g5he7
type: challenge
title: Custom Tools & Subagents
teaser: Add a web-search tool and a research subagent, then check the trace.
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
      <div class="sp-badge">Challenge 3</div>
      <p class="sp-title">03 - Custom Tools &amp; <span>Subagents</span></p>
      <p class="sp-sub">Give your agent a custom tool and a subagent to delegate to.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Uncomment two lines, run the agent, and look in the trace for web_search and any task calls.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Custom tool</b> — any @tool-decorated function, added alongside the built-ins via tools=[...].</li>
          <li><b>Subagent</b> — a separate agent the main one calls via task(). It runs in its own isolated context and returns only its final message.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>The trace shows whether the agent searched directly, delegated to the subagent, or answered from memory.</p>
      </div>
    </div>
tabs:
- id: di54oqqzihds
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/custom-tools-and-subagents/tree/instruqt/03_custom_tools_and_subagents.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
Tools let an agent reach the outside world. A subagent takes on a piece of
work in its own context and hands back only the answer, so long research
doesn't crowd the main agent.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Add the tool and subagent</h2>

The notebook defines `web_search` and a `research-agent` subagent. Uncomment
`tools=[...]` and `subagents=[...]` in the agent cell.

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 2: Run it</h2>

Run the research question. It can take a minute when the agent delegates.

**Result:** a summary with citations. Open the trace to see whether the agent
searched, delegated, or both.

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms your agent's run is in your traces. Without a LangSmith key, it reads a local trace log instead.

---

Next: Challenge 4 gives the agent memory that lasts between conversations.
