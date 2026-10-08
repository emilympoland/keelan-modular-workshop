---
slug: prompts
id: xxdkmjrxadvg
type: challenge
title: Prompts
teaser: Push a prompt to the LangSmith Prompt Hub, run it as an agent, and version
  it.
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
      <div class="sp-badge">Challenge 10</div>
      <p class="sp-title">10 - <span>Prompts</span></p>
      <p class="sp-sub">Author, version, and share prompts in LangSmith, then pull them into code.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Push a system prompt, pull it into a create_agent agent, then push a second version.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>Prompt Hub</b> — every saved prompt with its commit history, tags, and environments.</li>
          <li><b>Commit</b> — one saved version. Pushing under the same name adds a commit; earlier ones stay in the history.</li>
        </ul>
        <p class="sp-k">Requires</p>
        <p>The LangSmith key from Challenge 1.</p>
      </div>
    </div>
tabs:
- id: eyh8jx3ditpn
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/prompts/tree/instruqt/10_prompts.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: basic
timelimit: 0
enhanced_loading: null
---
Keep prompts in LangSmith instead of your code, so you can edit, compare, and
roll them back without redeploying.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Push and run a prompt</h2>

Push a system prompt to the Prompt Hub, then pull it into an agent.

**Result:** a prompt URL and a client briefing.

---

<h2 style="color: #7FC8FF;">Step 2: Version it</h2>

Uncomment the push of `prompt_v2`. Open the prompt page and toggle **Diff** to
compare.

**Result:** `New commit: https://...`

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms two commits, the latest being `prompt_v2`.

---

Next: Challenge 11 queries your traces from code.
