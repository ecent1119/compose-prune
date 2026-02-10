# compose-prune

Clean up unused volumes, networks, and services from your compose files.

---

## The problem

- Docker Compose files accumulate unused volumes over time
- Networks defined but never referenced by any service
- Services with profiles that no longer make sense
- "Is this volume still used?" requires manual searching

---

## What it does

- Scans compose files for **unused volumes**
- Detects **orphaned networks** not referenced by services
- Finds **orphaned services** (profile-gated with no dependencies)
- Generates a **clean version** with dead resources removed
- Outputs report of what can be safely removed

---

## New in v2.0

- **Configs/secrets detection** — find unused configs and secrets definitions
- **Extension detection** — find unused `x-` extension keys
- **Per-profile reports** — see what each profile adds
- **Keep/ignore patterns** — protect specific resources from being flagged
- **Patch output** — generate patch files for removal operations

---

## Example output

```
$ compose-prune analyze

Compose Cleanup Analysis
========================

📦 Unused Volumes:
  - legacy_data (defined but not mounted by any service)
  - old_cache (defined but not mounted by any service)

🌐 Unused Networks:
  - monitoring (defined but no services attached)
  - debug_net (defined but no services attached)

👻 Orphaned Services:
  - debug-tools (profile: debug, no other services depend on it)

Summary:
  - 2 unused volumes (can save disk space)
  - 2 unused networks (can remove safely)
  - 1 orphaned service (consider removing)

Run with --output to generate cleaned file
```

---

## Commands

```bash
# Analyze current directory
compose-prune analyze

# Analyze specific file
compose-prune analyze -f docker-compose.yml

# Analyze with specific profiles active
compose-prune analyze --profile debug --profile testing

# Generate cleaned file
compose-prune analyze --output cleaned-compose.yml

# JSON output for scripting
compose-prune analyze --format json

# Strict mode (exit error if issues found)
compose-prune analyze --strict
```

---

## What it detects

| Resource | Detection |
|----------|-----------|
| **Volumes** | Defined in `volumes:` but not in any service `volumes:` mount |
| **Networks** | Defined in `networks:` but not in any service `networks:` |
| **Services** | Only active via profile, no other services depend on them |
| **External** | Marks external resources as "keep" (managed elsewhere) |

---

## Safe by default

- **External resources** are never flagged for removal
- **Profiled services** are only flagged if no dependencies exist
- **Named volumes with data** are warned about (requires --force to include)
- Generates new file, never modifies original

---

## Use cases

- **Cleanup**: Remove cruft from long-lived compose files
- **Auditing**: Understand what resources are actually used
- **Migration**: Simplify before moving to production
- **Documentation**: Generate minimal effective compose

---

## Scope

- Local development and testing only
- Read-only analysis of compose files
- No Docker daemon interaction
- No actual resource deletion
- No telemetry, no network calls

---

## Common problems this solves

- "find unused docker volumes"
- "cleanup docker compose file"
- "remove orphan networks docker"
- "docker compose cleanup unused"
- "check if volume still used docker"
- "dead resources docker compose"
- "clean up old docker compose definitions"

---

## Get it

👉 [Download on Gumroad](https://ecent.gumroad.com/l/tfvsfyt)

---

## Related tools

| Tool | Purpose |
|------|---------|
| **[stackgen](https://github.com/ecent119/stackgen)** | Generate Docker Compose stacks |
| **[compose-flatten](https://github.com/ecent119/compose-flatten)** | Merge compose files |
| **[compose-diff](https://github.com/ecent119/compose-diff)** | Compare compose files |
| **[dataclean](https://github.com/ecent119/dataclean)** | Manage volume snapshots |

---

If this tool saved you time, consider starring the repo.

---

## License

MIT — this repository contains documentation and examples only.
