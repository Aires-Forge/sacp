# SACP — Status

**Phase:** Draft specification
**Last updated:** 2026-03-28
**MC Project:** MC-15

---

## Current State

- ✅ Project created (MC-15)
- ✅ README written
- ✅ SPEC.md v0.1.0 draft written
  - Identity + key model (Ed25519)
  - Message envelope format
  - 6 message types: message, tool-request, tool-response, event, handoff, ping/pong
  - Trust model (human-first, per-agent-pair)
  - Security: replay prevention, signature verification, content labeling, injection defense
  - Transport: WebSocket/TLS remote, EventEmitter local
  - Versioning + extensibility
  - Compliance checklist
- ⬜ GitHub repo created (public)
- ⬜ Reviewed and finalized
- ⬜ Reference implementation (Lumen Phase 5)

## Key Decisions

- **License:** MIT — protocol specs should have zero friction to adopt
- **Transport:** WebSocket/TLS only for remote. No HTTP REST.
- **Signing:** Ed25519 fixed — no algorithm negotiation, no weak parameter risk
- **Trust:** Human-established always — agents cannot self-authorize
- **Tool requests:** Protocol defines the capability; implementation decides execution policy
- **Security:** Content labeling and injection defense are implementation responsibilities, not protocol enforcement

## Next Actions

1. Create GitHub repo: `sacp-protocol` under `airesnoronha`
2. Final review of SPEC.md before publishing
3. Reference implementation ships with Lumen Phase 5

## Relationship to Lumen

SACP is a standalone protocol. Lumen is the reference implementation.
Lumen's SACP module implements this spec + adds Lumen-specific delivery (tick queue, P1/P4 priority, injection shield patterns).
