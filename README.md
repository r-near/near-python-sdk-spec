# NEAR Python Tooling Specification

This repository contains the specification for Python tooling in the NEAR ecosystem. The goal is to create a comprehensive, Pythonic development experience for NEAR smart contracts.

## Overview

The NEAR Python Tooling initiative aims to make it as easy as possible for Python developers to build, test, and deploy smart contracts on NEAR Protocol.

```mermaid
flowchart TD
    Python["Python Developer"]

    subgraph Tools ["Core Tools"]
        SDK["Python SDK<br>(near-sdk-py)"]
        Nearc["Compiler<br>(nearc)"]
        Testing["Testing Framework<br>(near-pytest)"]

        SDK --> Nearc
        SDK --> Testing
    end

    Python --> Tools
    Nearc --> WASM["contract.wasm"]
    WASM --> Deploy["NEAR Blockchain"]
    Testing --> Feedback["Developer Feedback"]
    Feedback -- Iterate --> Python

    style Python fill:#f5f5ff,stroke:#333,stroke-width:2px
    style Deploy fill:#f5fff5,stroke:#333,stroke-width:2px
    style Tools fill:#fffff5,stroke:#333,stroke-width:1px
    style Feedback fill:#fff5f5,stroke:#333,stroke-width:2px
```

This specification outlines:

- A compiler for Python → WASM conversion
- A comprehensive SDK for contract development
- Testing frameworks for both unit and integration tests
- Developer tools for improved workflow

## Documents

- [Architecture Overview](architecture-overview.md) - Visual explanation of how components fit together
- [Developer Specification](developer-spec.md) - Skimmable guide with practical examples
- [Detailed Specification](detailed-spec.md) - Comprehensive specification with implementation details

## Key Features

- **Python-first approach**: Leverage Python's simplicity and extensive ecosystem
- **Full development lifecycle**: Write, test, and deploy contracts in a seamless workflow
- **Testing frameworks**: Both unit testing and integration testing support
- **Developer ergonomics**: Familiar patterns and tools for Python developers
- **Interoperability**: Works with existing NEAR tools and standards

## Current Status

This is a draft specification intended to gather feedback from the community. No code has been implemented yet.

## Components Overview

| Component              | Purpose                        | Status  |
| ---------------------- | ------------------------------ | ------- |
| **nearc**              | Python to WASM compiler        | Planned |
| **near-sdk-py**        | Smart contract development SDK | Planned |
| **near-pytest**        | Unit testing framework         | Planned |
| **near-sandbox**       | Local NEAR node                | Planned |
| **near-workspaces-py** | Integration testing            | Planned |
| **near-py-vm**         | Fast simulation environment    | Future  |

## Feedback

Please open an issue in this repository with any feedback or suggestions.
