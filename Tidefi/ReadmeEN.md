&#x20;                                                       [<mark style="color:red;">**Website**</mark>](https://nodeist.net/) | [<mark style="color:blue;">**Discord**</mark>](https://discord.gg/ypx7mJ6Zzb) | [<mark style="color:green;">**Telegram**</mark>](https://t.me/noodeist)

&#x20;                                     [<mark style="color:purple;">**100$ Credit Free VPS for 2 Months(DigitalOcean)**</mark>](https://www.digitalocean.com/?refcode=410c988c8b3e&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)

![](https://user-images.githubusercontent.com/50621007/176684496-cee59c96-79be-4185-af80-c418ac4dbe63.png)



# Tidechain node setup for Lagoon Testnet

Official documents:
- Official guide: https://github.com/tidelabs/tidechain/blob/dev/docs/run-validator.md
- Telemetry: https://telemetry.tidefi.io/#list/Tidechain

## Recommended hardware requirements VDS
- CPU: 8 CPUs
- Memory: 32GB RAM
- Disk: 500 GB SSD Storage (to start)

## Required ports
If you are going to connect to the node on localhost, the port does not need to be opened.
For RPC and WebSockets the following should open: `9933/TCP, 9944/TCP`

### Node Installation
You can setup your tidechain full node in minutes using the automated script below.
```
wget -O Nodeisttidefi.sh https://raw.githubusercontent.com/Nodeist/Kurulumlar/main/Tidefi/Nodeisttidefi.sh && chmod +x Nodeisttidefi.sh && ./Nodeisttidefi.sh
```

### Post-installation steps

## Check your node sync
If the output is "false", your node is in sync.
```
curl -s -X POST http://localhost:9933 -H "Content-Type: application/json" --data '{"id":1, "jsonrpc":"2.0", "method": "system_health", "params":[]}' | jq .result.isSyncing
```

## Check connected peers
```
curl -s -X POST http://localhost:9933 -H "Content-Type: application/json" --data '{"id":1, "jsonrpc":"2.0", "method": "system_health", "params":[]}' | jq .result.peers
```

## Update the node
Please run the following command to upgrade your node to the new binaries:
```
cd $HOME && sudo rm -rf tidechain
APP_VERSION=$(curl -s https://api.github.com/repos/tidelabs/tidechain/releases/latest | jq -r ".tag_name")
wget -O tidechain https://github.com/tidelabs/tidechain/releases/download/${APP_VERSION}/tidechain
sudo chmod +x tidechain
sudo mv tidechain /usr/local/bin/
sudo systemctl restart tidechaind
```

## Reset node
If you are running a node before and want to switch to a new snapshot, please perform these steps and then follow the guide again:
```
tidechain-node purge-chain --chain lagoon --base-path /chain-data -y
sudo systemctl restart tidechaind
```

## Useful commands
Check node version
```
tidechain --version
```

Check node status
```
sudo service tidechaind status
```

Check the node logs
```
journalctl -u tidechaind -f -o cat
```

You should see something similar in the logs:
```
Jun 28 16:40:32 paeq-01 systemd[1]: Started tidechain Node.
Jun 28 16:40:32 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:32 tidechain Node
Jun 28 16:40:32 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:32 ✌️  version 3.0.0-polkadot-v0.9.16-6f72704-x86_64-linux-gnu
Jun 28 16:40:32 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:32 ❤️  by tidechain network <https://github.com/tidechainnetwork>, 2021-2022
Jun 28 16:40:32 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:32 📋 Chain specification: lagoon-network
Jun 28 16:40:32 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:32 🏷  Node name: ro_full_node_0
Jun 28 16:40:32 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:32 👤 Role: FULL
Jun 28 16:40:32 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:32 💾 Database: RocksDb at ./chain-data/chains/lagoon-substrate-testnet/db/full
Jun 28 16:40:32 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:32 ⛓  Native runtime: tidechain-node-3 (tidechain-node-1.tx1.au1)
Jun 28 16:40:36 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:36 🔨 Initializing Genesis block/state (state: 0x72d3…181f, header-hash: 0xf496…909f)
Jun 28 16:40:36 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:36 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
Jun 28 16:40:40 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:40 Using default protocol ID "sup" because none is configured in the chain specs
Jun 28 16:40:40 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:40 🏷  Local node identity is: 12D3KooWD4hyKVKe3v99KJadKVVUEXKEw1HNp5d6EXpzyVFDwQtq
Jun 28 16:40:40 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:40 📦 Highest known block at #0
Jun 28 16:40:40 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:40 〽️ Prometheus exporter started at 127.0.0.1:9615
Jun 28 16:40:40 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:40 Listening for new connections on 127.0.0.1:9944.
Jun 28 16:40:40 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:40 🔍 Discovered new external address for our node: /ip4/138.201.118.10/tcp/1033/ws/p2p/12D3KooWD4hyKVKe3v99KJadKVVUEXKEw1HNp5d6EXpzyVFDwQtq
Jun 28 16:40:45 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:45 ⚙️  Syncing, target=#1180121 (13 peers), best: #3585 (0x40f3…2548), finalized #3584 (0x292b…1fee), ⬇ 386.2kiB/s ⬆ 40.1kiB/s
Jun 28 16:40:50 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:50 ⚙️  Syncing 677.6 bps, target=#1180122 (13 peers), best: #6973 (0x6d89…9f39), finalized #6656 (0xaff8…65d9), ⬇ 192.5kiB/s ⬆ 7.8kiB/s
Jun 28 16:40:55 paeq-01 tidechain-node[27357]: 2022-06-28 16:40:55 ⚙️  Syncing 494.6 bps, target=#1180123 (13 peers), best: #9446 (0xe7e2…c2d9), finalized #9216 (0x1951…dc01), ⬇ 188.7kiB/s ⬆ 5.8kiB/s
```

To delete the node
```
sudo systemctl stop tidechaind
sudo systemctl disable tidechaind
sudo rm -rf /etc/systemd/system/tidechaind*
sudo rm -rf /usr/local/bin/tidechain*
sudo rm -rf /chain-data
```


* To create a validator, see the relevant document:
https://medium.com/tidefi/running-a-validator-on-the-tidefi-lagoon-testnet-f2928731f32c
