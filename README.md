# Storage Node

0G System is composed of multiple components, each with its own functionalities. Detailed steps are provided as a guideline to deploy the storage node.

* [Prerequisite](storage-node.md#prerequisite)
* [Storage Node](storage-node.md#storage-node)
* [Storage Node CLI](storage-node.md#storage-node-cli)

### Prerequisite

0G Storage interact with on-chain contracts for blob root confirmation and PoRA mining.

For official deployed contract addresses, visit [this page](../docs/contract-addresses.md).

### Storage Node

#### Hardware Requirement

```
- Memory: 16 GB RAM
- CPU: 4 cores
- Disk: 500GB / 1T NVME SSD
- Bandwidth: 500 MBps for Download / Upload
```

#### Deployment Steps

1. Install dependencies

* For Linux

<pre><code><strong>sudo apt-get update
</strong>sudo apt-get install clang cmake build-essential
</code></pre>

* For Mac

```bash
brew install llvm cmake
```

2. Install rustup

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

3. Install Go

* For Linux

<pre class="language-bash"><code class="lang-bash"><strong># Download the Go installer
</strong>wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz

# Extract the archive
sudo rm -rf /usr/local/go &#x26;&#x26; sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz

# Add /usr/local/go/bin to the PATH environment variable by adding the following line to your ~/.profile.
export PATH=$PATH:/usr/local/go/bin
</code></pre>

* For Mac

```bash
brew install go
```

Or  download the Go installer from [https://go.dev/dl/](https://go.dev/dl/).\
Open the package file you downloaded and follow the prompts to install Go.

4. Then download the source code

```bash
git clone -b v0.3.3 https://github.com/0glabs/0g-storage-node.git
```

5. Build the source code

```bash
cd 0g-storage-node
git submodule update --init

# Build in release mode
cargo build --release
```

6. Update the `run/config.toml`

```toml
# enr address, must fill your instance's public ip to support peer discovery
network_enr_address

# peer nodes, we provided 3 nodes with last one being HK region, you can also modify to your own ips
network_boot_nodes = ["/ip4/54.219.26.22/udp/1234/p2p/16Uiu2HAmTVDGNhkHD98zDnJxQWu3i1FL1aFYeh9wiQTNu4pDCgps","/ip4/52.52.127.117/udp/1234/p2p/16Uiu2HAkzRjxK2gorngB1Xq84qDrT4hSVznYDHj6BkbaE4SGx9oS","/ip4/18.167.69.68/udp/1234/p2p/16Uiu2HAm2k6ua2mGgvZ8rTMV8GhpW71aVzkQWy7D37TTDuLCpgmX"]

# flow contract address
log_contract_address

# mine contract address
mine_contract_address

# layer one blockchain rpc endpoint
blockchain_rpc_endpoint

# block number to start the sync
log_sync_start_block_number

# your private key with 64 length
# do not include leading 0x
# do not omit leading 0
# must fill if you want to participate in the pora and get mining reward
miner_key

# The max number of chunk entries to store in db.
# Each entry is 256B, so the db size is roughly limited to
# `256 * db_max_num_chunks` Bytes.
# If this limit is reached, the node will update its `shard_position`
# and store only half data.
db_max_num_chunks
```

You can also update the fields in `scripts/update_config.sh` and execute it to modify the config file.

7. Run the storage service

Check the command line configuration with `zgs_node -h`

```shell
cd run

# consider using tmux in order to run in background
../target/release/zgs_node --config config-testnet.toml --miner-key <your_private_key> --blockchain-rpc-endpoint <blockchain_rpc> --db-max-num-chunks <max_chunk_num>
```

### You are all set !
