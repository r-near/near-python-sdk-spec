# NEAR Python Tooling: Detailed Specification

This document provides a comprehensive specification for the NEAR Python tooling ecosystem, aimed at implementers and contributors.

## 1. `nearc` - NEAR Python Compiler

### Core Features

- **Simple CLI Interface**

  - [ ] Command: `nearc contract.py [options]`
  - [ ] Support for configuration file (`nearc.toml` or in `pyproject.toml`)
  - [ ] Verbose mode for debugging compilation issues
  - [ ] Output path specification
  - [ ] Version information command

- **Dependency Management**

  - [ ] Auto-detect dependencies from imports
  - [ ] Read dependencies from `pyproject.toml`
  - [ ] Support for virtual environments
  - [ ] Handle transitive dependencies
  - [ ] Configurable exclusion of packages
  - [ ] Support for package version pinning

- **MicroPython Integration**

  - [ ] Generate frozen modules manifest
  - [ ] Configure MicroPython build options
  - [ ] Support custom MicroPython modules
  - [ ] Cache MicroPython compiler for faster builds
  - [ ] Handle MicroPython version management

- **WASM Generation**

  - [ ] Compile to optimized WASM
  - [ ] Support for WASM size optimization
  - [ ] Support for WASM performance optimization
  - [ ] Include debugging symbols option
  - [ ] Generate ABI metadata

- **API for Tooling Integration**
  - [ ] Python module interface for programmatic use
  - [ ] Function to compile from string/file
  - [ ] Support callbacks for compilation progress
  - [ ] Error handling and reporting API
  - [ ] Stream compilation output

### Implementation Considerations

- Extract compilation logic from current `near-py-tool` codebase
- Ensure cross-platform compatibility (Linux, macOS, Windows)
- Consider Docker-based compilation for environment consistency
- Implement caching strategies for faster incremental builds
- Design for testability and clear error messages

### Usage Examples

```bash
# Basic compilation
nearc contract.py

# Advanced options
nearc contract.py --optimize-size --output-dir ./build --include-debug-info

# Use specific config
nearc contract.py --config nearc.toml
```

## 2. `near-sdk-py` - NEAR Python SDK

### 2.1 Low-Level API Module

- **Host Function Bindings**

  - [ ] Complete bindings for all NEAR host functions
  - [ ] Proper type annotations for all functions
  - [ ] Comprehensive docstrings with examples
  - [ ] Error handling wrappers for host functions
  - [ ] Constants for gas costs, limits, etc.

- **Core Types**

  - [ ] AccountId implementation
  - [ ] PublicKey implementation
  - [ ] Balance and Gas utility types
  - [ ] Serialization helpers
  - [ ] Base64/Binary conversion utilities

- **Context Management**
  - [ ] Transaction context access
  - [ ] Block context access
  - [ ] Account context access
  - [ ] Execution context utilities

### 2.2 High-Level API Module

- **Contract Structure**

  - [ ] Base contract class
  - [ ] Method decorators (`@view`, `@call`, etc.)
  - [ ] Contract initialization patterns
  - [ ] Contract metadata management
  - [ ] Interface definition utilities

- **Storage Abstractions**

  - [ ] Key-value storage wrapper
  - [ ] Collection types (Map, Vector, Set)
  - [ ] Serialization/deserialization utilities
  - [ ] Indexing and querying helpers
  - [ ] Lazy loading patterns
  - [ ] Storage cost management

- **Cross-Contract Communication**

  - [ ] Promise API wrappers
  - [ ] Callback management
  - [ ] Promise batching utilities
  - [ ] Promise result handling
  - [ ] Gas management for cross-contract calls

- **Token Standards Implementations**

  - [ ] NEP-141 (Fungible Token) implementation
  - [ ] NEP-171 (Non-Fungible Token) implementation
  - [ ] NEP-148 (Storage Management) implementation
  - [ ] Multi-token contracts support

- **Security Utilities**

  - [ ] Input validation helpers
  - [ ] Access control patterns
  - [ ] Reentrancy protection
  - [ ] Atomicity helpers
  - [ ] Assertion utilities

- **Logging and Events**
  - [ ] Structured logging helpers
  - [ ] Standard event emitters
  - [ ] Custom event definitions
  - [ ] Event schema validation

### Module Structure

