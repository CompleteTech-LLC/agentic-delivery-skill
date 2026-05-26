---
name: agentic-delivery-skill
description: >-
  Create CompleteTech LLC delivery execution artifacts for approved agentic development engagements, including kickoff agendas, access checklists, project plans, milestone trackers, status updates, decision logs, risk/issue logs, change request intake, prototype review, evaluation reports, acceptance packets, launch readiness, monitoring, support, handoff, runbooks, quickstarts, closeout, post-launch review, and escalation procedures. Use after proposal/SOW or contract approval when Codex needs to run bounded agentic workflow delivery cleanly.
version: 1.0.4
metadata:
  openclaw:
    skillKey: agentic-delivery-skill
    homepage: https://github.com/CompleteTech-LLC/agentic-delivery-skill
    requires:
      bins:
        - python3
    install:
      - kind: uv
        package: reportlab==4.5.1
      - kind: uv
        package: pypdfium2==5.8.0
      - kind: uv
        package: pillow==12.2.0
      - kind: uv
        package: pyyaml==6.0.3
---

# Agentic Delivery Skill

## Purpose

Create delivery execution artifacts for CompleteTech LLC agentic development engagements after a proposal/SOW or contract is approved.

## System Boundary

This skill owns execution after commercial approval: kickoff, project control, evaluation, launch preparation, handoff, and support artifacts. Use `agentic-proposal-skill` for unapproved commercial scope, `agentic-contract-skill` for legal agreement artifacts, `agentic-security-review-skill` for security or production-readiness review, `agentic-invoice-skill` for billing, `agentic-customer-success-skill` for relationship health and renewal, and `agentic-case-study-skill` only after outcomes are verified and approved for proof.

## Core Workflow

1. Identify the delivery need: kickoff, access, planning, status, decisions, risk/issue, change request, prototype review, evaluation, acceptance, launch, monitoring, support, handoff, closeout, or escalation.
2. Gather verified facts: approved scope, workflow, owners, timeline, milestones, systems, approval gates, evaluation examples, risks, dependencies, support expectations, and acceptance criteria.
3. Use `references/use-case-decision-table.md` to choose the right delivery artifact.
4. Use `references/delivery-positioning.md` for CompleteTech LLC delivery framing and guardrails.
5. Use `references/delivery-catalog.md` for the near-exhaustive delivery artifact library.
6. Keep delivery practical and bounded. Do not fabricate client facts, approvals, test results, metrics, regulated-use assurances, legal claims, or production readiness.

## Artifact Selection Guide

- First meeting after signature: use `kickoff-agenda`.
- Need client systems/docs/API access: use `client-access-checklist`.
- Need execution structure: use `project-plan`.
- Tracking milestone progress: use `milestone-tracker`.
- Regular client update: use `weekly-status-update`.
- Capturing decisions: use `decision-log`.
- Capturing risks or active issues: use `risk-and-issue-log`.
- Scope change request: use `change-request-intake`.
- Blocking prerequisites: use `dependency-tracker`.
- Communication cadence: use `stakeholder-communication-plan`.
- Prototype review: use `prototype-review-checklist`.
- Evaluation run: use `evaluation-run-report`.
- Test summary: use `test-results-summary`.
- Formal acceptance: use `acceptance-review-packet`.
- Launch preparation: use `launch-readiness-checklist`.
- Observability requirements: use `monitoring-plan`.
- Post-handoff help: use `support-plan`.
- Handoff preparation: use `handoff-checklist`.
- Admin/operator documentation: use `administrator-runbook`.
- Reviewer or user training: use `user-reviewer-quickstart`.
- After launch: use `post-launch-review`.
- Retrospective: use `lessons-learned`.
- Project close: use `closeout-summary`.
- Incoming support request: use `support-ticket-intake`.
- Escalation path: use `escalation-procedure`.
- Deployment-specific rollout: use `deployment-runbook`.
- Acceptance defects: use `defect-remediation-plan`.
- Client training session: use `training-session-plan`.

