# CLI Directory

The `cli` directory contains a command-line interface for interacting with the Entropy protocol.

## Overview

The CLI provides a simple interface to interact with Entropy variables through environment variables and commands. It connects to Solana RPC and performs operations like opening variables, cranking (sampling and revealing), and viewing variable state.

## Environment Variables

- **`KEYPAIR`**: Path to your Solana keypair file (required)
- **`RPC`**: Solana RPC endpoint URL (required)
- **`COMMAND`**: The operation to perform (required)
- **`ADDRESS`**: Variable account address (required for some commands)
- **`ID`**: Variable ID number (required for `open` command)

## Commands

### `open`
Creates a new Entropy variable.
- Requires: `ID` environment variable
- Generates a PDA address for the variable
- Initializes with a hardcoded commit and provider
- Sets the end slot to 150 slots in the future

### `crank`
Performs the sample and reveal operations for a variable.
- Requires: `ADDRESS` environment variable
- Checks if the variable is ready (end_at + 4 buffer slots)
- Fetches the seed from the Entropy API
- Executes sample and reveal instructions

### `close`
Closes a variable account and recovers rent.
- Requires: `ADDRESS` environment variable
- Returns rent to the payer

### `var`
Displays the current state of a variable.
- Requires: `ADDRESS` environment variable
- Shows all variable fields including authority, provider, commit, seed, slot hash, value, and samples

## Usage Examples

```bash
# Open a new variable
KEYPAIR=~/.config/solana/id.json RPC=https://api.mainnet-beta.solana.com COMMAND=open ID=1 cargo run

# View variable state
KEYPAIR=~/.config/solana/id.json RPC=https://api.mainnet-beta.solana.com COMMAND=var ADDRESS=<var-address> cargo run

# Crank the variable (sample and reveal)
KEYPAIR=~/.config/solana/id.json RPC=https://api.mainnet-beta.solana.com COMMAND=crank ADDRESS=<var-address> cargo run

# Close the variable
KEYPAIR=~/.config/solana/id.json RPC=https://api.mainnet-beta.solana.com COMMAND=close ADDRESS=<var-address> cargo run
```

## Implementation Details

- Uses the Entropy provider at: `AKBXJ7jQ2DiqLQKzgPn791r1ZVNvLchTFH6kpesPAAWF`
- Fetches seeds from: `https://entropy-api.onrender.com/var/{address}/seed`
- Includes compute budget instructions for priority fees
- Provides helper functions for RPC operations and transaction submission