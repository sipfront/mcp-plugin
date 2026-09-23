---
description: Analyze failed Sipfront test runs and explain the likely cause
argument-hint: project or test name/ID, optionally a time range (default: last 24 hours)
---

Find out why Sipfront tests in $ARGUMENTS failed and tell the user what to fix.

## Steps

1. **Load context.** Call `sipfront_llmdocs` once per session if you have not yet, then `sipfront_memory_list`; read any `findings/...` entries for the project or test in question so you do not repeat past analysis.
2. **Resolve the target.** Call `sipfront_list_projects` and, for a project, `sipfront_list_tests`. Match `$ARGUMENTS` against project and test names and IDs. If nothing is given, analyze all projects. If the match is ambiguous, show the candidates and ask.
3. **Collect the failed runs.** For each test, call `sipfront_results_for_test` (default: the last 24 hours, or the range the user gave) and keep the runs whose status is failed or errored. Also keep the most recent passing run of the same test as the baseline.
4. **Compare failed against passing.** For each failed run, fetch `sipfront_siptrace_for_run` and `sipfront_callstates_for_run`, and `sipfront_stats_for_run` for transcripts, turn metrics and audio quality (`sipfront_stats_types_for_run` lists what is available). Compare with the baseline run and look for:
    - Conversation: the bot never spoke or stopped answering, a turn that missed the expected intent or transcript content, slow responses (inter-turn latency, time to first audio), too many or too few turns, the bot talking over the caller, or the caller's TTS not being understood.
    - SIP: final response codes (4xx/5xx/6xx), timeouts without any response, authentication loops (401/407), wrong Request-URI or Contact, missing or changed User-Agent.
    - Media: SDP differences (codecs, SRTP/DTLS, ICE), IP/port differences that explain one-way audio, low MOS or high packet loss/jitter in the RTP statistics.
    - Timing: hour-of-day and day-of-week patterns across the failed runs, and whether all failures share one target, agent pool or region.
5. **Report.** Give one short verdict per test: what failed, the evidence (quote the decisive transcript turn, metric or SIP line), whether the voice agent or the call path is at fault, the most likely cause, and a concrete next step for the user. Use a table when more than one test failed. Do not paste whole traces.
6. **Persist.** Store the conclusion with `sipfront_memory_set` under `findings/<project_id>/<test_id>` so the next session starts from it. If the user agrees, add a one-paragraph summary to the run with `sipfront_add_run_comment`.

## Edge cases

- **No failed runs in the range:** say so, show the pass rate, and offer to widen the range or look at a specific run.
- **Failure has no SIP trace (e.g. test agent never started):** report the run status and error text from `sipfront_get_run_status` and point at agent pool or configuration rather than the target.
- **Many failures:** group by target and error, summarize counts, and analyze the largest group in detail first.
