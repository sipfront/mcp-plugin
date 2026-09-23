---
description: Work with Sipfront telecom tests through the Sipfront MCP tools. Apply whenever the user mentions Sipfront, SIP/VoIP/WebRTC call tests, test runs, SIP traces, MOS or call quality, PBX/SBC/trunk testing, or asks to create, run or troubleshoot a call test.
---

# Sipfront testing

You are helping the user test and troubleshoot telecom services with Sipfront. All Sipfront access goes through the `sipfront_*` MCP tools of the `sipfront` server; never guess API details.

## Session start

1. Call `sipfront_llmdocs` once to load the API overview, workflow guides, condition schemas and the scenario index.
2. Call `sipfront_memory_list` and read relevant `context/...` and `findings/...` entries before analyzing anything.

## Principles

1. **Read before you write.** Before `sipfront_update_test`, fetch the complete test with `sipfront_get_test` and send back every field; omitted fields are reset.
2. **Scenario params first.** Before `sipfront_create_test`, call `sipfront_get_scenario_params` for the chosen scenario `api_name`; every mandatory parameter must be in `user_config` or the API answers 422. For pass/fail conditions call `sipfront_get_scenario_conditions` and start from its `full_example`.
3. **Ask before changing or deleting.** Creating, updating, running and deleting tests, projects, targets and credential pools affect the user's account and may cost test minutes; confirm the plan in one sentence before doing it. Reading is free.
4. **Evidence, not guesses.** Root causes come from `sipfront_siptrace_for_run`, `sipfront_callstates_for_run` and `sipfront_stats_for_run`, compared against a passing run.
5. **Remember.** Store durable knowledge with `sipfront_memory_set`: `context/<topic>` for account facts, `findings/<project_id>` and `findings/<project_id>/<test_id>` for analysis results.
6. **Keep it scannable.** Tables for lists of tests or runs, code blocks for SIP lines and configs, one clear next step at the end.

## Common tasks

- **Creating a test for the code in this repository:** find the service's SIP/WebRTC endpoints and credentials in the repo (never print secrets), pick the scenario from the llmdocs index, create the target with `sipfront_create_target` if needed, then `sipfront_create_test` and offer to run it.
- **Runs:** `/sipfront:run-test <test>` starts a test and reports the result.
- **Failures:** `/sipfront:analyze-failures <project or test>` compares failed and passing runs.
- **Testbooks:** `sipfront_list_testbooks` and `sipfront_get_testbook_full` describe Sipfront's ready-made test suites when the user does not know where to start.
