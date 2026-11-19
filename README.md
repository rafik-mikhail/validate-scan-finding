# Validate Scan Finding

An agent skill that validates one existing finding of any category against authorized evidence and returns one disposition: `confirmed`, `refuted`, `dependent`, or `unresolved`. SAST findings route through [Prepare Scan Slice](https://github.com/rafik-mikhail/prepare-scan-slice) and [Investigate Scan Issue](https://github.com/rafik-mikhail/investigate-scan-issue). SCA and container findings use the VEX not-affected justifications; IaC, secrets, and DAST have their own routes.

## Use the skill

Use [`SKILL.md`](SKILL.md) in your agent’s skill directory under `validate-scan-finding/`. Provide one finding and the evidence its category needs: source, lockfiles, IaC templates or a rendered plan, image metadata, or recorded DAST traffic. Run it once per finding in queue order. The review is read-only: no scans, code execution, live requests, credential use, or fixes.

## Use as agent instructions

Copy the [Validate Scan Finding instruction gist](https://gist.github.com/rafik-mikhail/2cde28616758b39962b8662bd6a21383) into your agent’s instruction file. The gist contains the same guidance with `applyTo: "**"` front matter for tools that support it. If your agent accepts plain Markdown instructions, copy the body without the front matter.

## Where it fits

This skill is step 5 of a seven-step workflow that takes a received scan report to fix pull requests. Steps 1–4 work on the whole report, steps 5–6 work on one finding at a time, and step 7 works on one batch at a time.

| Step | Skill | Works on |
|---|---|---|
| 1 | [Normalize Scan Report](https://github.com/rafik-mikhail/normalize-scan-report) | whole report |
| 2 | [Deduplicate Scan Findings](https://github.com/rafik-mikhail/deduplicate-scan-findings) | whole report |
| 3 | [Group Findings by Root Cause](https://github.com/rafik-mikhail/group-findings-by-root-cause) | whole report |
| 4 | [Prioritize Scan Findings](https://github.com/rafik-mikhail/prioritize-scan-findings) | whole report, then again for confirmed findings |
| 5 | **Validate Scan Finding** | one finding at a time |
| 6 | [Decide Finding Action](https://github.com/rafik-mikhail/decide-finding-action) | one finding at a time |
| 7 | [Batch Finding Fixes](https://github.com/rafik-mikhail/batch-finding-fixes) | one batch at a time |

Verification by rescan and closing with regression monitoring are not covered yet.

## How to test

Follow the [evaluation guide](TESTING.md).

## Notes

[NOTES.md](NOTES.md) covers gotchas, areas for improvement, and the sources behind the guidance.
