---
slug: agents-and-skills
id: wzebybu2ozzq
type: challenge
title: Agents & Skills
teaser: Move the system prompt into an editable AGENTS.md file, and load a skill on
  demand.
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
      <div class="sp-badge">Challenge 6</div>
      <p class="sp-title">06 - Agents &amp; <span>Skills</span></p>
      <p class="sp-sub">Move the system prompt into an editable AGENTS.md file, and load a skill on demand.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Replace system_prompt with AGENTS.md, add a skill, and run the agent.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>AGENTS.md</b> — an editable file loaded into the system prompt via memory=[...].</li>
          <li><b>Skill</b> — instructions in /skills/. The agent sees each skill's description and reads the full file when the task matches.</li>
        </ul>
        <p class="sp-k">Why it matters</p>
        <p>The Check needs the report write, with AGENTS.md and the skill list in the model's prompt.</p>
      </div>
    </div>
tabs:
- id: 39oni0hcbv8g
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/agents-and-skills/tree/instruqt/06_agents_and_skills.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
Move the agent's instructions into an editable `AGENTS.md`, and add a skill it
loads only when a task needs it. Instructions change without code changes, and
new abilities don't grow every prompt.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Load AGENTS.md and the skill</h2>

Uncomment `memory=["/AGENTS.md"]` and `skills=["/skills/"]`.

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 2: Run it</h2>

Ask for a LinkedIn post. The format rules come only from the skill.

**Result (example):** a post with a bold hook and 3–5 hashtags. The trace shows
the agent reading the skill file.

---

<h2 style="color: #7FC8FF;">Step 3: Check</h2>

Click **Check**. It confirms the post was written with `AGENTS.md` and the skill list in the prompt. Without a LangSmith key, it reads a local trace log instead.

---

Next: Challenge 7 puts every component together.
