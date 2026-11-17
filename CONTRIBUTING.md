# Contributing to qquic

qquic is an independent Rust QUIC library project with planned browntls TLS
integration. It currently contains one empty library, no external dependencies,
and no stable API or support commitment. This document governs contribution
workflow; [AGENTS.md](AGENTS.md) provides concise orientation.

## Repository layout

Rust code lives in `crates/qquic/`, design and maintenance guidance in `docs/`,
repository checkers and their regression tests in `tests/scripts/`, CI helpers
in `scripts/ci/`, and automation and templates in `.github/`. The canonical
[delegation skill](.agents/skills/subagent-delegation/SKILL.md) lives in
`.agents/skills/`; Claude discovery uses a relative symlink to the same copy.
Do not create empty runtime or asset directories to imply decided architecture.

## Contribution workflow

1. Read affected code and applicable policies before editing. Discuss architecture
   before adding protocol APIs, dependencies, or runtime adapters.
2. Make bounded changes; preserve documented behavior and update relevant tests
   and documentation together. Never remove tests merely to make CI pass.
3. Run applicable checks and report exact failures, omissions, and environment
   limits. Distinguish observations from inference.
4. Review the full diff and clean up only resources created by your work.

Use a branch and PR normally. Explicitly authorized direct-to-main work must
meet the same signing, attribution, review, and validation rules. Permission to
edit does not imply permission to commit, push, or publish.

## Local checks

