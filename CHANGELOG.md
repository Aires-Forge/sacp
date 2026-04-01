# SACP Changelog

All notable changes to the SACP specification are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Version numbers follow [Semantic Versioning](https://semver.org/).

---

## [0.2.0] — 2026-04-01

### Added

- **Section 5.3 — Structured error format**
  The `error` field in `tool-response` now has a defined schema: `{code, message, detail}`.
  `code` is a machine-readable UPPER_SNAKE_CASE identifier. `message` is human-readable.
  `detail` is an optional free object for structured context (stack traces, exit codes, etc.).
  `error` MUST be `null` when `status` is `success` or `pending`.

- **Section 5.5 — Handoff context size guidance**
  `context` SHOULD be a concise summary, not a raw history dump.
  Recommended maximum: 32KB. Implementations SHOULD enforce a size cap and reject or
  truncate oversize payloads. Rationale: unbounded context is a DoS vector and can exhaust
  the receiving agent's context window.

- **Section 5.6 — ping/pong timeout defaults**
  Receivers MUST reply to a `ping` immediately.
  Senders SHOULD send a `ping` after 60 seconds of connection inactivity.
  Senders SHOULD consider a connection dead if no `pong` is received within 30 seconds.
  On dead connection: close and attempt reconnection.

- **Section 6.5 — Trust anchoring and instance identity**
  Trust is anchored to `agentId` + public key fingerprint — not to `instanceId`.
  `instanceId` is routing/session metadata only, not a trust boundary.
  Key rotation (new fingerprint) triggers a full re-verification flow (first contact).
  Receivers MUST NOT reject messages solely because `to.instanceId` is stale or null.

- **Section 8.5 — Connection loss and pending request recovery**
  New `tool-status` message type for querying outstanding request state after reconnection.
  Senders SHOULD persist `pending` request state to durable storage.
  Receivers MUST reply with current status or `REQUEST_NOT_FOUND` error code.
  Explicit scope note: best-effort recovery path, not a durability guarantee.

### Changed

- **Section 4.1 — `to.instanceId` now explicitly optional**
  Field type updated to `string (UUID v4) or null`. Senders MAY send `null` when the
  receiver's current instance is not known (first contact, post-restart).

- **Section 4.3 — Signature canonicalization mandates RFC 8785 (JCS)**
  Replaced the ambiguous `JSON.stringify` note with a mandatory requirement to use
  JSON Canonicalization Scheme (JCS / RFC 8785). Native `JSON.stringify` does not sort
  keys and produces different output across languages — breaking cross-implementation
  signature verification. JCS guarantees deterministic, UTF-8, lexicographically sorted
  output at every nesting level.
  Reference: https://github.com/cyberphone/json-canonicalization

- **Section 5.3 — `pending` declared non-terminal**
  A `pending` `tool-response` MUST be followed by a terminal response (`success`, `error`,
  or `rejected`) referencing the same `requestId`. Senders MUST retain request state until
  a terminal response arrives or `ttl` expires. An optional `timeout` hint (seconds) MAY
  be included in the `pending` response content.

- **Section 9 — Envelope `sacp` version field updated**
  Envelopes MUST use `"sacp": "0.2"` for implementations targeting this spec version.

- **README — Ethos Engine repository link updated**
  Link updated to `https://github.com/Aires-Forge/ethos-engine`.

### Removed

- **Section 11 (Compliance) — Circuit breaker removed**
  Circuit breaking is a runtime resilience pattern, not a protocol concern. Its presence
  in the compliance section incorrectly implied non-compliance for implementations that
  omit it. Removed entirely.

---

## [0.1.0] — 2026-03-28

### Added

- Initial specification draft
- Identity model: Ed25519 key pairs, agentId, instanceId, fingerprint
- Message envelope format with required and optional fields
- Message types: `message`, `tool-request`, `tool-response`, `event`, `handoff`, `ping`/`pong`
- Trust model: human-established, per-agent-pair, four trust levels
- Security: replay prevention (nonce + timestamp), signature verification, content labeling, injection defense
- Transport: WebSocket over TLS (`wss://`), local in-process event bus
- Versioning and extensibility rules
- Compliance checklist
- Appendices: example exchange, rationale for Ed25519, rationale for WebSocket

---

*Specification by Aires Noronha. Reference implementation: Ethos Engine.*
