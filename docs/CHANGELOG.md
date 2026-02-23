# Changelog

All notable changes to bolt-rendezvous are documented here. Newest first.

## [rendezvous-v0.0.5-trust-boundary] - 2026-02-23

### Added
- Trust boundary limits for all untrusted input:
  - `MAX_MESSAGE_BYTES` (1 MiB) — WebSocket message size cap
  - `MAX_DEVICE_NAME_BYTES` (256) — `Register.device_name` field cap
  - `MAX_PEER_CODE_BYTES` (16) — `Register.peer_code` and `Signal.to` cap
  - `RATE_LIMIT_PER_SECOND` (50) — per-connection message rate
  - `RATE_LIMIT_CLOSE_THRESHOLD` (3) — consecutive violations before socket close
- Protocol-level enforcement via `WebSocketConfig.max_message_size` and
  `max_frame_size` (first-line defense at tungstenite layer).
- Pure validation helpers: `validate_message_size()`, `validate_device_name()`,
  `validate_signal_target()`.
- `RateLimit` struct with fail-closed behavior (closes socket after 3
  consecutive violations). Applied in both registration and message loops.
- Binary frame rejection in both registration and message loops (signaling
  is text-only).
- 21 new unit tests for validation helpers, rate limiter (with tokio
  `time::pause`/`advance`), constants sanity, and WebSocketConfig verification.
- `tokio` `test-util` dev-dependency for deterministic time control in tests.

## [rendezvous-v0.0.4-docs] - 2026-02-22

### Added
- Documentation sync (Phase 4B signaling-only clarification).

## [rendezvous-v0.0.3-ci] - 2026-02-22

### Added
- Rust CI workflow (fmt, clippy, test).

## [rendezvous-v0.0.2-naming] - 2026-02-22

### Changed
- Renamed crate and binary to `bolt-rendezvous`.

## [rendezvous-v0.0.1] - 2026-02-21

### Added
- Initial WebSocket signaling server.
- IP-based room grouping for local-network device discovery.
- WebRTC signaling relay between peers in the same room.
- Peer registration, signal forwarding, and disconnect cleanup.
- Private IP detection (RFC 1918, CGNAT, IPv6 ULA/link-local).
- 7 protocol tests (serde roundtrip).
