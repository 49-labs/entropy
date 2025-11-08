# Program Directory

The `program` directory contains the on-chain Solana program implementation for the Entropy protocol.

## Overview

The Entropy program implements a provably-fair random number generation protocol using a commit-reveal scheme with slot hash sampling. It's built using the Steel framework for Solana program development.

## Instructions

### `open.rs`
Creates a new variable account (Note: currently commented out in the main processor).
- Initializes a variable with commit, provider, and timing parameters
- Sets up the commit-reveal cycle
- Configures automation settings

### `close.rs`
Closes a variable account and returns rent to the authority.
- Validates authority ownership
- Transfers lamports back to the authority
- Cleans up the account

### `next.rs`
Advances a variable to its next random value in the sequence.
- Validates the variable has been revealed
- Uses the previous seed as the new commit
- Resets slot hash and value for the next cycle
- Updates the end slot for the next reveal

### `reveal.rs`
Reveals the seed to generate the final random value.
- Validates the slot hash has been sampled
- Verifies the seed matches the commit using keccak hash
- Computes the final value by hashing seed with slot hash
- Increments the sample counter

### `sample.rs`
Samples the slot hash at the specified end slot.
- Validates timing (must be after end_at slot)
- Records the slot hash from the blockchain
- Enables the reveal phase

## Architecture

### Entry Point
The `lib.rs` file contains:
- Main `process_instruction` function that routes to instruction handlers
- Program entrypoint declaration
- Instruction parsing and validation

### Security Model
The protocol ensures fairness through:
1. **Commit phase**: Provider commits to future values without knowing slot hashes
2. **Wait period**: Time delay prevents manipulation of slot selection
3. **Sample phase**: Records unpredictable slot hash from the blockchain
4. **Reveal phase**: Provider reveals seed, which is verified against commit
5. **Finalization**: Random value computed from seed + slot hash

### State Transitions
```
Open → Sample (after end_at) → Reveal → Next → Sample → ...
```

## Building and Testing

The program uses Steel framework commands:
```bash
# Build the program
steel build

# Run tests
steel test
```

## Deployment

The program is deployed on Solana at address: `3jSkUuYBoJzQPMEzTvkDFXCZUBksPamrVhrnHR9igu2X`