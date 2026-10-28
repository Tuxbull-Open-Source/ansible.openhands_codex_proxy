# Repository instructions

This repository provides `tuxbull.openhands_codex_proxy` for an OpenHands and
Codex-compatible proxy deployment on EL 10. It deploys containers, local TLS,
nginx, and a runtime preview reverse-proxy path; treat all of those as
security-sensitive.

Never commit Basic Auth passwords, Codex OAuth material, device codes,
`auth.json`, certificates, inventories, or lab artifacts. Basic Auth passwords
must be supplied through Ansible Vault or an approved secret manager. Keep
secret-bearing tasks `no_log` and do not print credentials in task output.

Validate YAML, syntax, nginx configuration, Docker service/compose state, local
HTTPS and proxy health, and second-run idempotency in the default disposable
Linux lab. A successful local health endpoint does not prove Codex OAuth or a
billable model-completion request; document that boundary.

`README.md` is user guidance and `CHANGELOG.md` is release history. Source is
`Tuxbull-Open-Source/ansible.openhands_codex_proxy`; Galaxy FQN is
`tuxbull.openhands_codex_proxy`. Do not self-merge. After merge, create an
immutable SemVer tag and GitHub Release, then import and verify both unpinned
and version-pinned Galaxy installations.
