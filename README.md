# rules

[![license: Apache 2.0](https://img.shields.io/badge/license-Apache%202.0-blue?style=flat-square)](LICENSE)

Community rule packs for runlit — detect hallucinated and deprecated APIs in AI-generated code.

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

| Pack | Rules | What it covers |
|---|---|---|
| `community/stripe` | 8 | Removed methods, deprecated Payment Intents patterns |
| `community/openai` | 12 | Removed endpoints, deprecated client patterns, key format changes |
| `community/langchain` | 10 | Package restructuring (v0.1 → v0.2 → v0.3 breaking changes) |
| `community/anthropic` | 6 | SDK v2 → v3 breaking changes, removed models |
| `community/aws-sdk` | 15 | AWS SDK v2 breaking changes (Go + Python) |

## Contributing

1. Fork the repo
2. Add your rule under `rules/<pack>/`
3. Add a test case in `tests/<pack>/`
4. Run `make test` to validate
5. Open a PR with a description of what was fabricated and which model produced it

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full rule schema reference.

## License

Apache 2.0 — see [LICENSE](LICENSE).
