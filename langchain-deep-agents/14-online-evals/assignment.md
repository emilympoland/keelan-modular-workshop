---
slug: online-evals
id: cyeriytbygl2
type: challenge
title: Online Evals & Annotation Queues
teaser: Score every new trace with a run rule, then route scored runs to an annotation
  queue.
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
      <div class="sp-badge">Challenge 14</div>
      <p class="sp-title">14 - Online <span>Evals</span></p>
      <p class="sp-sub">Score every new trace automatically, then route runs for human review.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Create an online eval rule and a routing rule, then trigger both with new traces.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Run rule</b> — an automation on a tracing project: score each new run with a judge, or route matching runs to a queue.</li>
          <li><b>Annotation queue</b> — a list of runs waiting for a person to review and label.</li>
        </ul>
        <p class="sp-k">Requires</p>
        <p>The LangSmith key from Challenge 1. Scores need an OPENAI_API_KEY secret in your workspace.</p>
      </div>
    </div>
tabs:
- id: xcm84nfdl4w3
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/online-evals/tree/instruqt/14_online_evals.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
Online evals score real traffic as it arrives, and an annotation queue sends
the runs worth a closer look to a person. Scores need an `OPENAI_API_KEY`
secret in your LangSmith workspace.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Create the rules</h2>

Uncomment `filter="eq(is_root, true)"` so the judge scores whole traces, not
every step. Then create the queue and the rule that routes to it.

**Result:** two rule IDs and a queue name.

---

<h2 style="color: #7FC8FF;">Step 2: Trigger them</h2>

Run the trigger cell. It waits 100 seconds, then runs three traces.

**Result:** three replies and links to both rules and the queue.

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms both rules exist and the judge scores whole traces. Then run the clean-up cell.

---

✅ Track complete.
