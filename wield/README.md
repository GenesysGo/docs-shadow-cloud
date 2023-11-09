---
description: >-
  GenesysGo's Directed Acyclic Gossiping Graph Enabling Replication
  (D.A.G.G.E.R.) is currently running Testnet Phase 1 now with the ability for
  trustless operators to join in and help test!
---

# Run a Wield Node

## 1. Node Requirements - Specific hardware requirements are yet to be determined. Operating system requirements are Ubuntu 22.04 LTS kernel 5.15.0. Other Linux x86 distributions may work but are not supported at this time.

## 2. Operating system configuration.

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

## 3. Node configuration.

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
trusted_node = "147.75.47.13:2030"
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
