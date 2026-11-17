# Roadmap

## Present foundation

The workspace contains one empty, unpublished `qquic` library. Repository checks
and native Linux CI validate the scaffold, not QUIC correctness. No protocol,
cryptographic, interoperability, performance, or production claims follow from
those checks.

## Design before implementation

Design an independent QUIC implementation with browntls providing TLS integration.
Before adding a dependency, establish the TLS handshake and packet-protection
interfaces and qualify an immutable upstream revision or reviewed release.

Decide the initial protocol versions and extensions, public API and error model,
crate boundaries, packet ownership, timing and I/O interfaces, asynchronous runtime,
feature model, platform support, and any allocation constraints through design
review. No runtime, kernel integration, HTTP/3, or no_std support is selected.

## Implementation prerequisites

Define trust boundaries and resource budgets before exposing packet parsing or
transport state. Include regression and negative tests for each implemented
behavior, protocol test vectors where applicable, deterministic time and loss
scenarios, and fuzzing for untrusted parsing and state transitions. Establish
interoperability and platform qualification before claiming support.

Independent review, dependency admission, security assessment, and the
[release prerequisites](Maintenance.md#release-prerequisites) apply as those
surfaces are introduced. No delivery dates or compatibility guarantees are set.
