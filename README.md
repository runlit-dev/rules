# runlit community rules

Open-source rule packs for runlit's ast-grep rule engine.

Apache 2.0 licensed. Contributions welcome.

## Structure

```
rules/
├── hallucination/
│   ├── python/
│   │   ├── stripe.yml        # Stripe API hallucination patterns
│   │   ├── openai.yml        # OpenAI API hallucination patterns
│   │   └── langchain.yml     # LangChain hallucination patterns
│   ├── typescript/
│   └── go/
├── security/
│   ├── injection.yml
│   ├── secrets.yml
│   └── auth-bypass.yml
└── quality/
    └── common-mistakes.yml
```

## Contributing

See CONTRIBUTING.md. Rules use ast-grep YAML format.
