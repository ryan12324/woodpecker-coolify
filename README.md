# woodpecker-coolify

Coolify-ready Docker Compose for [Woodpecker CI](https://woodpecker-ci.org/) (server + agent), deployed at `https://ci.ryancreates.co.uk`.

## Services

| Service | Image | Role |
|---------|-------|------|
| `woodpecker-server` | `woodpeckerci/woodpecker-server:v3` | UI + API (port 8000), agent gRPC (9000) |
| `woodpecker-agent` | `woodpeckerci/woodpecker-agent:v3` | Docker backend agent (max 2 workflows) |

## Environment variables (Coolify)

| Key | Required | Notes |
|-----|----------|-------|
| `WOODPECKER_HOST` | yes | `https://ci.ryancreates.co.uk` |
| `WOODPECKER_ADMIN` | yes | GitHub username(s), e.g. `ryan12324` |
| `WOODPECKER_AGENT_SECRET` | yes | Shared secret (openssl rand -hex 32); keep private |
| `WOODPECKER_GITHUB_CLIENT` | for login | GitHub OAuth App **Client ID** |
| `WOODPECKER_GITHUB_SECRET` | for login | GitHub OAuth App **Client Secret** |

Without GitHub OAuth, the UI may start but login will not work.

## GitHub OAuth checklist (OAuth2 App, not GitHub App)

1. GitHub → Settings → Developer settings → **OAuth Apps** → New OAuth App
2. **Homepage URL:** `https://ci.ryancreates.co.uk`
3. **Authorization callback URL:** `https://ci.ryancreates.co.uk/authorize`
4. Copy Client ID / Client Secret into Coolify env vars above
5. Redeploy the Coolify application

## DNS

Point `ci.ryancreates.co.uk` at Coolify (Cloudflare **CNAME** to the Coolify host / same target as other `*.ryancreates.co.uk` apps). Until DNS resolves, the domain will not reach Traefik.

## Coolify domain notes

Use `http://ci.ryancreates.co.uk` in Coolify `docker_compose_domains` (HTTP scheme) when Cloudflare is Flexible SSL, to avoid Traefik 307 loops — same pattern as BookTimeWith.

`is_force_https_enabled` should be false for that pattern.

## Local reference

Compose lives at repo root (`/docker-compose.yml`). Do not commit `agent.secret`.
