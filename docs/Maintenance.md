# Maintenance

## Current baseline

The only Cargo graph is the root workspace and committed `Cargo.lock`. It has no
external dependencies. Rust 1.100.0 is both the minimum supported compiler and
pinned development/CI toolchain. Keep version changes explicit and qualify them
with the documented checks. Dependabot checks Cargo and GitHub Actions weekly.

CI uses native x86-64, aarch64, and riscv64 Linux runners. This validates the
empty scaffold; it does not establish a QUIC support matrix. The RISC-V job uses
`ubuntu-24.04-riscv` from the
[RISE organization app](https://github.com/apps/rise-risc-v-runners). A maintainer
must ensure that app is installed and authorized for this repository. Do not
silently substitute emulation or treat a queued job as a passing check.

Repository checkers and delegation guidance are adapted from browntls revision
`e04e51cffc22ba17ffd03a78a3b59b9f93e78ea2` under Apache-2.0.

Tool downloads are version-pinned and checksum-verified. Actions use immutable
commit pins. Review pins and checksums together during updates. actionlint is
upstream 1.7.12; a fork with the same executable name is not equivalent evidence.

## Adding dependencies and implementation

Inventory every new manifest, lockfile, vendored source, native dependency, build
script and feature. Review provenance, licenses, maintenance, unsafe code, and
security effects for direct and transitive dependencies. Define cargo-audit,
cargo-deny and cargo-vet admission with real evidence before introducing runtime
dependencies; never fabricate audits or use wildcard exemptions. Exceptions need
an exact scope, owner, justification, tracking issue, and expiry.

browntls is the intended TLS integration, not a current Cargo dependency. Select
and qualify its version and interface during implementation design. Add fuzzing,
interoperability, and relevant platform checks with the implemented surfaces.

## Release prerequisites

All packages have `publish = false`. No release automation exists. Before release,
establish package ownership, SemVer and compatibility policy, supported platforms,
feature coverage, artifact verification, changelog, and release security review.
Validate the exact candidate, create and verify a signed annotated tag, obtain
explicit publication authorization, and verify published artifacts. Never retag
an existing release. Commit or push permission does not imply publication permission.

## Private reporting and evidence

Enable GitHub private vulnerability reporting and verify it through an uncached
API readback before describing it as available. Repository files alone cannot
enable that setting. RISE app access also needs separate hosted verification.

Keep undisclosed findings and reproduction inputs out of public commits, issues,
CI logs, caches, and artifacts. No private evidence repository is designated;
verify an authorized private destination before retaining or uploading evidence.
Report unrun checks and coverage limits explicitly.
