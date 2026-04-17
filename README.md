# SysML v2 Containerized Tools

This repository provides containerized build environments for SysML v2 tooling, making it easy to use these tools without installing their dependencies on your host system.

## Tools

### sysml2

A fast, C-based SysML v2 and KerML parser with semantic validation, CLI tooling, and JSON output.

- **Source**: [zbigniewsobiecki/sysml2](https://github.com/zbigniewsobiecki/sysml2)
- **Features**: PEG parser (PackCC), arena memory allocation, Clang-style diagnostics, semantic validation, query/modify APIs
- **Usage**: Parse, validate, and transform SysML v2 and KerML files

### spec42

A Rust-based SysML v2 language server and CLI with full LSP support.

- **Source**: [elan8/spec42](https://github.com/elan8/spec42)
- **Features**: LSP server (diagnostics, hover, completion, go to definition, find references, rename, etc.), CLI validation, bundled standard library
- **Usage**: Editor integration (VS Code), CLI validation, CI/CD pipelines

### sysml-v2-lsp

A TypeScript-based SysML v2 language server powered by ANTLR4 with MCP server support.

- **Source**: [daltskin/sysml-v2-lsp](https://github.com/daltskin/sysml-v2-lsp)
- **Features**: LSP server (diagnostics, hover, completion, go to definition, find references, rename, semantic tokens), MCP server for AI integration, complexity analysis, Mermaid diagram generation
- **Usage**: Editor integration, AI-assisted modeling, CLI tooling

## Use Cases

| Task | Recommended Tool |
|------|------------------|
| Fast CLI parsing and validation | `sysml2` |
| Editor integration with IDE features | `spec42` or `sysml-v2-lsp` |
| JSON output for downstream processing | `sysml2` |
| Automated validation in CI/CD | `spec42` |
| Query and modify existing models | `sysml2` |
| Language server for VS Code | `spec42` |
| MCP server for AI agents | `sysml-v2-lsp` |
| Complexity analysis and diagrams | `sysml-v2-lsp` |

## Quick Start

### Build the Containers

```bash
# Build sysml2 container
podman build -t sysml2 -f sysml2/Containerfile .

# Build spec42 container
podman build -t spec42 -f spec42/Containerfile .

# Build sysml-v2-lsp container
podman build -t sysml-v2-lsp -f sysml-v2-lsp/Containerfile .
```

### Running Against SysML Files

**Using sysml2:**

```bash
# Validate a single file
podman run -v "$(pwd):/workspace" --rm sysml2 model.sysml

# Parse and output JSON
podman run -v "$(pwd):/workspace" --rm sysml2 -f json model.sysml

# Specify library paths for import resolution
podman run -v "$(pwd):/workspace" --rm sysml2 -I /path/to/library model.sysml

# Fix formatting in place
podman run -v "$(pwd):/workspace" --rm sysml2 --fix model.sysml
```

**Using spec42:**

```bash
# Validate a file
podman run -v "$(pwd):/workspace" --rm spec42 check model.sysml

# Validate with JSON output (for CI)
podman run -v "$(pwd):/workspace" --rm spec42 check model.sysml --format json

# Check a directory
podman run -v "$(pwd):/workspace" --rm spec42 check ./models

# Diagnose configuration
podman run --rm spec42 doctor

# Get version
podman run --rm spec42 --version

# Start LSP server (for editor integration)
podman run -v "$(pwd):/workspace" --rm -i spec42 lsp
```

### Interactive Editor Integration

For VS Code with spec42, you can mount the spec42 binary:

```bash
# Option 1: Make spec42 available on PATH in your host
podman run --rm spec42 > /usr/local/bin/spec42
chmod +x /usr/local/bin/spec42

# Option 2: Configure VS Code to use the containerized spec42
# Set "spec42.serverPath" to point to the container or copy
```

**Using sysml-v2-lsp:**

```bash
# Start LSP server (for editor integration)
podman run -v "$(pwd):/workspace" --rm -i sysml-v2-lsp sysml-lsp

# Start MCP server (for AI agent integration)
podman run -v "$(pwd):/workspace" --rm -i sysml-v2-lsp sysml-mcp
```

## Directory Structure

```
.
├── LICENSE          # MIT License
├── README.md        # This file
├── sysml2/          # sysml2 container
│   └── Containerfile
├── spec42/          # spec42 container
│   └── Containerfile
└── sysml-v2-lsp/    # sysml-v2-lsp container
    └── Containerfile
```

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) for details.

The individual tools have their own licenses:
- sysml2: MIT License
- spec42: MIT License
- sysml-v2-lsp: MIT License
- PackCC: MIT License (included in sysml2 build)

The SysML v2 standard library embedded in spec42 has separate licensing; see [spec42's Third Party Notices](https://github.com/elan8/spec42/blob/main/THIRD_PARTY_NOTICES.md).