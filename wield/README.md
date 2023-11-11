---
description: >-
  GenesysGo's Directed Acyclic Gossiping Graph Enabling Replication
  (D.A.G.G.E.R.) is currently running Testnet Phase 1 now with the ability for
  trustless operators to join in and help test!
---

# Run a D.A.G.G.E.R. Wield Node

## Testnet Information:

[Testnet 1: Release Overview](https://www.shdwdrive.com/blog/dagger-testnet-release)

[D.A.G.G.E.R. Hammer: The magic behind a keypress](https://www.shdwdrive.com/blog/dagger-hammer-the-magic-behind-a-keypress)

[Testnet Phase 1 Learnings](https://www.shdwdrive.com/blog/dagger-testnet-phase-1-learnings)

[D.A.G.G.E.R. Hammer](https://dagger-hammer.shdwdrive.com/)

Running a Wield Node will allow you to trustlessly participate in the D.A.G.G.E.R. phase 1 testnet which powers the D.A.G.G.E.R. Hammer interface located [here](https://dagger-hammer.shdwdrive.com/). We encourage node operators to review our blog articles for full context on the role of Wield Nodes and the purpose of D.A.G.G.E.R. Hammer.&#x20;

Wield Node operators will be handling thousands of live user test transactions and trustlessly executing all modules within D.A.G.G.E.R. that are requires to erasure code and store files uploaded to the Hammer test interface.&#x20;

## 1. Node Requirements:

**Operating system requirements are Ubuntu 22.04 LTS kernel 5.15.0. Other Linux x86 distributions may work but are not **_**officially**_** supported at this time.**

**Minimum hardware requirements\* to operate a D.A.G.G.E.R. Wield Node for Testnet Phase 1 are as follows:**

* **8 CPU cores**
* **32 GB RAM**
* **250 GB SSD for data storage and high speed i/o operations**
* **100mbps up/down network connection is the bare minimum**

## 2. Operating system configuration:

It is recommended to set the maximum open file descriptors (`ulimit`) to the maximum hard limit of `1048576` by editing `/etc/security/limits.conf` and adding the below lines to the bottom of the configuration file (log out and back in for changes to take effect):

```
*               soft    nofile          1048576
*               hard    nofile          1048576
```

The following kernel tuning parameters are recommended to be applied by editing `/etc/sysctl.conf` and adding the below lines to the configuration file, then applying the new parameters with `sudo sysctl -p`. NOTE: Please review these parameters to ensure they make sense for your specific hardware configuration.:

```
# set default and maximum socket buffer sizes to 12MB
net.core.rmem_default=12582912
net.core.wmem_default=12582912
net.core.rmem_max=12582912
net.core.wmem_max=12582912

# set minimum, default, and maximum tcp buffer sizes (10k, 87.38k (linux default), 12M resp)
net.ipv4.tcp_rmem=10240 87380 12582912
net.ipv4.tcp_wmem=10240 87380 12582912

# Enable TCP westwood for kernels greater than or equal to 2.6.13
net.ipv4.tcp_congestion_control=westwood
net.ipv4.tcp_fastopen=3
net.ipv4.tcp_timestamps=0
net.ipv4.tcp_sack=1
net.ipv4.tcp_low_latency=1
# don't cache ssthresh from previous connection
net.ipv4.tcp_no_metrics_save = 1
net.ipv4.tcp_moderate_rcvbuf = 1

# kernel Tunes
kernel.timer_migration=0
kernel.hung_task_timeout_secs=30
# A suggested value for pid_max is 1024 * <# of cpu cores/threads in system>
kernel.pid_max=65536

# vm.tuning
vm.swappiness=30
vm.max_map_count=1000000
vm.stat_interval=10
vm.dirty_ratio=40
vm.dirty_background_ratio=10
vm.min_free_kbytes = 3000000
vm.dirty_expire_centisecs=36000
vm.dirty_writeback_centisecs=3000
vm.dirtytime_expire_seconds=43200
```

## 3. Node configuration:

If you have not already done so, it is recommended to create a dedicated user to run the application. In this case, we create the `dagger` user with `sudo adduser dagger` (create a password of your choosing) and then add the `dagger` user to the `sudo` usergroup with `sudo usermod -aG sudo dagger`. Switch to the `dagger` user with `sudo su - dagger`. All remaining tasks will be ran as the `dagger` user.

Download the Wield binary to the `dagger` user directory:

```
wget -O wield https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/wield-0.1.0-x84_64-linux-gnu
```

Make the Wield binary executable:

```
sudo chmod +x wield
```

Download the Shdw-Keygen utility to the `dagger` user directory:

```
wget -O shdw-keygen https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/shdw-keygen-0.1.0-x84_64-linux-gnu
```

Make the Shdw-Keygen utility executable:

```
sudo chmod +x shdw-keygen
```

Create a new unique keypair ID using the Shdw-Keygen utility:

```
./shdw-keygen new -o ~/id.json
```

Create a config file with `nano config.toml` and paste the the below contents into the file:

```
trusted_node = "139.178.81.111:2030"
dagger = "JoinAndRetrieve"

[node_config]
socket = 2030
idle_timeout = 3600
keypair_file = "id.json"

[storage]
peers_db = "dbs/peers.db"
```

Create the Wield startup script with `nano start_wield.sh` and paste the below contents into the file. NOTE: These parameters are based on a 32c/64t processor. For tuning parameters on different hardware please consult the output of `wield --help`:

```
#!/bin/bash
PATH=/home/dagger
exec wield \
--processor-threads 36 \
--global-threads 12 \
--comms-threads 8 \
--log-level info \
--history-db-path /mnt/dag/historydb \
--config-toml config.toml \
```

Make the script executable with `sudo chmod +x start_wield.sh`

Create a location to store the `historydb` on a disk with at least 200GB of available space (preparing and mounting disks is beyond the scope of this document). This location must match what is specified in the `start_wield.sh` startup script `--history-db-path` flag. In our case, a spare disk is mounted to `/mnt/dag` and the `historydb` directory is created there:

```
sudo mkdir -p /mnt/dag/historydb
```

Change owner of `historydb` location to `dagger` user:

```
sudo chown -R dagger:dagger /mnt/dag/*
```

You will also need a `snapshots` directory at the same location as your `wield` binary. This should be your home directory if you've followed along thus far. You can created by doing the following:

```
mkdir ~/snapshots
```

Create a system service for `wield` with `sudo nano /etc/systemd/system/wield.service` and paste the below contents into the file:

```
[Unit]
Description=DAGGER Wield Service
After=network.target

[Service]
User=dagger
WorkingDirectory=/home/dagger
ExecStart=/home/dagger/start_wield.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

Register the service and start the `wield` process with:

```
sudo systemctl enable --now wield.service
```

Verify proper operation by tailing the log with `tail -f config.log`.



\*Disclaimer: By operating a Wield Node on GenesysGo's D.A.G.G.E.R. Testnet Phase 1, you acknowledge that you do so voluntarily and at your own risk. GenesysGo provides the Testnet software "as is" without any warranties, and accepts no responsibility for any direct, indirect, incidental, or consequential damages you may incur. You are responsible for the security of your own system and data. GenesysGo shall not be liable for any losses or damages resulting from your participation in the Testnet. By using the software, you agree to indemnify GenesysGo against any claims or disputes related to your node operation. This agreement is binding upon download and operation of a Wield Node. GenesysGo may alter or discontinue the Testnet without notice. This includes, but is not limited to, the hardware requirements to operate a D.A.G.G.E.R. Wield Node in Testnet Phase 1. If you disagree with these terms, do not participate in the Testnet.



### Frequently Asked Questions (FAQ) for D.A.G.G.E.R. Testnet and Wield Nodes

**Q: What are the system requirements for running a Wield Node?**
A: The recommended system requirements are an AWS EC2 t2.2xlarge instance or equivalent with 8 vCPU, 32GB RAM, running Ubuntu 22.04 LTS with kernel version 5.15.0 or newer.

**Q: What should I do if my Wield Node fails to start?**
A: If `wield.service` shows as failed, you can try adjusting the settings in `start_wield.sh` script or use the `--help` command for guidance. It may also be necessary to clear out snapshots and wait for 5 epochs before attempting to join again.

**Q: What is an epoch on the D.A.G.G.E.R. network?**
A: An epoch on the D.A.G.G.E.R. network is not correlated to Solana epochs. A DAGGER epoch can be as short as 5-10 minutes currently, depending on network activity.

**Q: What should I do if I encounter errors in the logs after starting my Wield Node?**
A: Share the logs in the support channel. Errors such as "Should not be genesis event" or timeout syncing with peers may require you to clear snapshots and wait for a few epochs. If errors persist, wait for the core engineering team to investigate.

**Q: Can I run a Wield Node on a virtualized environment?**
A: Yes, Wield Nodes have been tested on bare metal, VMs with 8 vCPU, and various cloud platforms. Performance may vary based on network traffic and usage increases.

**Q: What should I do if I get a GLIBC version error when generating a keypair?**
A: This error may occur if you are not running the correct version of the Linux kernel. Ensure you are using Ubuntu 22.04 with kernel 5.15.0 or newer. You may need to perform system updates (`sudo apt update` and `sudo apt upgrade`) and possibly reboot your machine.

**Q: Can I run a Wield Node on a lower-end machine just for testing?**
A: You may attempt to run a node on lower-end hardware, but be aware that if you do not meet the minimum requirements, particularly for RAM, your node may crash. It's also important to have sufficient CPU power to keep up with the network.

**Q: Where can I rent a server to run a Wield Node?**
A: Most cloud providers offer suitable images for running a Wield Node. Digital Ocean is a solid standard for comparison, but always ensure the provider meets the system requirements for running a node.

**Q: If I have no experience with Linux or running nodes, can I still set up a Wield Node?**
A: Yes, with a willingness to learn and some research, such as consulting YouTube tutorials or using resources like ChatGPT for Linux guidance, you can set up a node. The community and engineering team are also available to help with questions.

**Q: What should I do if I encounter unfamiliar terms or need further assistance?**
A: Don't hesitate to ask for clarification or assistance in the support channel. The community and the core engineering team are there to help you through the process.

Remember that the information provided in the FAQ is based on the latest knowledge as of the last update and may change as the D.A.G.G.E.R. network evolves or as new updates are released. Always refer to the official documentation and Discord support channels for the most current information.

