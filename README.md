# alienmatic-mint-anchor

Public tamper-evident logbook for the ALIENMATIC coin-mint proofs.

This repository is the **root of trust** for the mint. The Alienmatic app and
website are mirrors of what is published here; where they disagree with this
repo, this repo is correct.

## What a publication looks like

Each weekly run is published in two stages, each as its own signed git tag:

| Stage | Tag | Files |
|---|---|---|
| Commitment, published **before** the randomness exists | `mint/<runId>/commit` | `runs/<runId>.commit.json` + `.bundle` |
| Full verification bundle, published after the run | `mint/<runId>/bundle` | `runs/<runId>.bundle.json` + `.bundle` |

Each `.bundle` file is the Sigstore signature over the artefact beside it, and
each artefact is recorded in the Sigstore Rekor public transparency log. Rekor's
independent timestamps are what prove the commitment was public *before* the
beacon round it names — which is the whole basis of the claim that the rarity
was not steered.

A signed tag can be checked with
[gitsign](https://github.com/sigstore/gitsign):

```bash
gitsign verify-tag \
  --certificate-identity=mint-robot@solid-26985.iam.gserviceaccount.com \
  --certificate-oidc-issuer=https://accounts.google.com \
  mint/<runId>/commit
```

## This history is append-only

Published tags are never moved, re-signed or deleted, and this repository is
never force-pushed. Anything that has to be withdrawn is withdrawn by an
ordinary commit that anyone can see, so a clone made at any time remains a
valid witness to what was published.

## About the `scratch-` entries in this history

Before the first real run, the anchor path was rehearsed against this live
repository. Those rehearsals used run ids prefixed **`scratch-`** and were
never real mints — no inventory exists for them and nothing was ever sold or
issued from them. Their tags have been deleted and their files removed by an
ordinary commit; both remain visible in this history, and their Rekor entries
remain permanently in the public log, by design.

Three such rehearsals have happened. Two on 30 July 2026:
`scratch-test-20260730-1651` (commitment stage only) and
`scratch-full-20260730` (a complete end-to-end run). One on 2 August 2026:
`scratch-verifierfix-20260802`, a complete end-to-end run made to re-check the
published verifier after it was hardened — it is the run that proved an honest
publication still passes.

Real runs are identified by an ISO week id, for example `2026-W31`.
