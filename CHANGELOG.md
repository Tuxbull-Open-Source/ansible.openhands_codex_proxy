# Changelog

## v1.0.1 - 2026-10-27

### Security

- Remove the shipped usable nginx Basic Auth password and require a secret input
  when Basic Auth is enabled.

### Changed

- Migrate Galaxy identity to `tuxbull.openhands_codex_proxy`.
- Align task dispatch with the current Enterprise Ansible Role Template.
- Add early input validation and release documentation.

### Verified

- Disposable Rocky Linux 10 role deployment, nginx configuration, Docker
  service, and idempotency validation.
