# constitution-gate

**Architecture-level CI Guards for AI-native systems**

This repository provides reusable GitHub Actions that enforce
constitutional boundaries in long-lived AI systems.

## Included Gates

### External Tools Boundary Guard

Prevents external execution frameworks (CrewAI, LangChain, etc.)
from leaking into protected architectural layers such as:

- Method layer
- Skill layer
- Core abstractions

## Philosophy

- System > Tool
- Constitution > Convenience
- CI failure = Law enforcement

## Usage Example

```yaml
- uses: liye-ai/constitution-gate/.github/actions/external-tools-boundary@v1
  with:
    protected_paths: "src/method,src/skill"
    forbidden_patterns: "crewai,CrewAI,langchain"
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `protected_paths` | Comma-separated list of protected directories | `src/method,src/skill` |
| `forbidden_patterns` | Comma-separated list of forbidden patterns | `crewai,CrewAI,langchain,LangChain,AutoGPT,...` |
| `exclude_dirs` | Comma-separated list of excluded directories | `node_modules,dist,.git` |

## License

Apache 2.0

---

**"A system that depends on tools will eventually be owned by them."**
