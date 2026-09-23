---
description: Run a Sipfront test now, wait for it and report the outcome
argument-hint: test name or ID (optionally "in <project>")
---

Run the Sipfront test given in $ARGUMENTS, wait for the result and summarize it.

## Steps

1. **Resolve the test.** Call `sipfront_list_projects` and `sipfront_list_tests` to find the test by name or ID. If several match, list them with project names and ask which one. If the user names a project instead of a test, offer `sipfront_run_project` for all its tests.
2. **Start it.** Call `sipfront_run_test` with the test ID and keep the returned `session_uuid`.
3. **Wait.** Call `sipfront_wait_for_run_finished` with that `session_uuid` (default timeout 600 s). If it times out, tell the user the run is still going and how to check it later with `sipfront_get_run_status`.
4. **Report.** State pass/fail, duration and the link or IDs the user needs. On a failure, fetch `sipfront_stats_for_run` (transcript, turn and latency metrics, audio quality), `sipfront_siptrace_for_run` and `sipfront_callstates_for_run`, quote the decisive transcript turn, metric or SIP response, give the most likely cause and offer `/sipfront:analyze-failures <test>` for a comparison with earlier runs.

## Edge cases

- **Test not found:** show the closest matching test names and never dead-end.
- **Run cannot start (quota, paused subscription, no agents):** relay the API error verbatim and say what would unblock it.
- **User asks to change parameters first:** read the full test with `sipfront_get_test`, apply the change with `sipfront_update_test` sending back all fields, then run.