Run from the root with Rust 1.100.0, Python 3.10+, Git, and upstream
[actionlint](https://github.com/rhysd/actionlint) 1.7.12:

```sh
cargo build --workspace --all-targets --all-features --locked
cargo fmt --all --check
cargo clippy --workspace --all-targets --all-features --locked -- -D warnings
cargo test --workspace --all-features --locked
RUSTDOCFLAGS='-D warnings' cargo doc --workspace --all-features --no-deps --locked
python3 -B -m unittest discover -s tests/scripts -p 'test_*.py'
python3 -B tests/scripts/check_markdown_links.py --repo-root .
python3 -B tests/scripts/check_rust_line_count.py --repo-root .
actionlint -shellcheck= -pyflakes=
git diff --check
git diff --cached --check
git show --format= --check HEAD
```

The checkers inspect Git-tracked files, including staged additions. Include new
files in a disposable checkout/index or stage only intended additions before
checking; do not claim coverage of untracked files. The Markdown checker validates
local destinations and anchors, not external URLs, and is not a full CommonMark
parser. CI verifies the actionlint download checksum; optional shellcheck and
pyflakes integrations are disabled for reproducibility.

Every tracked Rust file is limited to 1,000 lines, counting comments, blanks,
tests, and generated content without exceptions. CI enforces this separately.

Rust uses Cargo's test harness and currently runs zero tests. Python uses
`unittest` and `test_*.py` filenames. Add descriptive regression and negative
tests alongside future behavior, using crate-local integration tests where
appropriate. No numerical coverage threshold is established. Passing maintenance
or empty-crate checks is not evidence of QUIC correctness.

## Style

Use rustfmt and Clippy, four spaces for Rust, and two spaces for Markdown, YAML,
and TOML. Use `snake_case` modules/functions, `UpperCamelCase` types, and
`SCREAMING_SNAKE_CASE` constants. Split modules by responsibility. Keep lockfile
and compiler upgrades explicit; see [maintenance](docs/Maintenance.md).

## Commit messages and signatures

All contribution commits must be cryptographically signed. Use Conventional
Commits with an imperative summary:

```text
<type>(<scope>): <subject>
```

Types: `feat`, `fix`, `chore`, `docs`, `ci`, `refactor`, `security`, `tests`,
or `perf`. Choose a responsibility-based scope. Put literal identifiers, paths,
and commands in inline code spans when used in prose. Keep messages portable,
sanitized, and understandable without session-specific artifacts.

```sh
git commit -S
git verify-commit HEAD
```

Verify hosted signatures after pushing. Release tags must also be signed and
verified; see the [release prerequisites](docs/Maintenance.md#release-prerequisites).

## LLM assistance notices

If any LLM agent participated in preparing or reviewing a contribution, disclose
that assistance on each affected commit, issue, comment, pull request, review,
or other account-authored contribution. Review by a person does not remove the
notice requirement. Identify the actual agent; never attribute another tool's
work to Codex.

For every Codex-assisted commit, append this trailer after a blank line:

```text
Assisted-by: Codex <codex@openai.com>
```

For other agents, use an `Assisted-by` trailer identifying the agent, without
inventing an email identity. For comments, PR descriptions, and other prose,
include an equivalent visible notice, such as `Assisted-by: Codex
<codex@openai.com>`. Keep the notice when editing an existing contribution.

## Security requirements

Follow [SECURITY.md](SECURITY.md) for undisclosed findings. Do not expose an
unpatched vulnerability through public issues, PRs, logs, or commit descriptions.

Treat peer-controlled QUIC datagrams and TLS data and caller-controlled configuration as untrusted.
For relevant changes, describe the trust boundary, attacker-controlled inputs,
failure behavior, compatibility effects, and remaining risks. Preserve peer authentication, packet protection, key isolation, transport state,
and bounded resource use.
Never weaken a security default without explicit review, tests, and documentation.

Do not add externally reachable panics or silently ignore security-sensitive
errors. Split Rust modules by responsibility, keep protocol policy separate from
OS effects, and review files above 750 lines as an advisory signal. No `.rs`
file may exceed 1000 lines; the [local checks](#local-checks) enforce that hard
limit. Do not introduce allocation, dispatch, or synchronization solely to split
a hot path.

First-party Rust currently forbids unsafe code. Changing that baseline requires
explicit design review before adding any exception. Any unsafe,
FFI, or kernel boundary needs a documented narrow exception, safety invariants,
a named non-author reviewer, and relevant Miri, sanitizer, fuzz, or syscall
evidence with applicability limits. Architecture and supported platforms must be
established before claiming those boundaries are safe.

Review direct and transitive dependencies, features, sources, build scripts,
licenses, maintenance, and unsafe code. Security-critical additions and exceptions
need a named owner and tracking issue. Exceptions must be exact, justified, and
time-bounded. Never invent cargo-vet audits or use wildcard exemptions.

## Independent review

Before landing substantive behavior or safety changes to cryptography, PKI,
parsing, protocol state, concurrency, resource limits, or unsafe/FFI/kernel
boundaries, obtain review by a capable, named non-author person or separate
agent. This applies to behavior-changing mechanical edits too; ordinary prose,
comments, and formatting do not trigger this gate.

The reviewer must inspect the actual diff and materially affected surrounding
code against the intended contract and safety invariants. A separate agent
using the same model is acceptable; changing providers alone establishes
neither independence nor correctness. Record reviewer identity, the reviewed
commit or reproducible diff, findings, verification, and remaining limits in the
task report. Keep public contribution messages portable and sanitized.

Resolve blocking findings and obtain review of material follow-up changes
before landing. If an eligible reviewer is unavailable, continue useful local
preparation and testing, report the gap, and leave affected changes unlanded.
Self-review does not satisfy the gate. Boundary-specific evidence and exception
requirements above still apply.

For delegated work, use the
[subagent delegation skill](.agents/skills/subagent-delegation/SKILL.md).
It defines bounded ownership, evidence reports, model selection, and escalation;
it does not waive contribution, review, signing, or disclosure requirements.

## Tests and temporary resources

Generate test keys, certificates, configurations, and logs per run in temporary
directories. Do not commit real secrets, generated private keys, build outputs,
or security evidence. Clean up on failure where practical. Future integration
checks must be reproducible locally and in CI, use run-specific resource names,
and avoid broad prune or deletion commands.

## Pull request checklist

- Describe the purpose, resulting behavior, affected areas, and compatibility.
- Link relevant issues; include screenshots only for visual changes.
- Use signed Conventional Commits and disclose actual LLM assistance.
- Include applicable tests, documentation, exact results, and skipped checks.
- Record required independent review and dependency or safety evidence.
- Confirm temporary resources are cleaned up and no secrets are included.

Public contributions are made under the repository's [Apache-2.0 license](LICENSE).
