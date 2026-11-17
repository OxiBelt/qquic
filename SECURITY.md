# Security Policy

## Scope and support

qquic is an independent Rust QUIC library project with planned browntls TLS
integration. It currently contains an empty, dependency-free library and
repository tooling. No QUIC implementation, stable API, release, audit,
cryptographic certification, or production-readiness claim exists.

`main` is the development branch for triage and fixes, not a stable release.
There are no released versions or backport commitments. Establish release
support and end-of-support rules before publication.

Repository code, documentation, dependencies when introduced, build and CI
configuration, and official future artifacts are in scope when a realistic
unsafe outcome is established. There are no blanket finding-class exclusions
or accepted-risk waivers. Missing future functionality alone is not a
vulnerability; insecure implemented behavior or misleading guidance can be.

## Threat boundaries and requirements

These are requirements for future design and review, not implemented controls:

- Treat peer-controlled datagrams, packet headers, frames, transport parameters,
  TLS handshake data, and connection identifiers as untrusted. Bound parsing,
  buffering, CPU, memory, connection state, and attacker-driven work.
- Preserve peer authentication, packet protection, key separation and updates,
  nonce uniqueness, transport state, address validation, and anti-amplification
  rules. Review replay, migration, loss recovery, and stream/resource limits
  when those behaviors are introduced.
- Protect private keys, handshake and traffic secrets, randomness, and sensitive
  plaintext. Keep them out of logs, errors, serialization, and public evidence.
- Validate caller configuration and the browntls integration boundary. Security
  failures must return safe errors rather than bypass verification, silently
  weaken policy, or expose remotely triggerable panics.
- Review ownership, lifetime, concurrency, cancellation, cleanup, privilege,
  and memory safety across runtime, OS, unsafe, and FFI boundaries if introduced.
- Preserve dependency, build, tool-download, signing, and release integrity.

Severity depends on demonstrated reachability, attacker prerequisites,
configuration, and impact. Development status does not suppress meaningful
findings. A dependency issue matters when the project's actual use is affected.

## Private reporting

Use [GitHub private vulnerability reporting](https://github.com/OxiBelt/qquic/security/advisories/new)
when enabled. Repository documentation alone does not enable this GitHub setting.
If the private route is unavailable, wait for maintainers to restore it; do not
substitute a public issue, PR, discussion, commit, or CI artifact containing
undisclosed details.

Include the affected immutable revision, platform and configuration, attacker
prerequisites, impact, and a minimal safe reproduction when available. Sanitize
logs and omit production credentials and secret keys. This policy does not
authorize testing systems you do not own or operate.

## Response and disclosure

Maintainers validate reports and coordinate remediation privately, including
with browntls or other upstreams when applicable. There are no guaranteed
response deadlines or fixed embargo periods. Credit reporters with consent.

Use one private repository advisory per validated report-worthy finding. Keep
it in private triage or draft until a supported fix or actionable mitigation
is verified and disclosure is explicitly authorized. Preview the exact payload
and obtain explicit approval before each advisory submission or publication.
Verify repository, revision, duplicates, identity, permissions, and visibility;
read back writes. Do not request a CVE unless explicitly authorized.

No private evidence repository is designated. Verify an authorized private
destination before retaining or uploading evidence. Keep undisclosed inputs,
findings, private evidence links, and secrets out of public contribution
messages, CI logs, caches, and artifacts. Follow the signing and assistance
requirements in [CONTRIBUTING.md](CONTRIBUTING.md).

## Current limitations

Scaffold builds and repository checks establish neither QUIC correctness nor
TLS integration safety. No security scan or protocol assessment is claimed by
this initialization. Review and test each trust boundary as implemented, and
report unreviewed surfaces and unrun checks explicitly.
