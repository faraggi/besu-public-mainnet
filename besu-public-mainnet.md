# Mainnet Ethereum Besu Node Guide
![Ethereum Network](https://i.imgur.com/qbiJ4Co.png)
![Ethereum Network](https://i.imgur.com/tsca5UT.png)

#### Contents

- [Current State of the Ethereum Network](#current-state-of-the-ethereum-network)
- [Why would you run a Besu Node?](#why-would-you-run-a-besu-node)
- [Hardware & Software](#hardware--software)
- [How to run a Public Ethereum Besu Node](#how-to-run-a-public-ethereum-besu-node)
- [Getting Started & Contributing](#getting-started--contributing)


Ethereum is an open source, public, permissionless peer-to-peer network of **distributed nodes** that send and receive transactions, validate those transactions and remotely execute smart contract's scripts.

These nodes are what make the Ethereum platform function and allow us to rely on a decentralized network of information.      
Running a public node means participating in the Ethereum network by having your node do the work necessary to validate transactions and new blocks on the blockchain.
You can run any mainnet Ethereum compatible [client software](https://ethereum.stackexchange.com/questions/269/what-exactly-is-an-ethereum-client-and-what-clients-are-there) on your node, including [Besu](https://besu.hyperledger.org/en/stable/HowTo/Get-Started/Run-Docker-Image/#default-node-for-mainnet).

## Current State of the Ethereum Network

![Europe's node locations](https://i.imgur.com/fkS0sqJ.jpg)

At the time of this article, there are [approximately 6000](https://www.ethernodes.org/) - [7500 running Ethereum nodes](https://etherscan.io/nodetracker) in the world. Of those, about 5000 are synced to mainnet- the [rest are syncing and somewhat behind](https://www.ethernodes.org/sync) the [lastest best block](https://ethereum.stackexchange.com/questions/26127/what-does-best-block-mean) in the blockchain.

## Why would you run a Besu Node?

The benefits of running an Ethereum node are two-fold; on one hand, the ecosystem as a whole will benefit from having higher distribution and decentralization of the infrastructure itself; and on the other hand, you might have personal reasons to run a node.

- Decentralizing the ecosystem:
    - Enabling more copies of the blockchain history for everyone to access
    - Lower latency/increased diversity (in terms of which nodes people are able to consult)

- Personal:
    - Blockchain Data analysis on local machines instead of doing remote calls
    - Sending your own transactions from a Node you trust, and propagating it from there
    - Developing purposes: If you're building your own dapp or service, you will certainly benefit from having your own node to run the dapp on while developing


Reasons why you would choose to run a **_Besu_** node are also two fold;

- Network client Variety:
    - Helping in the decentralization of the node-client share:
    The Ethereum network has lower risks of malfunctioning if the network is running on multiple clients, reducing risks of single points of failure. Historically, we've already seen the advantages of client diversity in the [2016 Devcon 2 Shanghai attack](https://ro.ecu.edu.au/cgi/viewcontent.cgi?article=1219&context=ism ), where the network continued to function since the attack only affected a single client.
    - The Ethereum network currently has ~75% running on the same client software. Ideally, this should be more distributed and spread out among client software.

- Personal, Besu specific features:
    -  [Tracing](https://besu.hyperledger.org/en/stable/Reference/Trace-Types/#trace) methods are now available in the [Besu API](https://besu.hyperledger.org/en/stable/Reference/API-Methods/#trace_replayblocktransactions). This allows you request a transaction's ordered list of calls to other smart contracts, to replay a history of a transaction, and every detail included in it such as Opcode calls to the EVM.
    - [Plugins](https://besu.hyperledger.org/en/stable/Concepts/Plugins/) are a reflection of Besu's internal modular design. They allow you to extend Besu's functionality and accessing additional data not available in the APIs or push data out to another third party application.
    - [Monitoring on Besu](https://besu.hyperledger.org/en/stable/Concepts/Monitoring/) is fairly straight forward thanks to the Grafana/Prometheus integration. Use it to obtain metrics on your node performance and/or get logging data.


## Hardware & Software

The hardware requirements to be able to run a public mainnet node on ethereum will depend on what client you choose. Besu will need at least 4GB of RAM, and 750GB of hard disk space to run performantly. Storage will need to be SSDs or NVMe in order to keep up with the speeds needed to sync the chain.

However, there are options that need more and possibilities to reduce the requirements. This some approximate data on syncing to mainnet on different setups:

Node Type | HD Space | RAM | Sync time | flags & comments
--- | --- | --- | --- | ---
Full node | 3TB | 8GB | 90 days | `besu`
Pruned node | 1TB | 8GB | 30 days | `besu --pruning-enabled`
Fast-sync | 1TB | 8GB | 3 days | `besu --sync-mode=FAST`
ARM device | 1TB | 2GB | 5 days | java `Xmx2G` option to reduce memory consumption


Software-wise, Besu runs on Linux, MacOS and Windows. It requires Java 11+ and some [minor dependencies if building from source and testing](https://wiki.hyperledger.org/display/BESU/Building+from+source).


![Hosting possibilities](https://i.imgur.com/Q2HnacS.png)

Running a public Besu node is possible on several different hosting possibilities. The most common option for individual participants is running on a on-prem device such as a laptop, desktop computer or server. However, cloud options like AWS or Azure are fully compatible with Besu.
The main advantages of running on a cloud instance is the ease of setup, and the possibility of running and stopping instances remotely.

## Running a Public Ethereum Besu Node
![Besu Terminal](https://i.imgur.com/GHToZxr.png)

After [installing Besu on your node](https://besu.hyperledger.org/en/stable/HowTo/Get-Started/Install-Binaries/), go through this checklist to make sure you know what you are setting up.

- Which network do you want to participate in?  
- Do you want to be able to execute JSON-RPC calls on it?  
- Where do you want to store your blockchain data?  
- Do you want to monitor your node?  
- Do you want a full node, archival node or pruned node?


The Besu options you need will depend on what usage you're giving your node.
You can configure these options via command line arguments, environment variables or configuration files. The following options show you how to run besu with command line argument flags. Alternatively, read here for [config file setup](https://besu.hyperledger.org/en/stable/Reference/CLI/CLI-Syntax/#config-file) & [environment variables](https://besu.hyperledger.org/en/stable/Reference/CLI/CLI-Syntax/#besu-environment-variables).

#### Which network do you want to participate in?  

If your goal is running a full, archival node, then no flags are necessary at all:
```
$ besu
```
Mainnet isn't the only public ethereum network available. If you were planning on syncing another public network, [choose a network from this list](https://besu.hyperledger.org/en/stable/Reference/CLI/CLI-Syntax/#network) and use the `--network` flag.

    --network=mainnet  # The main Ethereum network
    --network=ropsten  # A PoW network similar to the current main Ethereum network
    --network=rinkeby  # A PoA network using Clique
    --network=goerli  # A PoA network using Clique
    --network=dev  # A PoW network with a low difficulty to enable local CPU mining
    --network=classic  # The main Ethereum Classic network
    --network=mordor  # A PoW network
    --network=kotti  # A PoA network using Clique

#### Do you want to be able to execute JSON-RPC calls on it?  

For a full node, with an http port exposed, adding the `--rpc-http-enabled` flag will enable the RPC JsonRpcHttpService and allow you to send and request data from the Json-RPC endpoint. 
The default endpoint for this service is `127.0.0.1`. Use `--rpc-http-host=<new-host-ip>` to edit the endpoint.
```
$ besu --rpc-http-enabled
```

#### Where do you want to store your blockchain data?  

By default, Besu stores all blockchain data in the directory you installed Besu in, or ``/opt/besu/database` if using docker.

If you'd like to change this, use the [`data-path` flag](https://besu.hyperledger.org/en/stable/Reference/CLI/CLI-Syntax/#data-path).

```
--data-path=/home/username/data/path/
```


For a fast-synced Besu node, add the `--sync-mode=FAST` flag. This will modify the way the blockchain's past data syncs on your storage device. 
For a comprehensive video of the differences between fast-sync and full sync, [watch this video](https://www.youtube.com/watch?v=4GMIdPt_uTw).
```
$ besu --sync-mode=FAST
```
[![Fast sync v/s Full Sync](https://i.imgur.com/4JwYbnJ.png)](https://www.youtube.com/watch?v=4GMIdPt_uTw)


Fast-synced node, with pruning, with an http port exposed:
```
$ besu --rpc-http-enabled --sync-mode=FAST --pruning-enabled=true
```

If you want to mine ethereum using your node, then the `miner-enabled` flag is required. However, this type of usage is quite case-specific and needs tweaking and making sure the miner is working properly. Review the following links and command line arguments for further options:
[Besu Command Line Options](https://besu.hyperledger.org/en/stable/Reference/CLI/CLI-Syntax/#miner-enabled)

## Getting Started & Contributing

These are some links in order to start your node running:

- [Besu Documentation](Getting Started & Contributing)
- For installation, download [Besu Binaries](https://pegasys.tech/solutions/hyperledger-besu/#downloads) or follow our install guides; [Linux/MacOS](https://kauri.io/installing-besu-binary-on-linux/e00df6efcb644e07ab44df169d9375e9/a) or [Windows](https://kauri.io/using-besu-the-java-ethereum-client-with-windows/8ed3a9dac7e044f9b6b45491fcef0df5/a)
- [Besu wiki](https://wiki.hyperledger.org/display/BESU/Hyperledger+Besu)
- [Besu Rocket Chat](https://chat.hyperledger.org/channel/besu)


Want to Contribute to the Besu Project?

Besu is fully open source and only gets better with the help of the open source community and their contributions. If you want to help build Besu, [check out our Contributing Guide](https://wiki.hyperledger.org/display/BESU/How+to+Contribute).  
Or head over to our [issues](https://github.com/hyperledger/besu/issues) or our [github](https://github.com/hyperledger/besu).