---
slug: custom-tools-and-subagents
id: x6qgusxz6yoc
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
        <p>Uncomment two lines, run the agent, and check the trace for tavily_search and any task calls.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Custom tool</b> — any @tool-decorated function, added alongside the built-ins via tools=[...].</li>
          <li><b>Subagent</b> — a separate agent the main one calls via task(). It runs in its own isolated context and returns only its final message.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>The Check needs a tavily_search span with real results, so an answer from model memory fails.</p>
      </div>
    </div>
tabs:
- id: ljbi0d2y2bl3
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
Add a custom tool alongside the built-ins, then a subagent that runs in its
own isolated context. The main agent calls it through `task()` and receives
only its final message.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Read the tool and the subagent</h2>

Run the setup cell, then the two cells that define them:

- **`tavily_search`** — a plain `@tool`-decorated function. Its body calls
  `resilient_tavily_search`, which retries and falls back to a canned response
  if Tavily fails.
- **`research_subagent`** — a dict, not a function: `name`, `description`,
  its own `system_prompt`, and the `tools` it gets.

> ⚠️ This challenge needs a working Tavily key. If the search returns the
> canned fallback response, the Check fails.

**Result:** `tool: tavily_search` and `subagent: research-agent`

---

<h2 style="color: #7FC8FF;">Step 2: Update your agent</h2>

Your `create_deep_agent()` call from Challenge 2 is below, with a research
`system_prompt` and two commented-out lines.

> **Uncomment both.** `tools=[tavily_search]` gives the agent direct web
> search. `subagents=[research_subagent]` gives it the `research-agent` to
> delegate to.

![Subagents run in their own isolated context](../assets/deepAgentSubagents.png)

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 3: Run it</h2>

Run the **"Run it"** cell. It asks a question recent enough that the model
has to look it up. `run_name` labels the run in LangSmith so the Check can
find it.

> **This cell can take a minute or more.** A delegated run is a full agent run
> of its own.

The first line of output is a **View the trace** link. In the trace, look for
`tavily_search` calls and any `task` call to `research-agent`.

**Result (example):** a research summary with citations.

---

<h2 style="color: #7FC8FF;">Step 4: Check</h2>

Click **Check**. It finds the `custom-tools-and-subagents` run in LangSmith
and confirms it contains a `tavily_search` call with real results. The agent
can call `tavily_search` directly or through `task()`. The trace shows which.

---

Next: Challenge 4 adds a store so memory persists across threads.