```
near_sdk_py/
├── __init__.py
├── api/                  # Low-level API (included in WASM)
│   ├── __init__.py
│   ├── context.py
│   ├── economics.py
│   ├── promises.py
│   ├── storage.py
│   └── utils.py
├── contract/             # High-level contract abstractions
│   ├── __init__.py
│   ├── base.py
│   ├── decorators.py
│   ├── init.py
│   └── metadata.py
├── collections/          # Storage collections
│   ├── __init__.py
│   ├── map.py
│   ├── set.py
│   └── vector.py
├── token/                # Token standard implementations
│   ├── __init__.py
│   ├── fungible.py
│   └── non_fungible.py
├── security/             # Security utilities
│   ├── __init__.py
│   ├── access.py
│   └── validation.py
└── utils/                # General utilities
    ├── __init__.py
    ├── logging.py
    ├── serialization.py
    └── types.py
```

## 3. Testing Infrastructure

### 3.1 `near-pytest` - Unit Testing Framework

- **Test Framework Integration**

  - [ ] pytest plugin implementation
  - [ ] Test discovery for contract methods
  - [ ] Test fixture management
  - [ ] Test parameterization helpers
  - [ ] Console reporting

- **NEAR Runtime Simulation**

  - [ ] Mock context for transaction execution
  - [ ] State management between test cases
  - [ ] Storage simulation
  - [ ] Account identity management
  - [ ] Gas usage simulation

- **Test Decorators and Fixtures**

  - [ ] Contract initialization fixtures
  - [ ] Account fixtures
  - [ ] Transaction context fixtures
  - [ ] Storage state fixtures
  - [ ] Contract interaction fixtures

- **Assertion Helpers**

  - [ ] Contract state assertions
  - [ ] Event emission assertions
  - [ ] Gas usage assertions
  - [ ] Error/panic assertions
  - [ ] Balance change assertions

- **Coverage and Analysis**
  - [ ] Code coverage reporting
  - [ ] Gas usage reporting
  - [ ] State transition visualization
  - [ ] Test path analysis
  - [ ] Performance benchmarking

### 3.2 `near-sandbox` - Local Environment

- **Binary Management**

  - [ ] Auto-download appropriate nearcore binary
  - [ ] Version management
  - [ ] Platform-specific binary handling
  - [ ] Integrity verification
  - [ ] Update checking

- **Sandbox Configuration**

  - [ ] Network configuration templates
  - [ ] Custom genesis configuration
  - [ ] Account setup automation
  - [ ] Contract deployment automation
  - [ ] Persistent state management

- **Process Control**

  - [ ] Start/stop sandbox commands
  - [ ] Process monitoring
  - [ ] Log collection and analysis
  - [ ] RPC endpoint management
  - [ ] Health checking

- **Development Integration**
  - [ ] IDE integration support
  - [ ] CI/CD integration helpers
  - [ ] Script-friendly API
  - [ ] Environment variable configuration
  - [ ] Docker container support

### 3.3 `near-workspaces-py` - Integration Testing

- **Workspace Management**

  - [ ] Test workspace creation
  - [ ] Account management
  - [ ] Contract deployment
  - [ ] State initialization
  - [ ] Cleanup and teardown

- **Contract Interaction**

  - [ ] Call contract methods
  - [ ] Transaction batching
  - [ ] View call utilities
  - [ ] Receipt inspection
  - [ ] Result handling

- **Test Assertions**

  - [ ] Transaction outcome assertions
  - [ ] State transition assertions
  - [ ] Event emission assertions
  - [ ] Error handling assertions
  - [ ] Performance assertions

- **Multi-Contract Testing**

  - [ ] Contract interaction simulation
  - [ ] Promise chain testing
  - [ ] Complex scenario modeling
  - [ ] Protocol feature testing
  - [ ] Upgrade testing

- **Reporting and Analysis**
  - [ ] Test result reporting
  - [ ] Gas usage analysis
  - [ ] Transaction flow visualization
  - [ ] State transition graphs
  - [ ] Performance benchmarking

## 4. Developer Experience

### 4.1 `near-py-vm` (Stretch Goal)

- **Runtime Simulation**

  - [ ] Implement NEAR host function stubs
  - [ ] Support for context simulation
  - [ ] State persistence between calls
  - [ ] Gas metering simulation
  - [ ] Transaction simulation

