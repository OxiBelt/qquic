# Repository Guidelines

## Project Structure & Module Organization

qquic is an independent Rust QUIC library project with planned browntls TLS
integration. The compiling scaffold has no transport implementation or public API.
`crates/qquic/` holds the library; `docs/` holds roadmap and maintenance guidance;
`tests/scripts/` contains repository checks and their Python regression tests;
`scripts/ci/` contains toolchain setup; `.github/` contains workflows and templates.
No runtime assets exist yet. Add protocol tests alongside future implementation.

## Build, Test, and Development Commands

Run from the repository root with pinned Rust 1.100.0:

```sh
cargo build --workspace --locked
cargo fmt --all --check
cargo clippy --workspace --all-targets --all-features --locked -- -D warnings
cargo test --workspace --all-features --locked
RUSTDOCFLAGS='-D warnings' cargo doc --workspace --all-features --no-deps --locked
```

These build, format-check, lint, test, and document the scaffold. Nothing is
executable yet. See [CONTRIBUTING.md](CONTRIBUTING.md#local-checks) for Python,
Markdown, module-size, whitespace, and pinned actionlint checks.

## Coding Style & Naming Conventions

Use rustfmt, four-space Rust indentation, and two spaces for Markdown, YAML,
and TOML. Use `snake_case` modules/functions, `UpperCamelCase` types, and
`SCREAMING_SNAKE_CASE` constants. Unsafe code is forbidden. Review modules above
750 lines and split before exceeding the enforced 1,000-line limit.

## Testing Guidelines

Rust uses Cargo's test harness; currently it has zero tests. Python checks use
`unittest` with `test_*.py` names. Add descriptive regression and negative tests
for implemented behavior. No numerical coverage target exists. Passing scaffold
CI does not establish QUIC correctness.

## Commit & Pull Request Guidelines

The initial commit is `Create LICENSE`; going forward, require signed Conventional
Commits, such as `docs: clarify roadmap`. Include purpose, compatibility effects,
tests, and limitations in PRs; link relevant issues. Disclose every LLM-assisted
contribution; Codex commits require `Assisted-by: Codex <codex@openai.com>`.

[CONTRIBUTING.md](CONTRIBUTING.md) governs workflow and independent review.
Use the [delegation skill](.agents/skills/subagent-delegation/SKILL.md) when useful.
Keep secrets and undisclosed findings private; follow [SECURITY.md](SECURITY.md).
