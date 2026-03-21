# rules

[![license: Apache 2.0](https://img.shields.io/badge/license-Apache%202.0-blue?style=flat-square)](LICENSE)

Community rule packs for runlit — detect hallucinated and deprecated APIs in AI-generated code.

All rules are **runlit-authored Apache 2.0 IP**. Semgrep community rules are intentionally excluded — the Semgrep Rules License v1.0 prohibits SaaS use. These rules are written from scratch and run via [Opengrep](https://github.com/opengrep/opengrep) (LGPL-2.1), a drop-in replacement with identical YAML syntax.

Contributions welcome. If an AI model fabricated an API call that burned you in production, there should be a rule for that.

## Using these rules

Rules load automatically via the runlit GitHub App and CLI. To enable specific packs:

```yaml
# .runlit.yml
rules:
  packs:
    - community/stripe
    - community/openai
    - community/langchain
    - community/anthropic
    - security/secrets
```

## Rule format

```yaml
id: community/openai/completion-create-deprecated
pack: community/openai
severity: WARNING
message: "openai.Completion.create is legacy — use openai.chat.completions.create instead"
patterns:
  - 'openai\.Completion\.create'
  - 'client\.completions\.create'
languages: [python]
suggestion: "client.chat.completions.create(model=..., messages=[...])"
references:
  - https://platform.openai.com/docs/guides/text-generation/chat-completions-api
```

## Packs

### Hallucination packs

| Pack | Rules | What it covers |
|---|---|---|
| `community/stripe` | 6 | `attach_async` (never existed), deprecated `charge.create`, wrong kwargs, `confirm_async` |
| `community/openai` | 7 | `Edit` endpoint removed, legacy `Completion`, v0 `api_key`, `ChatCompletion` v0, `stream_to_file`, Node.js v3 imports |
| `community/langchain` | 7 | Post-v0.1 package split wrong imports (ChatOpenAI, OpenAI, Embeddings, schema, vectorstores, memory), deprecated `.run()` |
| `community/anthropic` | 5 | v0 completion API, wrong model names, wrong `stop_reason`, Node SDK v0, wrong package name |

### Security packs

| Pack | Rules | What it covers |
|---|---|---|
| `security/secrets` | 8 | Hardcoded API keys, secrets in logs, `eval`-on-input (Python+Node), `pickle.loads`, `subprocess shell=True`, Go SQL format injection |

## Contributing

1. Fork the repo
2. Add your rule under `community/<pack>/` or `security/`
3. Add a test case in `tests/<pack>/`
4. Run `make test` to validate
5. Open a PR with: what was fabricated, which model produced it, and a real-world example

When contributing, verify your rule is original — do not copy Semgrep community rules (license incompatibility).

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full rule schema reference.

## License

Apache 2.0 — see [LICENSE](LICENSE).
