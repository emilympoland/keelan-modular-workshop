---
slug: package-and-deploy
id: ab29iek20bvh
type: challenge
title: Package & Deploy
teaser: Validate the deployable project, build its graph the way the server does,
  and optionally deploy it.
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
      <div class="sp-badge">Challenge 9</div>
      <p class="sp-title">09 - Package &amp; <span>Deploy</span></p>
      <p class="sp-sub">Package the deep agent so LangSmith Deployments can serve it.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Inspect the deployable project, validate langgraph.json, build the graph from its factory, and optionally deploy.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>langgraph.json</b> — the deploy config. It registers each graph by module path and lists dependencies.</li>
          <li><b>Graph factory</b> — a function the server calls to build the graph, so an assistant can pin runtime config.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>The Check runs validate and builds every registered graph, because validate alone only checks the config schema.</p>
      </div>
    </div>
tabs:
- id: sms8bpbqazmw
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/package-and-deploy/tree/instruqt/09_package_and_deploy.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
`langgraph.json` tells LangSmith Deployments which agent to serve.
`langgraph validate` only checks the file's format, so you'll also build the
agent the way the server does, which catches a broken path before a deploy
does.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Validate the config</h2>

Look over the project files, then run `langgraph validate`.

**Result:** `Configuration file ... is valid. (1 graph found)`

---

<h2 style="color: #7FC8FF;">Step 2: Build the graph</h2>

Uncomment the three lines that import the `agent` factory and build it, then
run the confirm cell.

**Result:** `langgraph.json validates and the graph builds.`

---

<h2 style="color: #7FC8FF;">Step 3: Deploy (optional)</h2>

Set `DEPLOY = True` to deploy. It needs a service key (`lsv2_sk_...`) and
creates a real deployment; delete it when you're done.

---

<h2 style="color: #7FC8FF;">Step 4: Check</h2>

Click **Check**. It validates the config and builds the graph itself.

---

Next: Challenge 10 versions a prompt in LangSmith.
