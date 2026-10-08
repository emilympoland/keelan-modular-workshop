---
slug: agents-and-skills
id: rzu2zkvhydnx
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
- id: ulnbx5fyvqum
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
`memory=["/AGENTS.md"]` loads an editable file into the system prompt on every
call. Here it takes over the role of the hardcoded `system_prompt`. Skills
load on demand: the agent sees each skill's name and description, and reads
the full `SKILL.md` when the task matches.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Read the identity and the skill</h2>

Run the setup cell, then the cell that defines two strings:

- **`agents_md`** — workflow steps, rules, and a pointer to `/skills/` for
  format instructions.
- **`linkedin_skill`** — format rules for one kind of output. The agent reads
  it only when the task calls for a LinkedIn post.

**Result:** `AGENTS.md and skill content ready`

---

<h2 style="color: #7FC8FF;">Step 2: Update your agent</h2>

Your `create_deep_agent()` call from Challenge 5 is below. `system_prompt` is
already commented out, and two new lines are commented out.

> **Uncomment `memory=["/AGENTS.md"]` and `skills=["/skills/"]`.**

💡 Dropping `system_prompt` also drops Challenge 4's instruction to save facts
to `/memories/`. This agent follows `AGENTS.md` instead.

**Result:** a repr of your compiled deep agent.

---

<h2 style="color: #7FC8FF;">Step 3: Run it</h2>

Run the **"Run it"** cell. The prompt asks for a LinkedIn post saved to
`/final_report.md`. The format rules come only from the skill.

> `interrupt_on` is still active from Challenge 5. This cell approves the
> first paused write automatically.

The cell prints a trace link for each run: the first run, and the resumed one
if a write paused.

**Result (example):** a LinkedIn post with a bold hook, short paragraphs, and
3–5 hashtags. In the first trace, a `read_file` call on
`/skills/linkedin-post/SKILL.md` shows the agent loaded the skill.

---

<h2 style="color: #7FC8FF;">Step 4: Check</h2>

Click **Check**. It confirms in LangSmith that the agent wrote
`/final_report.md`, and that your `AGENTS.md` and the skill list reached the
model.

---

Next: Challenge 7 runs the complete agent with every component from
Challenges 2–6.
