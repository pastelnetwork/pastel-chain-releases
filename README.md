# Pastel Chain Releases

---

The repository hosts Pastel Chain releases.

It also contains hassle-free guides and scripts for getting started and managing Pastel Chain clients.

# ❗ The latest release is Matisse 2.2.1 ❗️

> Pastel Daemon version: **v2.2.1-unk**<br/>
> pasteld Hash (`sha256sum pasteld`) **b352f3075a8b1ecbf7694d66fe82a654fc5ea8a2577b901f9ccb320c27e736bd**<br/><br/>
> Supernode version: **v2.2.0**<br/>
> Supernode service Hash (`sha256sum supernode-linux-amd64`) **9d12b76c8f9ecb04c8557e3447cf2e4f01d50c312f32ec469e6f57213092ee9b**<br/><br/>
> Hermes version: **v2.2.0**<br/>
> Hermes service Hash (`sha256sum hermes-linux-amd64`) **6bd97e7786e64047f6bf6b8a41176950ff36dd29d649c85fe674f17c5a538eec**

## Release Files

* All required release files can be found [here](https://download.pastel.network/#latest-release/mainnet)
* [pastelup](https://github.com/pastelnetwork/pastelup/releases)
* [paslel-core](https://github.com/pastelnetwork/pastel/releases)
* [goNode](https://github.com/pastelnetwork/gonode/releases)
* [dd-service](https://github.com/pastelnetwork/dd-service/releases)
* [rq-service](https://github.com/pastelnetwork/rqservice/releases)

## Requirements

The following requirements are required to run the node natively.

### Hardware 

The following table shows the recommended hardware requirements for the running node on the mainnet.

| Full Node | WalletNode | SuperNode | 
| ----------- | ----------- | ----------- |
| Memory: 8Gb | Memory: 16Gb | Memory: 16Gb+ | 
| Storage: 50Gb | Storage: 1Tb | Storage: 1Tb+ | 
| Network: 5Gbps+	| Network: 5Gbps+ | Network: 5Gbps+ |

### OS / Software 
* Ubuntu 22.04 Required for running a SuperNode (Note: Full Node and WalletNode clients compatible on MacOS, Windows, and Linux)

_NOTE: Install guides for various client types can be found here - https://github.com/pastelnetwork/pastelup_

_NOTE: Upgrade guides for various client types can be found below._

## 0. FullNode Update Instructions for halted clients during Vermeer Upgrade

```shell
  # navigate to where pasteld binary is located:
  ./pastel-cli stop
  mv pasteld pasteld-2.0
  mv pastel-cli pastel-cli-2.0
  wget https://download.pastel.network/latest-release/mainnet/pasteld/pastel-linux-amd64.zip
  unzip pastel-linux-amd64.zip
  sudo chmod +x pasteld
  sudo chmod +x pastel-cli
  mv ~/.pastel/blocks ~/.pastel/blocks-2.0
  mv ~/.pastel/chainstate ~/.pastel/chainstate-2.0
  ./pasteld -rescan -reindex -txindex=1
```


## 1. Update WalletNode

1.a Download `pastelup` to Wallet host
   * https://download.pastel.network/#latest-release/pastelup/
1.b Update existing `pastelup`
  * `./pastelup-linux-amd64 update pastelup`
2. Stop pastel software
  ```shell
  ./pastelup-darwin-amd64 stop walletnode
  ```
2. Run the update command
  ```shell
  ./pastelup-darwin-amd64 update walletnode
  ```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
    * rq-server
    * walletnode
3. Start all services
  ```shell
  ./pastelup-darwin-amd64 start walletnode
  ```
 
## 2. Update SuperNode (HOT/HOT)

> NOTE: SuperNode Update assumes that:
>   * `REMOTE` host has:
>     * pastel node (`pasteld` and `pastel-cli`) running as `masternode`,
>     * wallet with collateral transaction for SN(s)
>     * `masternode.conf` for SN(s)
>     * All other services: supernode, rq-service, dd-service, hermes
>   * OPTIONAL: `LOCAL` host - the user's computer used for performing upgrade

### 2.1 Update existing client to latest Pastel Chain version FROM LOCAL host

1.a Download `pastelup` to `LOCAL` host (we assume that node doesn't have pastel application installed)
   * https://download.pastel.network/#latest-release/pastelup/
1.b Update existing `pastelup`
  * `./pastelup-linux-amd64 update pastelup`
2. Stop pastel software on `REMOTE` host by running the following command from `LOCAL` node
  ```shell
  ./pastelup-darwin-amd64 stop supernode remote --ssh-ip <IP address of REMOTE node> --ssh-user <ssh user of REMOTE node> --ssh-key <path to the key of REMOTE node user>
  ```
3. Update `REMOTE` node by running the following command from `LOCAL` node:
  ```shell
  ./pastelup-darwin-amd64 update supernode remote --ssh-ip <IP address of REMOTE node> --ssh-user <ssh user of REMOTE node> --ssh-key <path to the key of REMOTE node user>
  ```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
    * rq-server
    * hermes
    * supernode
    * dd-service
4. Start all services by running the following command from `LOCAL` node:
  1. Start Supernode on `HOT` node:
    ```shell
    ./pastelup-darwin-amd64 start supernode remote --ssh-ip <IP address of HOT node> --ssh-user <ssh user of HOT node> --ssh-key <path to the key of HOT node user>
    ```
### 2.2 Update existing client to latest Pastel Chain version FROM REMOTE host

1.a Download `pastelup` to `REMOTE` nodes
   * https://download.pastel.network/#latest-release/pastelup/
1.b Update existing `pastelup`
  * `./pastelup-linux-amd64 update pastelup`
2. Stop pastel software on `REMOTE` host by running the following command from `REMOTE` node itself:
  ```shell
  ./pastelup-darwin-amd64 stop supernode
  ```
3. Update `REMOTE` node by running the following command from `REMOTE` node itself:
  ```shell
  ./pastelup-darwin-amd64 update supernode
  ```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
    * rq-server
    * hermes
    * supernode
    * dd-service
4. Start all services by running the following command from `REMOTE` node itself:
  ```shell
  ./pastelup-darwin-amd64 start supernode
  ```

## 3. Update SuperNode (COLD/HOT)

> NOTE: SuperNode Update assumes that SuperNode is established via a COLD/HOT mode set-up where:
>   * `COLD` node has:
>     * pastel node (`pasteld` and `pastel-cli`) set as simple node (not as `masternode`),
>     * wallet with collateral transaction for SN(s) that ran on `HOT` node(s) 
>     * `masternode.conf` for SN(s) that ran on `HOT` node(s)
>   * `HOT` node has:
>     * pastel node (`pasteld` and `pastel-cli`) running as `masternode`
>     * All other services: supernode, rq-service, dd-service, hermes
>   * OPTIONAL: `LOCAL` host - the user's computer used for performing upgrade, this CAN be the `COLD` node  

### 3.1 Update existing client to latest Pastel Chain version FROM LOCAL Node

1.a Download `pastelup` to `LOCAL` node (we assume that node doesn't have pastel application installed)
   * https://download.pastel.network/#latest-release/pastelup/
1.b Update existing `pastelup`
  * `./pastelup-linux-amd64 update pastelup`
2. Stop pastel software on `COLD` node by running the following command from `LOCAL` node
  ```shell
  ./pastelup-darwin-amd64 stop node remote --ssh-ip <IP address of LOCAL node> --ssh-user <ssh user of LOCAL node> --ssh-key <path to the key of LOCAL node user>
  ```
3. Update `COLD` node by running the following command from `LOCAL` node
  ```shell
  ./pastelup-darwin-amd64 update node remote --ssh-ip <IP address of HOT node> --ssh-user <ssh user of HOT node> --ssh-key <path to the key of HOT node user>
  ```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
4. Stop pastel software on `HOT` host by running the following command from `LOCAL` node
  ```shell
  ./pastelup-darwin-amd64 stop supernode remote --ssh-ip <IP address of HOT node> --ssh-user <ssh user of HOT node> --ssh-key <path to the key of HOT node user>
  ```
3. Update `HOT` node by running the following command from `LOCAL` node:
  ```shell
  ./pastelup-darwin-amd64 update supernode remote --ssh-ip <IP address of HOT node> --ssh-user <ssh user of HOT node> --ssh-key <path to the key of HOT node user>
  ```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
    * rq-server
    * hermes
    * supernode
    * dd-service
4. Start all services by running the following command from `LOCAL` node:
  1. Start Supernode on `HOT` node:
    ```shell
    ./pastelup-darwin-amd64 start supernode remote --ssh-ip <IP address of HOT node> --ssh-user <ssh user of HOT node> --ssh-key <path to the key of HOT node user>
    ```
  2. _NOTE: In MOST cases you don't need to start pastel application on `COLD` node after upgrade!!!_

### 3.2 Update existing client to latest Pastel Chain version FROM COLD Node

1.a Download `pastelup` to `COLD` node
   * https://download.pastel.network/#latest-release/pastelup/
1.b Update existing `pastelup`
  * `./pastelup-linux-amd64 update pastelup`
2. Stop pastel software on `COLD` node by running the following command from `COLD` node itself:
  ```shell
  ./pastelup-darwin-amd64 stop node
  ```
3. Update `COLD` itself node by running the following command from `COLD` node itself:
  ```shell
  ./pastelup-darwin-amd64 update node
  ```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
4. Stop pastel software on `HOT` host by running the following command from `COLD` node
  ```shell
  ./pastelup-darwin-amd64 stop supernode remote --ssh-ip <IP address of HOT node> --ssh-user <ssh user of HOT node> --ssh-key <path to the key of HOT node user>
  ```
5. Update `HOT` node by running the following command from `COLD` node:
  ```shell
  ./pastelup-darwin-amd64 update supernode remote --ssh-ip <IP address of HOT node> --ssh-user <ssh user of HOT node> --ssh-key <path to the key of HOT node user>
  ```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
    * rq-server
    * hermes
    * supernode
    * dd-service
6. Start all services by running the following command from `COLD` node:
  1. Start Supernode on `HOT` node:
    ```shell
    ./pastelup-darwin-amd64 start supernode remote --ssh-ip <IP address of HOT node> --ssh-user <ssh user of HOT node> --ssh-key <path to the key of HOT node user>
    ```
  2. _NOTE: In MOST cases you don't need to start pastel application on `COLD` node after upgrade!!!_

### 3.3 Update existing client to latest Pastel Chain version FROM COLD and HOT Node

1.a Download `pastelup` to `COLD` and `HOT` nodes
   * https://download.pastel.network/#latest-release/pastelup/
1.b Update existing `pastelup`
  * `./pastelup-linux-amd64 update pastelup`
2. Stop pastel software on `COLD` node by running the following command from `COLD` node itself:
  ```shell
  ./pastelup-darwin-amd64 stop node
  ```
3. Update `COLD` node by running the following command from `COLD` node itself:
```shell
./pastelup-darwin-amd64 update node
```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
2. Stop pastel software on `HOT` host by running the following command from `HOT` node itself:
  ```shell
  ./pastelup-darwin-amd64 stop supernode
  ```
3. Update `HOT` node by running the following command from `HOT` node itself:
```shell
./pastelup-darwin-amd64 update supernode
```
  * The above command will update the following applications:
    * pasteld
    * pastel-cli
    * rq-server
    * hermes
    * supernode
    * dd-service
4. Start all services by running the following command:
  1. Start Supernode on `HOT` node:
    ```shell
    ./pastelup-darwin-amd64 start supernode
    ```
  2. _NOTE: In MOST cases you don't need to start pastel application on `COLD` node after upgrade!!!_
