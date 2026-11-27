# Targets

Deliberately vulnerable applications used as lab and benchmark targets for AgentX (static reachability, sandboxed exploit validation, and VEX output).

> **Warning:** every application here is intentionally insecure. Run them only inside an isolated lab or staging network with no route to production or the internet. Never deploy them on a reachable host.

## Inventory

Each target is a snapshot of an upstream release tag, copied file for file without its Git history. `targets.json` holds the same data in machine-readable form.

| Target | Directory | Upstream | Release tag | Upstream commit | License | Stack |
|---|---|---|---|---|---|---|
| OWASP WebGoat | `webgoat/` | https://github.com/WebGoat/WebGoat | `v2026.5` | `7e198f39189f33e1912f7cbb60e24e42635ca06a` | GPL-2.0-or-later | Java (Spring Boot, Maven), containerized |

## Adding a target

1. Shallow-clone the upstream repository at a release tag.
2. Copy its tracked files into a new top-level directory, excluding `.git`.
3. Confirm the copied file list matches `git ls-files` upstream.
4. Add a row above and an entry in `targets.json` with the tag, full commit ID, license, and import date.

Keep each target's upstream license file in its directory.
