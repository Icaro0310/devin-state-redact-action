# devin-state-redact-action

GitHub Action that gates a checked-out repo with
[`devin-redact`](https://pypi.org/project/devin-redact/) — the
secret-scanning package from the
[devin-state](https://github.com/Icaro0310/devin-state) family of the
devin-* ecosystem.

It runs `devin-redact gate` on the workspace and fails the job when the
verdict is `BLOCKED`. Read-only by design: the action only scans, it
never modifies files.

## Usage

```yaml
- uses: actions/checkout@v4
- uses: Icaro0310/devin-state-redact-action@v1
```

With options:

```yaml
- uses: Icaro0310/devin-state-redact-action@v1
  with:
    paths: "src docs"            # default: "."
    version: "devin-redact==0.2.0"  # default: ">=0.2,<0.3"
    fail-on-review: "true"       # default: only BLOCKED fails
```

## Verdicts

| Verdict   | Meaning                                        | Fails job?                |
|-----------|------------------------------------------------|---------------------------|
| `CLEAN`   | no findings                                    | no                        |
| `REVIEW`  | findings a human should look at                | only with `fail-on-review`|
| `BLOCKED` | secret-class findings — publication is unsafe  | yes                       |

The gate verdict is also exposed as the `verdict` output.

## Notes

- Part of the devin-* ecosystem; this repository is only the Action
  distribution vehicle — all scanning logic lives in the
  [`devin-redact`](https://github.com/Icaro0310/devin-state/tree/main/packages/redact)
  package.
