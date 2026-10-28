# OpenHands Codex proxy role

Deploy OpenHands behind nginx HTTPS with `codex-as-api` as an OpenAI-compatible
Codex OAuth proxy on Enterprise Linux 10.

## Requirements

- Ansible 2.15 or later.
- Rocky/RHEL-compatible EL 10 host with root privileges.
- Network access to Docker CE, npm, and required container registries.
- A secret Basic Auth password when `openhands_codex_proxy_do_basic_auth: true`.
- A Codex OAuth login on the target before real model calls are expected.

## Important variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `openhands_codex_proxy_fqdn` | target FQDN | HTTPS hostname and runtime URLs. |
| `openhands_codex_proxy_do_basic_auth` | `true` | Protect main nginx virtual host. |
| `openhands_codex_proxy_basic_auth_user` | `openhands` | Basic Auth username. |
| `openhands_codex_proxy_basic_auth_password` | empty | Required secret when Basic Auth is enabled. |
| `openhands_codex_proxy_runtime_access_mode` | `allowlist` | `allowlist` or `public` runtime preview policy. |
| `openhands_codex_proxy_runtime_allowed_cidrs` | RFC1918 CIDRs | Networks allowed to use `/runtime/`. |

Supply the password through Ansible Vault or an approved secret manager. The
role has no usable shipped password.

## Example

```yaml
- hosts: openhands
  become: true
  vars_files: [vault.yml]
  roles:
    - role: tuxbull.openhands_codex_proxy
      vars:
        openhands_codex_proxy_basic_auth_password: "{{ vault_openhands_password }}"
```

## Operations and security boundary

The main UI is protected by nginx Basic Auth. The `/runtime/<port>/...` sandbox
path bypasses that Basic Auth so OpenHands runtime and preview traffic works; by
default it is restricted to `openhands_codex_proxy_runtime_allowed_cidrs`. Do
not set `public` unless an approved outer access-control layer protects it.

Complete Codex device login directly on the target after deployment:

```bash
sudo env CODEX_HOME=/opt/openhands-codex-proxy/codex /usr/local/bin/codex login --device-auth
sudo docker compose -f /opt/openhands-codex-proxy/docker-compose.yml restart codex-proxy openhands
```

Never commit the resulting `auth.json` or device-auth code. The role validates
nginx and local stack/health endpoints but does not perform OAuth or a billable
model-completion request.
