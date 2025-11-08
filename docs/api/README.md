# API Directory

The `api` directory contains the core type definitions, constants, and SDK functions for the Entropy protocol.

## Structure

- **`consts.rs`**: Program-wide constants used throughout the Entropy protocol
- **`error.rs`**: Custom error types for better error handling and debugging
- **`instruction.rs`**: Instruction enum and data structures for all program instructions
- **`lib.rs`**: Main library entry point that re-exports all modules via the `prelude` module
- **`sdk.rs`**: Helper functions to build instructions for interacting with the Entropy program
- **`state/`**: State account definitions
  - **`mod.rs`**: Module exports
  - **`var.rs`**: The `Var` account structure that tracks random variables

## Key Components

### State Management
The `Var` account is the primary state account that tracks:
- Authority and provider information
- Commit/reveal scheme data (commit, seed, slot_hash, value)
- Sample count and automation settings
- Lifecycle management (end_at slot)

### SDK Functions
The SDK provides convenient functions to build instructions:
- `open()`: Create a new random variable
- `close()`: Close and reclaim rent from a variable
- `next()`: Move to the next random value in the sequence
- `reveal()`: Reveal the seed to finalize the random value
- `sample()`: Sample the slot hash for randomness

### Program ID
The program is deployed at: `3jSkUuYBoJzQPMEzTvkDFXCZUBksPamrVhrnHR9igu2X`

## Usage

Import the prelude module to access all API components:

```rust
use entropy_api::prelude::*;
```