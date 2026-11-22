<div align="center">

# HubScan

<h3>Find poisoned documents in RAG corpora by geometry</h3>

[![Status](https://img.shields.io/badge/status-design%20stage-orange?style=flat-square)](ROADMAP.md)
[![Go 1.26+](https://img.shields.io/badge/go-1.26+-00ADD8?style=flat-square)](https://go.dev/dl/)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue?style=flat-square)](LICENSE)

---

[How it works](#how-it-works) · [Thesis](THESIS.md) · [Roadmap](ROADMAP.md) · [Contributing](CONTRIBUTING.md) · [Security](SECURITY.md)

</div>

---

## What is HubScan?

Finds poisoned documents in a RAG corpus by where they sit in embedding space.
Poisoned documents are usually written to be retrieved for as many queries as
possible, which makes them unusual "hubs" that sit close to everything.
`hubscan` finds documents that show up in the top results for far more queries
than they should, plus chunks that carry hidden instructions, without needing
to know what the attack says.

> **Status: design stage.** There is no code yet. The commands and output
> below are the proposal, and they will change. Part of the
> [OpenAstra](https://openastra.org) research agenda, area IX: AI Data
> Security & Privacy.

---

## The problem

Anyone who can get a document into a RAG corpus (a public wiki page, a support
ticket, a shared drive) can try to poison it: plant text that gets retrieved
for many questions and steers the answers. Scanning for known attack strings
misses new attacks. But to work, a poisoned document has to be *retrievable*,
and that leaves a geometric trace.

---

## How it works

1. **Hubness.** In high-dimensional spaces some points become nearest
   neighbours of a disproportionate number of others ("Hubs in space",
   Radovanović et al., 2010). `hubscan` samples or replays queries, counts how
   often each chunk appears in the top-k, and flags chunks whose count is far
   above what their topic predicts.
2. **Query spread.** A real document is retrieved for related questions. A
   poisoned one is retrieved for unrelated topics. `hubscan` measures the
   topical spread of the queries each chunk answers.
3. **Instruction content.** Flag chunks that address the model rather than the
   reader ("ignore previous instructions", tool-call syntax, hidden Unicode,
   white-on-white text in the source).
4. **Rank and explain.** Combine the signals and show, for each suspect chunk,
   which unrelated queries it hijacks.

---

## Example (proposed)

```console
$ hubscan scan --index kb-prod --queries logs/queries-30d.jsonl
replayed 50,000 queries, top-k = 5

SUSPECT  chunk wiki/2291#4   hub score 38.2 (corpus p99: 4.1)
         retrieved for 1,912 queries across 27 unrelated topics
         contains instruction text addressed to the assistant
         sample queries: "reset my password", "Q3 travel policy", "VPN setup"
SUSPECT  chunk tickets/88120#1   hub score 21.7
         zero-width characters in 14 places

2 suspects in 182,440 chunks.
```

---

## Planned surfaces

- CLI and Python library over common vector stores
- Query replay from logs, or synthetic queries when no logs exist
- Ingestion-time check: score new chunks before they're indexed
- Reports in JSON and HTML

---

## Maps to

| Framework | Items |
|-----------|-------|
| OWASP LLM Top 10 (2025) | LLM04 Data and Model Poisoning, LLM08 Vector and Embedding Weaknesses, LLM01 Prompt Injection |
| MITRE ATLAS | Data poisoning techniques |
| NIST SP 800-53 Rev. 5 | SI-7 Software, Firmware, and Information Integrity, SI-4 System Monitoring |

---

## Open research questions

- Hubness baselines per embedding model and per corpus size
- How adaptive attackers can lower hub scores, and at what cost to the attack
- Evaluation against published RAG poisoning attacks

---

## Roadmap

The plan, from the first working version to 1.0, is in [ROADMAP.md](ROADMAP.md). The reasoning behind the project, and what would make us stop, is in [THESIS.md](THESIS.md).

---

## Related OpenAstra projects

- [Tripwire](https://github.com/sadayamuthu/tripwire) and [ACLDrift](https://github.com/sadayamuthu/acldrift): the other two RAG data checks
- [Taintline](https://github.com/sadayamuthu/taintline): treats retrieved content as untrusted at runtime

---

## Contributing

Design feedback, real-world cases and fixes are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for how to help while the project is at the design stage, and [GOVERNANCE.md](GOVERNANCE.md) for how decisions are made.

---

## Security

Please report vulnerabilities privately through [GitHub security advisories](https://github.com/sadayamuthu/hubscan/security/advisories/new), not in a public issue. See [SECURITY.md](SECURITY.md) for what to include and what's in scope.

---

## License

[Apache License 2.0](LICENSE)

---

<div align="center">

**HubScan: find poisoned documents in RAG corpora by geometry**

HubScan is managed by [OpenAstra](https://openastra.org)

[GitHub](https://github.com/sadayamuthu/hubscan) · [Thesis](THESIS.md) · [Roadmap](ROADMAP.md)

</div>
