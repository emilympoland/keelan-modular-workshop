---
slug: tracing
id: rkjxniercvfn
type: challenge
title: Tracing
teaser: Generate traces with the research agent, then query them with list_runs and
  the filter DSL.
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
      <div class="sp-badge">Challenge 11</div>
      <p class="sp-title">11 - <span>Tracing</span></p>
      <p class="sp-sub">Generate traces, then query them from code.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Run the research agent three times, then read the traces back by time window and by latency.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Root run</b> — the top-level run of a trace. is_root=True returns these and skips their children.</li>
          <li><b>Filter DSL</b> — the expression language list_runs takes in filter=, e.g. gt(latency, 5).</li>
        </ul>
        <p class="sp-k">Requires</p>
        <p>The LangSmith key from Challenge 1.</p>
      </div>
    </div>
tabs:
- id: oedu6qxzvkz5
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/tracing/tree/instruqt/11_tracing.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: basic
timelimit: 0
enhanced_loading: null
---
Every run is traced automatically. Reading traces from code is how you find
slow or failing runs at scale, and it's the base for monitoring and evals.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Generate traces</h2>

Run the research agent three times.

**Result:** three short answers with their timings.

---

<h2 style="color: #7FC8FF;">Step 2: Query them</h2>

List recent runs, then uncomment `filter="gt(latency, 5)"` to return only slow
ones, and run the confirm cell.

**Result:** `Every run returned is slower than 5 seconds.`

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms your runs are in LangSmith and the filtered query passed.

---

Next: Challenge 12 scores the agent against a dataset.
