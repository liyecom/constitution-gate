# constitution-gate

**Architecture-level CI Guards for AI-native systems**

This repository provides reusable GitHub Actions that enforce
constitutional boundaries in long-lived AI systems.

## Governance Stack

```
LiYe Governance Stack
┌─────────────────────────────────────────┐
│  constitution-gate@v1                   │
├─────────────────────────────────────────┤
│  external-tools-boundary                │
│  - Prevents external frameworks in      │
│    protected layers                     │
├─────────────────────────────────────────┤
│  layer-dependency                       │
│  - Prevents forbidden cross-layer       │
│    imports                              │
└─────────────────────────────────────────┘
```

## Included Gates

### 1. External Tools Boundary Guard

Prevents external execution frameworks (CrewAI, LangChain, etc.)
from leaking into protected architectural layers.

```yaml
- uses: liyecom/constitution-gate/.github/actions/external-tools-boundary@v1
  with:
    protected_paths: "src/method,src/skill"
    forbidden_patterns: "crewai,CrewAI,langchain,LangChain"
    exclude_dirs: "node_modules,dist,.git"
```

| Input | Description | Default |
|-------|-------------|---------|
| `protected_paths` | Protected directories | `src/method,src/skill` |
| `forbidden_patterns` | Forbidden patterns | `crewai,CrewAI,...` |
| `exclude_dirs` | Excluded directories | `node_modules,dist,.git` |

### 2. Layer Dependency Guard

Prevents forbidden cross-layer dependencies in layered architecture.

```yaml
- uses: liyecom/constitution-gate/.github/actions/layer-dependency@v1
  with:
    layer_rules: |
      [
        {"from": "src/method", "to": "src/runtime", "pattern": "from.*runtime"},
        {"from": "src/skill", "to": "src/domain", "pattern": "from.*domain"}
      ]
    file_extensions: "ts,js,py"
```

| Input | Description | Default |
|-------|-------------|---------|
| `layer_rules` | JSON array of forbidden dependencies | See action.yml |
| `file_extensions` | File extensions to check | `ts,js,py` |
| `exclude_dirs` | Excluded directories | `node_modules,dist,.git` |

## Philosophy

- **System > Tool** — Don't let tools own your architecture
- **Constitution > Convenience** — Rules before shortcuts
- **CI failure = Law enforcement** — Governance gates block, never warn

## Emergency Bypass

There is no runtime bypass. To change a rule:

1. Modify the constitutional document (e.g., `docs/architecture/ARCHITECTURE.md`)
2. Get governance owner approval
3. Merge constitution change first
4. Then merge your code

See: [CI_GOVERNANCE.md](https://github.com/liyecom/liye-ai/blob/main/docs/architecture/CI_GOVERNANCE.md)

## License

Apache 2.0

---

**"A system that depends on tools will eventually be owned by them."**
