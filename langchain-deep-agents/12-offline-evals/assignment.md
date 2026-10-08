---
slug: offline-evals
id: 6wbemcljepke
type: challenge
title: Offline Evals
teaser: Score the research agent against a dataset with an LLM judge and a trajectory
  evaluator.
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
      <div class="sp-badge">Challenge 12</div>
      <p class="sp-title">12 - Offline <span>Evals</span></p>
      <p class="sp-sub">Score the agent against a fixed dataset, two ways.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Build a dataset, run a final-response experiment with an LLM judge, then a trajectory experiment with code evaluators.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Experiment</b> — one run of a target function over a dataset, with each evaluator's score attached as feedback.</li>
          <li><b>Trajectory eval</b> — scores the sequence of tool calls, not just the final answer.</li>
        </ul>
        <p class="sp-k">Requires</p>
        <p>The LangSmith key from Challenge 1. The two experiments take a few minutes each.</p>
      </div>
    </div>
tabs:
- id: 48tnrf513hfn
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/offline-evals/tree/instruqt/12_offline_evals.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
Score the agent over a fixed dataset, so prompt and model changes become
numbers you can compare. Scoring the tools it called catches right answers
reached the wrong way.

![Dataset examples run through the application, then scored by evaluators](../assets/evals-conceptual.png)

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Judge the final answers</h2>

![Final-response evaluation: compare the answer with a reference](../assets/final-response.png)

Build the dataset and run an experiment scored by an LLM judge. It takes a few
minutes.

**Result:** `Experiment: final-response-...`

---

<h2 style="color: #7FC8FF;">Step 2: Score the trajectory</h2>

![Trajectory evaluation: compare the steps taken with a reference](../assets/trajectory.png)

Uncomment `evaluators=[...]` in the trajectory eval and run it.

**Result:** `Experiment: trajectory-...`. Compare both in **Datasets &
Experiments**.

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms your dataset has a scored trajectory experiment.

---

Next: Challenge 13 grades an agent by the files it leaves behind.
