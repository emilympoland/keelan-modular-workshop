---
slug: environment-evals
id: kvchwtbq8tkf
type: challenge
title: Environment Evals
teaser: Grade the files an agent leaves behind, including re-running the code it wrote.
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
      <div class="sp-badge">Challenge 13</div>
      <p class="sp-title">13 - Environment <span>Evals</span></p>
      <p class="sp-sub">Run the agent in a sandbox and grade what it leaves behind.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Run a report agent in a scratch workspace, grade its files with a verifier, and optionally run the full Harbor job.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Verifier</b> — a script that grades the agent's files and returns a reward. Here it also re-runs the agent's own sources.py.</li>
          <li><b>Harbor</b> — runs each task in an isolated environment, then runs its verifier. --env langsmith uses LangSmith Sandboxes.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>Re-computing the answer catches errors the finished report alone doesn't show.</p>
      </div>
    </div>
tabs:
- id: 8nvhwfmakbqg
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/environment-evals/tree/instruqt/13_environment_evals.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
Some work can only be judged by its results. The agent writes a report and a
script, and a verifier grades the files, including re-running the script. It's
an exact score with no LLM judge.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Run the agent</h2>

Run the agent in a scratch workspace.

**Result:** a three-paragraph report and a `sources.py`.

---

<h2 style="color: #7FC8FF;">Step 2: Grade it</h2>

Run the verifier. It deletes the report as part of grading, so re-run step 1
before grading again.

**Result:** seven scores and a `reward` between 0 and 1.

---

<h2 style="color: #7FC8FF;">Step 3: Harbor (optional)</h2>

Set `RUN_HARBOR = True` to run all four tasks in LangSmith Sandboxes. Needs
Sandboxes enabled for your workspace.

---

<h2 style="color: #7FC8FF;">Step 4: Check</h2>

Click **Check**. It confirms the agent ran and the verifier scored it.

---

Next: Challenge 14 scores new traces automatically.
