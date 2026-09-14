# Lisply MCP — agent guidance

This file guides Claude Code (claude.ai/code) or other AI agents
working in this repo.

## What this repo is

The Genworks-maintained fork of [Cyborg
Whisperer](https://github.com/gornskew/cyborg-whisperer) (remote
`upstream` in a working clone): a Node.js MCP middleware that
presents MCP tools (`lisp_eval`, `http_request`, `ping_lisp`, and
`lisply_search` where the backend carries a document corpus -- called
`skewed_search` until 2026-09-09; the alias was dropped 2026-09-10) to
any MCP client and relays them over HTTP to a Lisply-compliant backend.

This fork keeps the project's original name, `lisply-mcp`, which is
also the wrapper's default server name — here the repository name and
the runtime defaults agree.  Upstream changes are merged when chosen;
deliberate divergence is limited to naming, attribution, and
documentation voice.  Code fixes that apply upstream belong upstream
first.

## Layout

- `scripts/mcp-wrapper.js` — main entry point
- `scripts/lib/` — config, logger, server, utils
- `scripts/handlers/` — per-tool request handlers
- `BACKEND-REQS.md` — the Lisply protocol spec (what a compliant
  backend must implement)
- `CORPUS.md` — the Lisply corpus: how a project provides its
  `lisply_search` index (file format, the `lisply.corpus` image
  label, how a console merges), and `scripts/lisply-index.js`, the
  reference indexer that writes the format.  Content lives with each
  project; the format lives here, with the protocol that promises
  the tool.
- `regression-tests/harness.js` — stdio JSON-RPC regression harness
- `attic/` — the retired container-management subsystem, history only

## Working on it

- The wrapper is a **pure HTTP client**: it never pulls, starts, or
  manages containers.  Container lifecycle belongs to the Basalt
  build system (`~/projects/basalt`, `./basalt up`).  Do not
  reintroduce docker plumbing here.
- After JS edits: `node --check scripts/mcp-wrapper.js` (and any
  touched lib/handler files).
- Regression harness, against a live backend (from inside a
  deployment's network, substitute the service hostname for the
  loopback address):

```bash
node regression-tests/harness.js --backend-host 127.0.0.1 --http-host-port 9081
```

- Quick manual probe of a Lisply backend (default Gendl backend
  published on host port 9081):

```bash
curl -X POST http://127.0.0.1:9081/lisply/lisp-eval -d '{"code": "(+ 1 2 3)"}'
```

## Compatibility contracts — do not sweep these

The following defaults are user-visible contracts.  Changing them is
a versioned behavior decision, never part of a naming or doc sweep:

- default `--server-name`: `lisply-mcp` (feeds MCP tool prefixes)
- default log file: `/tmp/lisply-mcp-wrapper.log`
- the `LISPLY_*` environment-variable prefix
- the `lisply` endpoint prefix and endpoint names

These same defaults are contracts upstream as well; a merge from
upstream must never change them.

## Trust model

Lisply backends are exposed to the LLM as **trusted sandboxes**: the
wrapper does not restrict Lisp operators, filesystem access, or
subprocesses.  Isolation is the operator's job, at the container
boundary.  Keep tool metadata advertising this (`TRUST_AS_SANDBOX`,
`SANDBOX_NOTE`) intact.
