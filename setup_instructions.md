# Linera Protocol Setup Instructions for Windows

## Step 1: Install WSL (Windows Subsystem for Linux)
```bash
wsl --install
```

## Step 2: Install Rust in WSL
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
rustup target add wasm32-unknown-unknown
```

## Step 3: Install Protocol Buffers
```bash
curl -LO https://github.com/protocolbuffers/protobuf/releases/download/v21.11/protoc-21.11-linux-x86_64.zip
unzip protoc-21.11-linux-x86_64.zip -d $HOME/.local
export PATH="$PATH:$HOME/.local/bin"
```

## Step 4: Build Linera
```bash
cd /mnt/c/Users/atulk/Documents/GitHub/linera-protocol
cargo build --release
```

## Step 5: Run Local Test Network
```bash
# Add binaries to PATH
export PATH="$PWD/target/release:$PATH"

# Import helper function
source /dev/stdin <<<"$(linera net helper 2>/dev/null)"

# Start local network
linera_spawn linera net up --with-faucet --faucet-port 8080
```

## Quick Demo Commands
```bash
# Set environment variables
export LINERA_WALLET="$LINERA_TMP_DIR/wallet.json"
export LINERA_KEYSTORE="$LINERA_TMP_DIR/keystore.json"
export LINERA_STORAGE="rocksdb:$LINERA_TMP_DIR/client.db"

# Initialize wallet
linera wallet init --faucet http://localhost:8080

# Create chains and transfer tokens
linera wallet request-chain --faucet http://localhost:8080
linera transfer 10 --from CHAIN1 --to CHAIN2
```