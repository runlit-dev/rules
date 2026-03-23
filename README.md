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
    - community/google-ai
    - community/huggingface
    - community/llamaindex
    - community/pinecone
    - community/vercel-ai
    - security/secrets
    - security/crypto
    - security/injection
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
| `community/openai` | 18 | `Edit` endpoint removed, legacy `Completion`/`ChatCompletion` v0, v0 `api_key`, `stream_to_file`, Node.js v3 imports, Assistants v2 (`file_ids` removed), DALL-E v0, Whisper v0, outdated model names, `o1`/`o3` wrong params, structured output confusion |
| `community/anthropic` | 12 | v0 completion API, wrong model names (March 2026), wrong `stop_reason`, Node SDK v0, wrong package name, extended thinking format, tool result type, batch API (`beta` prefix removed), vision image format, wrong current model IDs |
| `community/langchain` | 15 | Post-v0.2 import paths (ChatOpenAI, OpenAI, Embeddings, schema, vectorstores, memory), deprecated `.run()`, `LLMChain`/`ConversationChain`/`SequentialChain` removed, LCEL `.invoke()` wrong arg, LangGraph import/node errors, `hub.pull` missing package |
| `community/stripe` | 13 | `attach_async` (never existed), deprecated `charge.create`, wrong kwargs, `confirm_async`, Plans API deprecated, wrong webhook arg order, `source` param legacy, Prices API migration |
| `community/google-ai` | 10 | Old `google-generativeai` SDK (`configure`, `GenerativeModel`, wrong import), retired model names (`gemini-pro`, `gemini-pro-vision`, `gemini-ultra`), `start_chat`, `count_tokens`, `embedding-001`, Node.js legacy package |
| `community/huggingface` | 9 | Hallucinated `.predict()`, `use_gpu` kwarg, deprecated `use_auth_token`, `tokenizer.encode()` as model input, incompatible `temperature`/`do_sample`, missing `pad_token_id`, `.generate()` on pipeline, `device_map` + `device` conflict, gated model token |
| `community/llamaindex` | 9 | Old `from llama_index import ...` paths (v0.10 breaking change), `GPTSimpleVectorIndex`/`GPTTreeIndex` hallucinated classes, `ServiceContext` deprecated, `LLMPredictor` removed, old LLM/embeddings imports, `query()` return type confusion |
| `community/pinecone` | 9 | `pinecone.init()` removed (v3), `pinecone.Index()`/`create_index()`/`list_indexes()`/`delete_index()` module-level functions removed, old `environment` param, tuple upsert deprecated, `PineconeClient` renamed to `Pinecone` |
| `community/vercel-ai` | 9 | `experimental_StreamData` renamed, `experimental_onFunctionCall` removed, `ai/openai` / `ai/anthropic` import paths removed, `OpenAIStream`/`AnthropicStream` removed, `StreamingTextResponse` removed, `LangChainAdapter` path, raw string as model |

### Security packs

| Pack | Rules | What it covers |
|---|---|---|
| `security/secrets` | 8 | Hardcoded API keys, secrets in logs, `eval`-on-input (Python+Node), `pickle.loads`, `subprocess shell=True`, Go SQL format injection |
| `security/crypto` | 14 | MD5/SHA1 for passwords, SHA-256 without salt, AES-ECB mode, DES/3DES, hardcoded IV, `random` for tokens (Python+JS), `requests verify=False`, `ssl.CERT_NONE`, `NODE_TLS_REJECT_UNAUTHORIZED=0`, HTTPS `rejectUnauthorized:false`, Go `InsecureSkipVerify`, RSA PKCS1v15 padding |
| `security/injection` | 13 | `yaml.load()` unsafe, `yaml.load_all()` unsafe, `exec()` on input, Jinja2 `autoescape=False`, SSTI via `from_string(user_input)`, Flask `render_template_string(user_input)`, path traversal (`open(request.x)`, `os.path.join` without `realpath`), Node path traversal, lxml XXE, MongoDB `$where` injection, MongoDB unparameterized query, prototype pollution via `Object.assign(target, req.body)` |

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
