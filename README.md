# Targets

Deliberately vulnerable applications used as lab and benchmark targets for AgentX (static reachability, sandboxed exploit validation, and VEX output).

> **Warning:** every application here is intentionally insecure. Run them only inside an isolated lab or staging network with no route to production or the internet. Never deploy them on a reachable host.

## Inventory

Each target is a snapshot of an upstream release tag, copied file for file without its Git history. Where upstream publishes a release binary for the same tag, it sits in the target's `binary/` subfolder so source and binary match. `targets.json` holds the same data in machine-readable form.

| Target | Directory | Upstream | Release tag | Upstream commit | License | Stack |
|---|---|---|---|---|---|---|
| OWASP WebGoat | `webgoat/` | https://github.com/WebGoat/WebGoat | `v2026.4` | `872d6149d4ef29e4929c2f4eda279f7fbedc52e8` | GPL-2.0-or-later | Java 25, Spring Boot 4.1.1, Maven, containerized |

### Binaries

| Target | File | Source | Size | SHA-256 |
|---|---|---|---|---|
| OWASP WebGoat | `webgoat/binary/webgoat-2026.4.jar` | Official release asset, https://github.com/WebGoat/WebGoat/releases/download/v2026.4/webgoat-2026.4.jar | 160,523,398 bytes | `846dd1d60692558fcab42627e561db2dd22d49fed1eeac68b6cbea088bf13e00` |

Binaries are stored with Git LFS. Install Git LFS before cloning, or run `git lfs pull` afterwards. Check a download with `sha256sum -c SHA256SUMS` inside the `binary/` folder.

WebGoat v2026.4 is used because the v2026.5 release has no published binary. The JAR was checked against the v2026.4 source: its manifest reports `Implementation-Version: 2026.4`, and 783 of its 785 packaged resource files are byte-identical to `webgoat/src/main/resources`. The two that differ are the properties files where Maven substitutes `@project.version@` with `2026.4` at build time. The tag's `pom.xml` reads `2026.4-SNAPSHOT` because WebGoat's release workflow sets the version during the build.

Run it in an isolated lab with Java 25: `java -jar webgoat/binary/webgoat-2026.4.jar`.

## Adding a target

1. Shallow-clone the upstream repository at a release tag.
2. Copy its tracked files into a new top-level directory, excluding `.git`.
3. Confirm the copied file list matches `git ls-files` upstream.
4. If upstream publishes a binary for the same tag, add it under `<target>/binary/`, track it with Git LFS in `.gitattributes`, and record its SHA-256 in `SHA256SUMS`.
5. Add rows above and an entry in `targets.json` with the tag, full commit ID, license, binary digest, and import date.

Keep each target's upstream license file in its directory.
