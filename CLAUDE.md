# CLAUDE.md (v12)

## Scope and Precedence

- Treat this file as a baseline. Follow the user's current request and the repository's own instructions when they are more specific.
- Scale the process to the risk. For reversible, low-risk work, make reasonable assumptions, state the important ones, and proceed.
- Use the project's existing planning, issue, and documentation systems. Do not introduce new tracking files when the project already has a system.

## Core Principles

- **Simplicity First** — Make every change as simple as possible. Prefer minimal, local changes.
- **Root Cause Thinking** — Always identify the root cause. Avoid temporary fixes or band-aids.
- **Minimal Impact** — Touch the fewest files possible. Avoid regressions. No unnecessary refactoring. Preserve existing architecture unless change is required.
- **Scope Discipline** — Do NOT rewrite working systems without strong reason. Do NOT introduce frameworks or major architecture changes without approval.

---

## Secrets and Environment Safety

- Never log, print, echo, or commit API keys, tokens, passwords, or secrets.
- Never hardcode credentials. Use the project's approved secret manager or environment-variable mechanism.
- For local development, use `.env` only when the project expects it and the file is excluded from version control.
- Before committing or sharing output, inspect the relevant diff or files for exposed secrets.

---

## Operating Modes

**Build Mode** — For feature requests, new functionality, or creation tasks:
Follow the Workflow (Sections 0–4), then apply relevant Standards (Sections 5–9) during implementation.

**Fix Mode** — For bug reports, errors, or broken behavior:
Investigate logs → rule out environment/dependency causes → identify root cause → implement fix → verify with evidence. Apply by default unless an Escalation Threshold (Section 8) is triggered.

---

## Workflow

These steps are sequential. Follow them in order for every non-trivial task.

### 0. Problem Specification

1. Restate the request in one sentence
2. Define the desired outcome
3. Identify constraints
4. Define success criteria (concrete and verifiable)

If an ambiguity would materially change the solution, scope, or risk, ask a clarifying question. Otherwise state a reasonable assumption and proceed.

### 1. Task Classification

- **Trivial** (1–2 steps) → execute immediately
- **Standard** (3–7 steps) → plan when it improves coordination or verification
- **Complex** (architecture changes or research) → research + plan

If unclear → ask clarifying questions.

### 2. Planning

For non-trivial tasks, create a plan when the work has dependencies, meaningful risk, or several verifiable stages. Include a clear objective, checkable steps, expected outputs, and material unknowns.

Compare candidate approaches when the choice matters, identify the relevant tradeoffs, and choose the simplest viable implementation. Establish success criteria before consequential changes.

If something goes sideways → **STOP and re-plan immediately.**

### 3. Assumptions

State material assumptions explicitly and validate them where practical. If a critical assumption is uncertain, ask the user. Do not burden routine work with an exhaustive assumption list.

### 4. Research Mode

Enter research mode for unfamiliar libraries, unclear errors, or missing documentation: search docs → review examples → validate assumptions → only then implement. Avoid guessing APIs or behavior.

---

## Standards

Not sequential. Apply whichever sections are relevant to the current task.

### 5. Investigation Rules

**Environment and dependency checks:** Before concluding the issue is in application code, rule out configuration, environment variables, dependency versions, build artifacts, service availability, and permissions.

**Exploration before fixing:**

1. Trace the execution path
2. Identify the failure point
3. Distinguish symptom from root cause
4. Check adjacent components, dependencies, and recent changes
5. When the cause is not obvious, form and test an alternative hypothesis
6. Only then implement

Do not patch the first suspicious line. If root cause is uncertain, keep investigating.

### 6. Before Modifying Code

**Impact analysis:** Identify which files will change, which modules depend on them, and what behaviors might break. Prefer the smallest safe change. If a change affects multiple systems, document the impact and verify each affected area.

**Interface contracts:** Before changing shared interfaces, signatures, schemas, or contracts: identify callers, check compatibility, prefer backward-compatible changes, and verify affected call sites. Do not change shared contracts unless necessary.

### 7. Uncertainty, Fallbacks, and Stop Conditions

**Low confidence:** State what is uncertain. Distinguish facts from hypotheses. Never present guesses as conclusions. Gather more evidence before high-impact changes.

**High-risk solution:** Prefer the smallest reversible fix. Document remaining risk. Preserve a rollback path. A fallback does not replace root-cause understanding unless explicitly accepted.

**Stop and re-evaluate if:** the same fix has failed twice, logs contradict expectations, the problem scope is expanding, or new evidence invalidates the hypothesis. When stopped: re-analyze and create a new plan. If uncertainty remains material, escalate to the user.

### 8. Escalation Thresholds

Pause and ask for guidance if: requirements conflict, multiple solutions have materially different tradeoffs, the fix requires architectural or schema changes, production or security risk is high, needed evidence is unavailable, or confidence remains low after investigation.

When escalating: summarize the problem, options, key tradeoffs, and what information is missing.

**If an escalation threshold is triggered, it overrides autonomous execution.**

### 9. Testing

Choose verification proportional to risk and scope: small change → targeted tests; shared logic → verify dependent paths; schema or interface change → verify callers; broad change → wider regression coverage.

If no test suite exists, verify by running the code and observing expected behavior.

---

## Completion

### Verification

Do not claim success based on code changes alone. A task is verified only with concrete evidence: passing tests (if they exist), successful runtime execution, a reproduced bug no longer reproducing, or expected logs/outputs/UI behavior.

Distinguish between **implemented**, **tested**, and **verified**.

**Final gate:** Do not mark a task complete unless the relevant code or checks run, behavior matches the specification, and no regressions are detected by the verification performed. State anything that remains untested.

> Would a staff engineer approve this change and the evidence supporting it?

### Reporting and Decision Logging

When presenting results, include: what was changed, which files were modified, why this approach was chosen, what evidence was observed, and what remains uncertain or unverified.

For longer changes, use the project's existing decision log or task tracker. If none exists and continuity would benefit, record what changed, why, and what remains risky in `tasks/todo.md`.

---

## Task Management

Use these files only when the project has adopted this task-management pattern:

- `tasks/todo.md` — Plans, progress tracking, and decision logs. Update at planning, during execution, and at completion.
- `tasks/status.md` — Current project state for session continuity. Update before ending a session when another session will rely on it.
- `tasks/lessons.md` — Reusable insights from past work. Update when something is learned that would help future sessions.

Workflow when adopted: plan in `todo.md` → verify plan matches spec → track progress during execution → document results → capture lessons → update `status.md` before ending session.

Avoid repeating large context blocks. Summarize intermediate findings. Keep long-term state in task files.
