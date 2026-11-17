# qquic

An independent Rust QUIC library project with planned
[browntls](https://github.com/OxiBelt/browntls) TLS integration.

This repository is a compiling foundation, not a QUIC implementation. The single
`crates/qquic` library has no protocol APIs, external dependencies, or transport
functionality. Architecture, asynchronous runtime, supported deployment platforms,
and TLS integration interfaces remain to be designed. There are no releases,
stable API guarantees, audits, or production-readiness claims.

## Development

Rust 1.100.0 is pinned in `rust-toolchain.toml`, including rustfmt and Clippy.
Run these commands from the repository root:

```sh
cargo build --workspace --locked
cargo fmt --all --check
cargo clippy --workspace --all-targets --all-features --locked -- -D warnings
cargo test --workspace --all-features --locked
RUSTDOCFLAGS='-D warnings' cargo doc --workspace --all-features --no-deps --locked
```

There is no executable to run. Rust currently has zero unit, integration, or
protocol tests; maintenance tooling has its own regression tests.

## Contributing

- [Repository guidelines](AGENTS.md)
- [Contributor workflow and checks](CONTRIBUTING.md)
- [Roadmap and deferred design](docs/Roadmap.md)
- [Maintenance and release prerequisites](docs/Maintenance.md)
- [Security policy](SECURITY.md)

The contributor workflow is adapted from
[OxiBelt](https://github.com/OxiBelt/OxiBelt) and
[browntls](https://github.com/OxiBelt/browntls).

## License

[Apache-2.0](LICENSE).
