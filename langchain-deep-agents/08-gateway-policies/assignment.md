---
slug: gateway-policies
id: iacxbyya3aya
type: challenge
title: Gateway Policies
teaser: Put a workspace policy in front of every model call with the LangSmith LLM
  Gateway.
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
      <div class="sp-badge">Challenge 8</div>
      <p class="sp-title">08 - Gateway <span>Policies</span></p>
      <p class="sp-sub">Put a workspace policy in front of every model call with the LangSmith LLM Gateway.</p>
      <div class="sp-card">
        <p class="sp-k">What you'll do</p>
        <p>Create a PII/secrets policy, send traffic through the gateway, compare traces, then delete the policy.</p>
        <p class="sp-k">Key concepts</p>
        <ul>
          <li><b>LLM Gateway</b> — a managed proxy between a model client and the provider. Opt in by changing the client's base_url.</li>
          <li><b>Gateway policy</b> — a workspace-level rule (PII redaction, allow-lists, cost caps) applied to every call through the gateway.</li>
        </ul>
        <p class="sp-k">Requires</p>
        <p>The LangSmith key from Challenge 1, the LLM Gateway enabled for your organization, and workspace-admin rights. Optional and not graded.</p>
      </div>
    </div>
tabs:
- id: x1rcetfwpvmm
  title: Notebook
  type: service
  hostname: workstation
  path: /lab/workspaces/gateway-policies/tree/instruqt/08_gateway_policies.ipynb
  port: 8888
  custom_response_headers:
  - key: Content-Security-Policy
    value: ""
difficulty: intermediate
timelimit: 0
enhanced_loading: null
---
The LLM Gateway sits between your agents and the model provider. A policy on
it, like blocking personal data, covers every request in the workspace with no
code changes.

> **Optional, not graded.** Needs the Gateway enabled for your organization
> and admin rights. Without them the cells say so, and you can move on.

[button label="Notebook" background="#7FC8FF" color="#030710"](tab-0)

---

<h2 style="color: #7FC8FF;">Step 1: Create a policy</h2>

Create a PII/secrets policy for your workspace.

**Result:** `Policy created.`, or the reason it wasn't.

---

<h2 style="color: #7FC8FF;">Step 2: Send traffic through the gateway</h2>

Send a clean prompt, then one containing an email, and compare their traces.

**Result:** two replies and a trace link.

---

<h2 style="color: #7FC8FF;">Step 3: Delete the policy</h2>

Run the last cell. The policy applies to everyone in your workspace.

**Result:** `Policy deleted.`

---

<h2 style="color: #7FC8FF;">Step 4: Check</h2>

Not graded. Click **Check** to continue.

---

Next: Challenge 9 packages the agent for deployment.
