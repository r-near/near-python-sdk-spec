# NEAR Python Tooling: Developer Spec

## TL;DR

- Building Python tools for NEAR smart contracts
- Makes complex WASM compilation simple
- Write smart contracts in Python with proper tooling
- Full testing framework (unit + integration)
- Goal: Make Python devs feel at home on NEAR

## Components

### `nearc` - Compiler

- **What**: CLI tool to convert Python → WASM
- **Why**: Simplifies complex compilation process
- **Features**:
  - Simple: `nearc contract.py`
  - Handles dependencies automatically
  - Optimization flags (size/speed)
  - Can be integrated with other tools
- **Output**: Single WASM file ready for deployment

### `near-sdk-py` - SDK

- **What**: Python library for smart contract development
- **Why**: Abstracts low-level details, adds Pythonic interfaces

#### Low-Level API

- Direct bindings to NEAR host functions
- Typed interfaces to blockchain functions
- Examples:
  ```python
  from near_sdk_py.api import storage_read, log_utf8, value_return
  ```

#### High-Level API

- Contract structure with class-based approach
- Method decorators: `@view`, `@call`, `@init`
- Storage abstractions (collections, maps, vectors)
- Cross-contract calls made easy
- Security utilities
- Examples:

  ```python
  @view
  def get_balance(account_id):
      return balances.get(account_id, 0)

  @call
  def transfer(receiver_id, amount):
      # Implementation
  ```

### Testing Framework

#### `near-pytest`

- Unit testing for contract methods
- Runs locally without deployment
- Simulates blockchain environment
- pytest plugin: `pytest -xvs tests/`
- Example:
  ```python
  def test_transfer(near_contract):
      contract.call("transfer", {"receiver_id": "bob", "amount": 100})
      assert contract.view("get_balance", {"account_id": "bob"}) == 100
  ```

#### `near-sandbox`

- Local NEAR node for testing
- Auto-downloads binary
- Example:
  ```python
  sandbox = NearSandbox()
  sandbox.start()
  # Test against local network
  sandbox.stop()
  ```

#### `near-workspaces-py`

- Integration testing across contracts
- Deploys to sandbox or testnet
- Example:
  ```python
  async def test_contracts():
      worker = await Sandbox.init()
      contract = await worker.dev_deploy("contract.wasm")
      result = await contract.call("increment")
      assert result == 1
  ```

### Developer Tools

#### `near-py-vm` (Stretch Goal)

- Runs contracts directly in Python
- No compilation/deployment needed
- Fast iteration feedback loop
- Example:
  ```python
  vm = NearVM()
  contract = vm.load("contract.py")
  result = vm.call(contract, "increment")
  ```

## What Each Tool Does

| Tool                   | Input         | Output             | Primary Usage        |
| ---------------------- | ------------- | ------------------ | -------------------- |
| **nearc**              | `contract.py` | `contract.wasm`    | Compilation          |
| **near-sdk-py**        | N/A           | Importable library | Contract development |
| **near-pytest**        | Test scripts  | Test results       | Unit testing         |
| **near-sandbox**       | N/A           | Running node       | Local blockchain     |
| **near-workspaces-py** | Test scripts  | Test results       | Integration testing  |
| **near-py-vm**         | `contract.py` | Execution results  | Rapid development    |

## Workflow

1. **Develop**: Write contract using `near-sdk-py`
2. **Test**: Unit test with `near-pytest`
3. **Compile**: Generate WASM with `nearc`
4. **Integration Test**: Test with `near-workspaces-py` against `near-sandbox`
5. **Deploy**: Use existing `near-cli-rs` to deploy to testnet/mainnet

## For Python Developers

- Use familiar Python patterns and idioms
- Test with pytest (the tool you already know)
- Type hints and IDE integration work properly
- Standard Python project structure

## Development Checklist

### Phase 1 (Compiler & Core SDK)

- [ ] Extract compiler from existing code
- [ ] Create proper Python bindings for NEAR host functions
- [ ] Implement decorators for contract methods
- [ ] Resolve `import near` typing issues
- [ ] Basic unit testing support

### Phase 2 (DX & High-Level SDK)

- [ ] Storage abstractions (collections)
- [ ] Complete testing framework
- [ ] Contract templates
- [ ] Cross-contract call abstractions
- [ ] Token standard implementations

### Phase 3 (Advanced Features)

- [ ] Integration testing framework
- [ ] Simulation environment
- [ ] Advanced security patterns
- [ ] More project templates
- [ ] CI/CD integration

## Command Line Interface

```bash
# Compile a contract
nearc contract.py

# Advanced compilation options
nearc contract.py --optimize size --output build/

# Run unit tests
python -m pytest

# Start a local sandbox
near-sandbox start

# Integration tests
near-workspaces-py test integration_tests/
```

## Examples

```python
# Simple counter contract
from near_sdk_py.contract import view, call, init
from near_sdk_py.collections import Map

counter = Map[str, int]("c")

@init
def initialize():
    counter["total"] = 0

@call
def increment():
    current = counter["total"]
    counter["total"] = current + 1
    return current + 1

@view
def get_count():
    return counter["total"]
```
