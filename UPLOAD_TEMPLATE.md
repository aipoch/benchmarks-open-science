# Benchmark Upload Template

Use this template when adding benchmark results to this repository. Keep each run self-contained so that another researcher can understand the configuration, inspect the agent trace, and reproduce or regrade the result.

## 1. Directory layout

Create one directory per trial using this structure:

```text
<benchmark-name>/
  <task-name>/
    <task-name>__<trial-id>/
      result.json
      config.json
      regrade-result.json        # Optional: include only after regrading
      exception.txt              # Optional: include only if the run failed
      agent/
        trajectory.json
        trajectory-<session-id>.json  # Optional: worker or additional sessions
        events.ndjson.gz              # Recommended: complete event stream
        <agent-name>-identity.json    # Recommended: agent/model identity
      verifier/
        ...                           # Native verifier or grader outputs
```

Use lowercase kebab-case for benchmark and task directory names. Trial directories should combine the task name and a stable, unique trial identifier.

## 2. Required files

- `result.json`: the final score, reward, verdict, timing, and run status.
- `config.json`: the complete run configuration, including benchmark version, task identifier, agent/model settings, execution environment, timeout, and grader settings.
- `agent/trajectory.json`: the primary agent trajectory. If no trajectory is available, explain why in the upload summary below and include the available verifier evidence.
- `verifier/`: the benchmark's native grader outputs needed to support the reported result.

Preserve the benchmark's native output format whenever possible. Do not rewrite raw scores or verifier output solely for presentation.

## 3. Upload summary

Copy this section into the pull request description or the benchmark-level README and replace every placeholder.

```markdown
## Benchmark run

- **Benchmark:** `<name and version>`
- **Benchmark source:** `<repository or release URL>`
- **Evaluation date:** `<YYYY-MM-DD>`
- **Task set:** `<full set, public subset, science subset, or explicit task list>`
- **Number of tasks:** `<count>`
- **Number of trials per task:** `<count>`
- **Reported metric:** `<metric name>`
- **Aggregate result:** `<value>`
- **Selection rule:** `<best of one, pass@k, mean over trials, etc.>`
- **Execution environment:** `<runner, container/runtime, and infrastructure>`
- **Agent/model:** `<agent name, model, and reasoning/effort setting>`
- **Verifier/judge:** `<native verifier or judge model and version>`
- **Timeout:** `<per-task or per-run limit>`
- **Commit or release:** `<benchmark/harness commit SHA or release tag>`

### Notes and exceptions

`<Describe regrades, missing traces, timeouts, infrastructure failures, scoring differences, excluded tasks, or other limitations. Write "None" if there are no exceptions.>`
```

## 4. Results table template

```markdown
| Task | Trial ID | Score / Verdict | Counted | Trace | Notes |
| --- | --- | ---: | :---: | --- | --- |
| `<task-name>` | `<trial-id>` | `<score or pass/fail>` | yes/no | [trace](<relative-path>) | `<optional note>` |
```

If multiple trials exist for a task, list every trial and clearly identify which result is counted in the aggregate metric.

## 5. Privacy and integrity check

Before uploading, confirm that:

- [ ] Credentials, API keys, tokens, cookies, and private endpoints are removed.
- [ ] Personal data, local usernames, home paths, hostnames, and internal infrastructure details are removed or replaced with clear placeholders.
- [ ] Base64 or binary attachments have been reviewed; sensitive attachments are removed or replaced with placeholders.
- [ ] Encrypted reasoning tokens or other non-public model internals are not included.
- [ ] Task artifacts are included only when their license permits redistribution.
- [ ] The reported score matches the committed verifier output.
- [ ] Missing, truncated, failed, timed-out, or regraded runs are explicitly documented.
- [ ] Large files are compressed where appropriate and remain within GitHub's file-size limits.

Do not upload a run until this checklist is complete.
