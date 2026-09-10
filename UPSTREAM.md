# Upstream

Lisply-MCP tracks an upstream project, **Cyborg Whisperer**,
maintained by Gornskew Enterprises
(https://github.com/gornskew/cyborg-whisperer), from which it is
periodically merged. This file is the one place in this repository
that records the relationship; the product documentation does not
depend on it.

**Shared with upstream, and kept compatible:** the wrapper, the
Lisply protocol (`BACKEND-REQS.md`), the default `--server-name`
(`lisply-mcp`), the default log file, the `LISPLY_*` environment
variables, and the `lisply` endpoint prefix and names. These are
compatibility contracts; changing any of them is a versioned
behavior decision made with upstream, never a documentation edit.

**Deliberately different here:** the attribution and the
documentation voice. The project name is the same in both
distributions.

**Issues and fixes:** file issues against this repository. A fix that
applies to the shared code is carried upstream by the maintainers;
upstream changes are merged here when chosen.

Copyright © 2026 Genworks International; portions copyright © 2026
Gornskew Enterprises. GNU Affero General Public License, version 3
or later.
