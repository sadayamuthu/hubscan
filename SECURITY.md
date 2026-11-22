# Security Policy

## Reporting a vulnerability

Please don't report security problems in a public issue, pull request or
discussion. Use
[**Report a vulnerability**](https://github.com/sadayamuthu/hubscan/security/advisories/new)
on the repository's Security tab instead. Only the maintainers can see the
report.

Include what you can of:

- the HubScan version (`hubscan version`) or commit, your Go version and OS
- how HubScan was used, and its configuration with secrets removed
- steps to reproduce
- what happened and what you expected

You'll get a reply when the report has been read, and again when a fix is
ready. A fix ships as a patch release; the advisory names you unless you'd
rather it didn't.

## Supported versions

HubScan is at the design stage and has no releases yet. Once it does,
fixes land in the latest release only, until 1.0.

## What counts

HubScan reads a vector index and, optionally, query logs, and writes reports. It may call an embedding model you configure to embed synthetic queries.

HubScan is a security tool, so anything that breaks one of these promises
is a security issue:

- **Read-only.** HubScan never writes to an index. A path that does is in scope.
- **Query logs stay local.** Queries are not sent anywhere except the embedding endpoint you configure. A path that sends them elsewhere is in scope.

These are by design, and not vulnerabilities:

- HubScan finds suspects, not proof. An adaptive attacker may evade it; see the open research questions in the README.
- Reports quote suspect chunk text and sample queries. Treat them as sensitive.

A wrong result that isn't covered above is a bug, not a vulnerability. Report
it in a normal issue; see [CONTRIBUTING.md](CONTRIBUTING.md).