When several artifacts fit, choose the one closest to the operational event. Do not mark launch-ready, accepted, or complete unless the verified evidence supports it.

## Quality Rules

- Execute the approved scope; route new scope into change request intake.
- Protect human approval gates for external communications, production changes, purchases, data export, and material business decisions.
- Track decisions, risks, issues, dependencies, and acceptance evidence explicitly.
- Verify evaluation examples before acceptance.
- Document logs, monitoring, runbooks, quickstarts, support, and handoff.
- Use `TBD` or open questions for unknowns.

## Resource Guide

- `references/delivery-positioning.md`: load for CompleteTech LLC delivery language and boundaries.
- `references/use-case-decision-table.md`: load when choosing a delivery artifact.
- `references/delivery-lifecycle.md`: load for flow from kickoff through support and closeout.
- `references/delivery-catalog.md`: load for the near-exhaustive delivery template library.
- `references/template-index.json`: machine-readable template metadata used by the renderer.
- `scripts/render_delivery.py`: list delivery artifacts or render a draft with placeholders.

## Runtime Permissions

This skill needs local filesystem access only for its documented renderer workflow:

- Reads bundled templates, references, examples, `assets/logo.png`, and user-provided Markdown or variable inputs.
- Writes only to the user-selected `--out`, `--png`, `--markdown-out`, or default `output/` artifact paths.
- Runs local Python entry points `scripts/render_delivery.py` and `scripts/render_pdf.py`.
- Does not require network access, credential access, persistence, privilege escalation, or destructive file operations.

## Renderer

```bash
python3 scripts/render_delivery.py --list
python3 scripts/render_delivery.py --stage status --list
python3 scripts/render_delivery.py --template kickoff-agenda --var client_name=Acme --var workflow="support triage"
```

Rendered artifacts are drafts. Replace placeholders with verified project facts before sending or storing them.

## Rendering to a Branded PDF

Artifacts from this skill are delivered as branded CompleteTech LLC **PDF** documents. The renderer can emit PDF, Markdown, and optional PNG preview in one local command:

```bash
pip install -r requirements.txt
python3 scripts/render_delivery.py --template launch-readiness-checklist \
  --out artifact.pdf --png artifact.png \
  --title "Launch Readiness Checklist" --doc-type "DELIVERY ARTIFACT" \
  --subtitle "Northwind Trading Co. — Support Email Triage Agent (Pilot)" --meta "DOCUMENT NO.=DEL-2026-0233" --meta "DATE=2026-06-12" \
  --var client_name="Client Name" --var workflow="support triage"
```

| Output Need | Use |
|---|---|
| Branded PDF | `--out artifact.pdf` |
| PNG preview | `--png artifact.png` |
| Markdown source | `--markdown-out artifact.md` |
| Markdown only | `--no-pdf` |
| No cover page | `--no-cover` |
| Existing delivery Markdown to PDF | `python3 scripts/render_pdf.py --markdown artifact.md --out artifact.pdf --logo assets/logo.png --title "Launch Readiness Checklist" --doc-type "DELIVERY ARTIFACT"` |

| Rendering Support | Details |
|---|---|
| Markdown subset | `#`, `##`, `###`, paragraphs, `-` bullets, tables, `>` callouts, `**bold**`, and `[PAGE_BREAK]`. |
| Required package | `reportlab==4.5.1` for PDF rendering. |
| Optional preview packages | `pypdfium2==5.8.0` and `pillow==12.2.0` for `--png`. |
| Example output | See `assets/examples/` for rendered Markdown, PDF, and PNG artifacts. |

## Network Boundary

| Boundary | Requirement |
|---|---|
| Local-only runtime | No outbound network helpers, callbacks, telemetry, receipt helpers, or delivery-run metadata posting. |
| External actions | Does not deploy, launch, send, publish, or call project-management systems. |
| Approval-sensitive work | Launch, production, customer send, and acceptance actions require verified approval outside this renderer. |
