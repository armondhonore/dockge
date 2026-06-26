# Nexlayer — dockge

<!-- nexlayer:meta version=1 analyzed=2026-06-26T04:28:45Z repo=https://github.com/armondhonore/dockge branch=master -->

> **For AI agents (Claude Code, Cursor, Gemini CLI, Copilot):**
> This file is the **project context** for this Nexlayer deployment — tech stack, env vars, secrets, live URL.
> For full platform detail (nexlayer.yaml schema, Dockerfile rules, CI/CD, task recipes) read **`nexlayer.skills`** in this repo.
>
> **Critical rules (full detail in `nexlayer.skills`):**
> - Inter-pod refs: `${podName:port}` only — never `localhost` or bare hostnames
> - Docker Hub images: prefix with `mirror.gcr.io/library/` — bare tags fail on the cluster
> - Secrets: set in the Nexlayer dashboard — never commit to `nexlayer.yaml` or Dockerfile
>
> **This file:** `agent-managed` sections update automatically. `user-editable` sections (Local Development Setup, Nexlayer Deployment Plan, Build Notes) are yours — preserved across re-analysis.

## Project Summary
<!-- nexlayer:section agent-managed=project_summary -->
Dockge is a reactive, self-hosted manager for Docker Compose stacks, allowing users to create, edit, and manage compose.yaml files with an interactive web interface and terminal.
<!-- nexlayer:end -->

## Technology Stack
<!-- nexlayer:section agent-managed=tech_stack -->
| Name | Kind | Version | Detected From |
|------|------|---------|---------------|
| Node.js | language | unknown | package.json |
| TypeScript | language | unknown | tsconfig.json |
| Docker | infra | unknown | compose.yaml, README.md |
<!-- nexlayer:end -->

## Repository Structure
<!-- nexlayer:section agent-managed=structure_map -->
- backend/ — Server-side logic and Docker API integration
- frontend/ — User interface built with TypeScript/React
- common/ — Shared types and utilities between frontend and backend
- docker/ — Containerization configurations
<!-- nexlayer:end -->

## External Services Required
<!-- nexlayer:section agent-managed=external_deps -->
Services that must be configured separately (not deployed by Nexlayer):

- Docker Engine API (Requires access to /var/run/docker.sock)
<!-- nexlayer:end -->

## Local Development Setup
<!-- nexlayer:section user-editable=local_setup -->
### Prerequisites

- Node.js
- npm or yarn
- Docker Engine

### Environment variables

Copy `.env.example` to `.env.local` and fill in:

```
DOCKGE_STACKS_DIR=/opt/stacks
```

### Steps

1. `npm install` — Install project dependencies
2. `npm run build` — Build the frontend and backend assets
3. `docker compose up -d` — Launch the application using the provided compose file

<!-- nexlayer:end -->

## Nexlayer Setup
<!-- nexlayer:section agent-managed=nexlayer_setup -->
### Pod Environment Variables

| Pod | Variable | Value | Kind |
|-----|----------|-------|------|
| `app` | `DOCKGE_STACKS_DIR` | `/opt/stacks` | plain |
| `dockge-stacks` | `mountPath` | `/opt/stacks` | plain |
| `dockge-stacks` | `size` | `5Gi` | plain |

### nexlayer.yaml

```yaml
application:
  name: dockge
  pods:
  - name: app
    image: mirror.gcr.io/louislam/dockge:1
    path: /
    servicePorts:
    - 5001
    vars:
      DOCKGE_STACKS_DIR: /opt/stacks
    volumes:
    - name: dockge-stacks
      mountPath: /opt/stacks
      size: 5Gi
```

<!-- nexlayer:end -->

## Nexlayer Deployment Plan
<!-- nexlayer:section user-editable=deployment_plan -->
### Pod Topology

| Pod | Image | Port | Role |
|-----|-------|------|------|
| dockge | mirror.gcr.io/library/node:20-alpine | 5001 | web |

### Deployment notes

- The application requires a mounted Docker socket (/var/run/docker.sock) to manage containers on the host.
- Since Dockge is a management tool for the host's Docker engine, it operates as a single-pod web service that interacts with the host OS daemon.

<!-- nexlayer:end -->

## Build Notes
<!-- nexlayer:section user-editable=build_notes -->
<!-- Add notes for future builds here — preserved across re-analysis -->
<!-- nexlayer:end -->

## Nexlayer Configuration
<!-- nexlayer:section agent-managed=nexlayer_config -->
**Last deployed:** 2026-06-26T04:29:59Z  
**Live URL:** https://relaxed-weasel-dockge.cloud.nexlayer.ai  
**Runtime:**  · **Port:** auto-detected  
**Deploy branch:** master  

```yaml
application:
  name: dockge
  pods:
  - name: app
    image: mirror.gcr.io/louislam/dockge:1
    path: /
    servicePorts:
    - 5001
    vars:
      DOCKGE_STACKS_DIR: /opt/stacks
    volumes:
    - name: dockge-stacks
      mountPath: /opt/stacks
      size: 5Gi
```
<!-- nexlayer:end -->

## Build History
<!-- nexlayer:section agent-managed=build_history -->
| Date | Status | Notes |
|------|--------|-------|
| 2026-06-26T04:28:45Z | analyzed | initial repo analysis |
| 2026-06-26T04:29:59Z | success | deployed https://relaxed-weasel-dockge.cloud.nexlayer.ai |
<!-- nexlayer:end -->
