# SysML v2 Tools - TODO

## Completed Containers

| Tool | Status | Notes |
|------|--------|-------|
| sysml2 | ✅ Done | C-based parser |
| spec42 | ✅ Done | Rust LSP |
| sysml-v2-lsp | ✅ Done | TypeScript ANTLR4 LSP |
| sysand | ✅ Done | Package manager |
| windseeker | ✅ Done | Dependency analysis CLI |

## Known Issues

### windtrader

**Problem:** Container build fails because `windtrader` is not published to PyPI.

```
ERROR: No matching distribution found for windtrader
```

**Containerfile:** Created at `windtrader/Containerfile`

**Options to fix:**
1. Install directly from GitHub: Already attempted with `pip install git+https://github.com/Westfall-io/windtrader.git` - needs verification
2. Alternative: Clone and install locally in the Containerfile
3. Check if package name is different on PyPI

**Reference:** https://github.com/Westfall-io/windtrader

---

### sysmod-sysml2-api

**Problem:** Complex dependencies and requires external services.

**Containerfile:** Not created

**Requirements:**
- Running SysML v2 API server (e.g., from Systems-Modeling/SysML-v2-Release)
- Python dependencies from requirements.txt
- Node.js/npm for MCP inspector
- Optional: OpenAI API key for AI features
- Many optional dependencies (Flask, mcp, openai, pypdf, python-docx)

**This tool is more suited for development environments than simple containerization.**

**Reference:** https://github.com/Open-MBEE/sysmod-sysmlv2-api

---

## Ideas for Future Containers

- **sysml2py** - Python library for constructing SysML v2 classes
  - Reference: https://github.com/Westfall-io/sysml2py
  - Available on PyPI

- **MontiCore/sysmlv2** - Java parser for SysML v2
  - Reference: https://github.com/MontiCore/sysmlv2
  - Requires Java 21 and Maven

- **syside** - Syside Automator (proprietary, see licensing)
  - Reference: https://pypi.org/project/syside/

---

## Notes

- All containers use podman and are based on Ubuntu 24.04 or Python 3.12-slim
- Multi-stage builds used where appropriate to reduce image size
- All containers pushed to GitHub after successful build and test