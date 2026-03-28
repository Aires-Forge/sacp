# SACP — Secure Agent Communication Protocol

> An open, framework-agnostic protocol for agent-to-agent communication.

**Version:** 0.1.0 (draft)
**Status:** Pre-release — specification in progress
**License:** MIT
**Author:** Aires Noronha

---

## What Is SACP?

SACP is a protocol specification for communication between AI agents. It defines how agents identify themselves, how they establish trust, how they exchange messages, and how they stay secure — without dictating what the agents do with those messages internally.

SACP is to agent communication what HTTP is to document transfer: a standard that any implementation can speak, regardless of the underlying framework, language, or architecture.

---

## What SACP Defines

- Message envelope format
- Identity and key management
- Trust establishment between agents
- Message signing and verification
- Replay attack prevention
- Message types and their semantics
- Transport requirements
- Content labeling requirements for security

## What SACP Does NOT Define

- How a receiving agent processes messages internally
- Whether tool requests are executed, queued, or rejected
- Priority systems, tick loops, or runtime architecture
- Prompt assembly or LLM interaction
- Memory, learning, or cognitive features

These are implementation concerns. SACP is the wire protocol only.

---

## Design Principles

1. **Framework-agnostic** — any agent runtime can implement SACP
2. **Human-first trust** — trust between agents is established by humans, not agents
3. **Security by default** — unsigned messages are rejected; replay attacks are prevented at protocol level
4. **Implementation freedom** — the protocol defines capabilities; implementations decide how to use them
5. **Minimal surface** — as few required fields as possible; extensible via optional fields
6. **No lock-in** — MIT licensed; use it, implement it, build on it freely

---

## Reference Implementation

**Lumen** is the reference implementation of SACP. Lumen's inter-agent communication module is built on SACP and serves as the canonical example of how a full implementation works — including trust management, injection shielding, and tool request handling.

→ [Lumen project](https://github.com/airesnoronha/lumen) *(coming soon)*

---

## Specification

See [SPEC.md](./SPEC.md) for the full protocol specification.

---

*Created by Aires Noronha. Coordinated by Meridian.*