- **Development Integration**

  - [ ] REPL for interactive testing
  - [ ] Hot reloading for code changes
  - [ ] Debugger integration
  - [ ] Breakpoint support
  - [ ] Variable inspection

- **State Inspection**

  - [ ] Storage state visualization
  - [ ] Call graph analysis
  - [ ] State transition tracking
  - [ ] Event log inspection
  - [ ] Gas usage profiling

- **Performance Optimization**
  - [ ] Fast execution for quick feedback
  - [ ] Configurable accuracy vs. speed tradeoffs
  - [ ] Selective feature simulation
  - [ ] Caching strategies
  - [ ] Parallel execution support

### 4.2 Project Templates & Scaffolding

- **Project Templates**

  - [ ] Basic contract template
  - [ ] Fungible token template
  - [ ] Non-fungible token template
  - [ ] DAO template
  - [ ] Marketplace template

- **Configuration Templates**

  - [ ] CI/CD workflow templates
  - [ ] Testing configuration
  - [ ] Deployment configuration
  - [ ] Documentation templates
  - [ ] Linting and formatting configuration

- **Documentation Generation**
  - [ ] API documentation generation
  - [ ] Contract interface documentation
  - [ ] Usage example generation
  - [ ] Deployment guide generation
  - [ ] Security considerations documentation

## Implementation Strategies

### Resolving the `import near` Typing Issue

Several approaches could be taken to resolve the typing issue with `import near`:

1. **PEP 561 Type Stubs**:
   Create a separate package `near-stubs` that provides type definitions for the `near` module. This would allow for proper typing while keeping the runtime import intact.

   ```python
   # In near-stubs/near.pyi
   def log_utf8(message: str) -> None: ...
   def storage_read(key: bytes | str) -> bytes | None: ...
   # etc.
   ```

2. **TYPE_CHECKING Pattern Enhancement**:
   Enhance the current pattern to make it more maintainable:

   ```python
   # In near_sdk_py/api/__init__.py
   from typing import TYPE_CHECKING

   if TYPE_CHECKING:
       from near_sdk_py.api.stubs import near
   else:
       import near

   # Re-export everything from near
   from near import *
   ```

3. **Runtime Import Aliasing**:
   Use import hooks or other mechanisms to alias imports at runtime, allowing `import near_sdk_py.api as near` to work as `import near` at runtime.

### near-pytest Implementation Concept

The pytest plugin could work by injecting a simulated NEAR context:

```python
# near_pytest/plugin.py
import pytest
from .context import NearContext

@pytest.fixture
def near_context():
    """Provides a simulated NEAR contract context."""
    context = NearContext()
    yield context
    context.reset()

# Usage in tests
def test_transfer(near_context):
    # Set up context
    near_context.set_predecessor("alice.near")
    near_context.set_attached_deposit(1000000)

    # Call contract method
    result = my_contract.transfer("bob.near", 100)

    # Assertions
    assert near_context.storage_has("my_key")
    assert result == "expected result"
```

## Development Roadmap

### Phase 1: Core Infrastructure

1. **nearc Compiler**

   - Extract compilation logic from existing tool
   - Design and implement CLI interface
   - Implement dependency management
   - WASM generation and optimization
   - API for tooling integration

2. **near-sdk-py Low-Level API**
   - Define API structure and module organization
   - Implement core host function bindings
   - Add type annotations and documentation
   - Testing and validation

### Phase 2: Developer Experience

1. **near-sdk-py High-Level API**

   - Implement contract structure and decorators
   - Develop storage abstractions
   - Add cross-contract communication
   - Implement security utilities and event logging

2. **near-pytest**
   - Create pytest plugin structure
   - Implement runtime simulation
   - Add test fixtures and helpers
   - Develop assertion helpers and reporting

### Phase 3: Testing Infrastructure

1. **near-sandbox**

   - Implement binary management
   - Create sandbox configuration
   - Add process control
   - Develop integration with other tools

2. **near-workspaces-py**
   - Design workspace management
   - Implement contract interaction
   - Add test assertions
   - Support multi-contract testing

### Phase 4: Advanced Features

1. **Token Standards & Templates**

   - Implement token standards
   - Create project templates
   - Develop documentation generation

2. **near-py-vm (Stretch Goal)**
   - Design runtime simulation
   - Implement host function stubs
   - Add development integration features
