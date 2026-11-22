# HubScan: thesis

**In one line:** to work, a poisoned RAG document must be retrievable for
many queries, and that leaves a geometric trace you can find without knowing
what the attack says.

## The problem

Anyone who can get text into a RAG corpus can try to plant content that is
retrieved widely and steers answers. Scanning for known attack strings misses
new attacks.

## The bet

1. **Hubness is a detectable signature.** Poisoned chunks optimised for broad
   retrieval become unusual hubs in embedding space (Radovanović et al.,
   2010), appearing in top-k results far more often than their topic
   predicts.
2. **Query spread separates hubs from popular documents.** Legitimate popular
   chunks answer related questions; poisoned ones answer unrelated topics.
3. **Combining geometry with simple content signals** (text addressed to the
   model, hidden Unicode) gives high precision without an injection
   classifier.
4. **Attackers pay to evade.** Lowering a hub score means being retrieved
   less, which weakens the attack.

## Why now

Published RAG poisoning attacks show a few documents can steer answers, more
corpora ingest user-writable sources (tickets, wikis, shared drives), and
query logs to replay now exist in most deployments.

## What success looks like

- High detection rates against published RAG poisoning attacks at low false
  positive rates on clean corpora.
- Baselines published per embedding model and corpus size.
- An ingestion-time check fast enough to run on every new chunk.
- A paper with the evaluation and the adaptive-attacker analysis.

## What would prove us wrong

- Targeted poisoning (aimed at a few specific queries) is effective enough
  that attackers don't need hubness.
- Legitimate hubs (FAQs, boilerplate) are so common that scores can't
  separate them from attacks.
- Adaptive attacks keep hub scores normal while staying effective.

## Not a bet on

Detecting all prompt injection, or permission leaks (ACLDrift, Tripwire).

## Where it fits

Area IX, AI Data Security & Privacy. One of three RAG data checks with
[Tripwire](https://github.com/sadayamuthu/tripwire) and
[ACLDrift](https://github.com/sadayamuthu/acldrift).
