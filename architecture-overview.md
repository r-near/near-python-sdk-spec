# NEAR Python Tooling: Architecture Overview

## System Architecture

This diagram shows the overall development flow from writing Python code to deployment on NEAR blockchain:

```mermaid
flowchart TD
    subgraph Development ["DEVELOPMENT FLOW"]
        Contract["contract.py"]
        Contract --> SDK["near-sdk-py"]
        Contract --> PyTest["near-pytest"]
    end

    subgraph Compilation ["COMPILATION"]
        Contract2["contract.py"]
        Contract2 --> Nearc["nearc"]
        Nearc --> Wasm["contract.wasm"]
    end

    Development --> Compilation

    subgraph Testing ["TESTING"]
        Wasm2["contract.wasm"]
        Wasm2 --> Sandbox["near-sandbox"]
        Wasm2 --> Workspaces["near-workspaces-py"]
        Sandbox <--> Workspaces
    end

    Compilation --> Testing

    Testing --> Blockchain["NEAR BLOCKCHAIN"]
```

## Component Relationships

How the tools connect and interact with each other:

```mermaid
flowchart TD
    SDK["near-sdk-py"] --> Pytest["near-pytest"]
    SDK --> Workspaces["near-workspaces-py"]
    SDK --> Nearc["nearc"]

    VM["near-py-vm\n(future)"] --> SDK

    Pytest --> Developer["Developer"]
    Workspaces --> Developer

    Nearc --> WASM["contract.wasm"]
    WASM --> Deploy["Deployment"]

    Workspaces --> Sandbox["near-sandbox"]
    Sandbox --> Blockchain["NEAR Blockchain"]
    Deploy --> Blockchain

    classDef core fill:#f9f,stroke:#333,stroke-width:2px
    classDef secondary fill:#bbf,stroke:#333,stroke-width:1px
    classDef future fill:#ddd,stroke:#333,stroke-width:1px,stroke-dasharray: 5 5

    class SDK,Nearc core
    class Pytest,Workspaces,Sandbox secondary
    class VM future
```

## Module Composition

How the near-sdk-py is structured internally:

```mermaid
flowchart TD
    subgraph SDK ["near-sdk-py"]
        LowAPI["Low-level API"] --> HighAPI["High-level API"]
        LowAPI --> Stubs["Stubs"]
        HighAPI --> Collections["Collections"]
    end

    style SDK fill:#f5f5ff,stroke:#333,stroke-width:2px
```

## Data Flow

How data moves through the system:

```mermaid
flowchart LR
    Contract["contract.py"] --> Nearc["nearc"]
    Nearc --> WASM["contract.wasm"]
    WASM --> Sandbox["NEAR Sandbox"]
    Sandbox --> Workspaces["near-workspaces"]
    Workspaces --> Results["Test Results"]
```

## Development Workflow

The typical developer workflow:

```mermaid
flowchart TD
    Write["Write Contract"] --> Tests["Write Tests"]
    Tests --> UnitTest["Unit Test"]
    UnitTest --> Issues["Fix Issues"]
    Issues --> Compile["Compile WASM"]
    Compile --> Deploy["Deploy"]
    Deploy --> Integration["Integration Test"]
    Integration --> Production["Production Deployment"]

    style Write fill:#d4f1c5
    style Production fill:#f9d6d2
```

## Component Breakdown

### Core Components

| Component            | Purpose                                 | Depends On              | Used By                        |
| -------------------- | --------------------------------------- | ----------------------- | ------------------------------ |
| `near-sdk-py`        | Python library for contract development | N/A                     | Developers, nearc              |
| `nearc`              | Compiler from Python to WASM            | MicroPython, Emscripten | CI/CD, Developers              |
| `near-pytest`        | Unit testing framework                  | near-sdk-py             | Developers, CI                 |
| `near-sandbox`       | Local NEAR node                         | near-cli-rs             | near-workspaces-py, Developers |
| `near-workspaces-py` | Integration testing                     | near-sandbox            | Developers, CI                 |

### Optional Components

| Component         | Purpose                      | Implementation Difficulty |
| ----------------- | ---------------------------- | ------------------------- |
| `near-py-vm`      | Fast iteration simulation    | High                      |
| Project Templates | Scaffolding for new projects | Low                       |
| CI/CD Templates   | Ready-to-use CI/CD configs   | Low                       |
| IDE Extensions    | Better tooling integration   | Medium                    |

## Implementation Priority

```mermaid
flowchart LR
    subgraph Phase1 ["Phase 1"]
        Nearc["nearc"]
        SDK["near-sdk-py\nCore API"]
    end

    subgraph Phase2 ["Phase 2"]
        PyTest["near-pytest"]
        HighLevel["Collections\n& High-Level API"]
        Sandbox["near-sandbox"]
        Workspaces["near-workspaces"]
    end

    subgraph Phase3 ["Phase 3"]
        VM["near-py-vm"]
        Templates["Templates\n& Examples"]
    end

    Nearc --> PyTest
    SDK --> HighLevel
    PyTest --> VM
    HighLevel --> Templates
    HighLevel --> Sandbox
    Sandbox --> Workspaces

    style Phase1 fill:#f9f9ff,stroke:#333
    style Phase2 fill:#f5f5ff,stroke:#333
    style Phase3 fill:#f0f0ff,stroke:#333
```
