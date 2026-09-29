---
description: Plan -> Implement -> Audit one step of the Group 13 assignment
argument-hint: <task, e.g. "Q7 pipeline ingest and clean filters">
---

Task: $ARGUMENTS

Follow `CLAUDE.md` Section 4 and `AGENTS.md` (one step at a time, short replies):

1. **Plan.** Read `CLAUDE.md`, `AGENTS.md` and the relevant files. Produce the PLAN block from `.claude/agents/implementer.md`. Show it to me and wait for my OK.
2. **Implement.** Delegate to the `implementer` subagent with the approved plan. If its report is not "Ready for audit: yes", stop and tell me why.
3. **Audit.** Delegate to the `business-auditor` subagent. Give it only: the list of changed files/cells, the rules in scope, and the implementer's report as claims to verify. Do not pass your own reasoning.
4. **Fix loop.** On FAIL, send every BLOCKER and MAJOR back to the `implementer`, then re-audit. Maximum 3 rounds, then stop and show me the open findings.
5. **Colab.** If anything needs running, tell me exactly which cells to run and ask me to upload the notebook with outputs. Re-run the `business-auditor` on the real outputs.
6. **Finish.** Show the General Instructions & Submission Guidelines table from the audit (always), the final audit verdict, files changed, and any QUESTION items. Update the progress table in `AGENTS.md` only after I confirm. Do not commit unless I ask.
