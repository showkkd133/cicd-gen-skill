# cicd-gen-skill

A Claude Code skill that auto-detects your project type and generates production-ready CI/CD pipelines.

## What it does

This skill analyzes your codebase — language, framework, package manager, monorepo structure — and generates optimized CI/CD configuration files with proper caching, security, and deployment steps.

### Supported Platforms

| Platform | Status |
|----------|--------|
| GitHub Actions | Full support (primary) |
| GitLab CI | Template support |
| CircleCI | Template support |

### Supported Tech Stacks

| Language | Runtimes / Frameworks |
|----------|----------------------|
| TypeScript/JavaScript | Bun, Node.js (npm/pnpm/yarn), Next.js, Vite, Express, Fastify, Hono |
| Python | pip, uv, FastAPI, Flask, Django |
| Go | Gin, Echo, Fiber, Chi |
| Rust | Cargo, Actix-web, Axum |
| Docker | Dockerfile, docker-compose |

### Deployment Targets

- Vercel
- Netlify
- AWS (ECR + ECS/EKS)
- Docker Registry (GHCR, Docker Hub)
- npm Registry
- GitHub Pages

### What it generates

- **CI workflow** — lint, type-check, test, build with proper caching
- **CD workflow** — deployment to your target platform
- **dependabot.yml** — automated dependency updates
- **Security** — minimal permissions, OIDC auth, pinned action versions
- **Monorepo support** — path filtering, affected-only builds

## Installation

### Claude Code (recommended)

Add to your Claude Code settings (`~/.claude/settings.json`):

```json
{
  "skills": {
    "cicd-gen": {
      "source": "github:showkkd133/cicd-gen-skill"
    }
  }
}
```

Or copy `skills/cicd-gen/SKILL.md` to `~/.claude/skills/cicd-gen/SKILL.md`.

### Manual

1. Clone this repo
2. Copy the skill file:
   ```bash
   cp skills/cicd-gen/SKILL.md ~/.claude/skills/cicd-gen/SKILL.md
   ```

## Usage

In Claude Code, just say:

- "generate ci" / "生成 ci"
- "github actions" / "配置流水线"
- "setup ci/cd pipeline"
- "add github workflow"

The skill will:
1. Detect your project type, framework, and package manager
2. Choose the optimal pipeline pattern (lint -> test -> build -> deploy)
3. Generate `.github/workflows/ci.yml` with proper caching
4. Optionally generate deployment workflow
5. Optionally generate `.github/dependabot.yml`
6. Validate with `actionlint`
7. List required GitHub Secrets

## Cache Strategy

| Stack | Cache Method | Key |
|-------|-------------|-----|
| Bun | Built-in | `bun.lockb` |
| npm | `setup-node` + `cache: 'npm'` | `package-lock.json` |
| pnpm | `setup-node` + `cache: 'pnpm'` | `pnpm-lock.yaml` |
| pip | `setup-python` + `cache: 'pip'` | `requirements*.txt` |
| Go | `setup-go` + `cache: true` | `go.sum` |
| Rust | `Swatinem/rust-cache` | `Cargo.lock` |
| Docker | BuildKit GHA cache | Layer hash |

## License

MIT
