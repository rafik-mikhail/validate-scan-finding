# Notes: Validate scan finding

## Gotchas

- **This skill is a router for SAST.** For SAST it defers to [Prepare Scan Slice](https://github.com/rafik-mikhail/prepare-scan-slice) and [Investigate Scan Issue](https://github.com/rafik-mikhail/investigate-scan-issue) and keeps the same four dispositions.
- **Reachability tools are not proof.** Vendors report large noise reduction from reachability (Socket cites up to 90% with full-application analysis), but function-level reachability still cannot tell whether a reachable function is exploitable, and missing call sites can hide behind reflection or plugins. CISA's guidance says not to assume a single execution path.
- **Lockfile, not manifest.** A manifest range can include an affected version that the lockfile does not install, and the reverse.
- **Intent is not a false positive.** A public bucket on purpose is a true finding with an accepted risk. Keeping these apart keeps the false-positive rate honest and the risk register complete.
- **Do not test secrets.** Trying a credential is an access attempt. Use the tool's validity check only if the owner authorized it.
- **LLM triage works best with structure.** Research such as ZeroFalse reports that giving the model flow traces and CWE-specific guidance beats generic prompts. Semgrep reports users agree with its assistant's triage more than 96% of the time, which still leaves errors a human must catch.

## Areas for improvement

- Add CWE-specific checklists (injection, path traversal, deserialization, SSRF) as references.
- Add an IaC variable-resolution helper that reads a rendered plan when available.
- Add an output in OpenVEX or CycloneDX VEX for SCA results.
- Add DAST and container routes in more depth.

## Sources

- [CISA: VEX status justifications](https://www.cisa.gov/sites/default/files/publications/VEX_Status_Justification_Jun22.pdf) and [Minimum requirements for VEX](https://www.cisa.gov/sites/default/files/2023-04/minimum-requirements-for-vex-508c.pdf): the five not-affected justifications.
- [Semgrep: reachability analysis white paper](https://semgrep.dev/assets/content/whitepapers/semgrep-reachabilityanalysis-whitepaper-1225.pdf): dependency, function, and data-flow reachability limits.
- [Socket: reachability analysis](https://docs.socket.dev/docs/reachability-analysis): tiers of reachability.
- [ZeroFalse (arXiv 2510.02534)](https://arxiv.org/abs/2510.02534): LLM adjudication of static-analysis alerts with traces and CWE knowledge.
- [Semgrep: Assistant triage results](https://semgrep.dev/blog/2025/semgrep-is-confidently-handling-60-of-all-triage-for-users-without-reducing-coverage/).
- [Checkov: suppressing and skipping policies](https://www.checkov.io/2.Basics/Suppressing%20and%20Skipping%20Policies.html): intent recorded as a justified skip.
