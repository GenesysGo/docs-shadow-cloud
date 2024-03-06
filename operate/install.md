# Install

### Option 1 - Guided install + startup script

{% hint style="info" %}
Please make sure to review the [minimum hardware requirements](./#hardware-requirements) prior to installation.

It is highly recommended to, at the very least, review the manual install steps below prior to determining how you would like to proceed. If you are comfortable with basic Linux commands and would like to be involved in the configuration of your node, follow the steps outlined in Option 2 - Manual Install below.
{% endhint %}

If you'd like a more guided experience, we have created a special installer script that will:

* Check your system's hardware to ensure it meets the minimum hardware specified
* Apply some kernel tunes to allow for improved traffic flow over tcp/udp
* Download `shdw-node` and `shdw-keygen` binaries
* Generate a keypair file
* Generate a config file and startup script based on your machine's specs
* Generate and configure a system service
* Start up the shdw-node service

To do this, all you have to do is run the following command from your terminal:

```sh
wget -O shdwnode-installer.sh https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/shdwnode-installer.sh && chmod +x shdwnode-installer.sh && ./shdwnode-installer.sh
```

If you have any issues with the above script, please give Option 2 a shot, below.

### Option 2 - Manual Install

#### 1. Node Requirements - 16 CPU threads (8 cores), 32 GB of RAM, 2 TB of SSD storage, 100 Mbps up/down network. Operating system requirements are Ubuntu 22.04 LTS kernel 5.15.0. Other Linux x86 distributions may work but are not officially supported at this time.

#### 2. Operating system configuration.

Begin by ensuring your operating system is up to date:

```sh
sudo apt update && sudo apt upgrade -y
```

If necessary, reboot your system to ensure the kernel is up to date.

The following kernel tuning parameters are recommended to be applied by editing `/etc/sysctl.conf` and adding the below lines to the configuration file, then applying the new parameters with `sudo sysctl -p`. NOTE: Please review these parameters to ensure they make sense for your specific hardware configuration.:

```sh
# set default and maximum socket buffer sizes to 12MB
net.core.rmem_default=12582912
net.core.wmem_default=12582912
net.core.rmem_max=12582912
net.core.wmem_max=12582912

# make changes for ulimit
fs.nr_open = 5000000
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
kernel.pid_max=16384

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

It is recommended to increase the maximum open file descriptors (`ulimit`) beyond the maximum hard limit to `5000000` by editing `/etc/security/limits.conf` and adding the below lines to the bottom of the configuration file (log out and back in for changes to take effect):

```sh
*               soft    nofile          5000000
*               hard    nofile          5000000
```

#### 3. Initial Node configuration:

If you have not already done so, it is recommended to create a dedicated user to run the application. In this case, we create the `dagger` user with `sudo adduser dagger` (create a password of your choosing) and then add the `dagger` user to the `sudo` usergroup with `sudo usermod -aG sudo dagger`. Switch to the `dagger` user with `sudo su - dagger`. All remaining tasks will be ran as the `dagger` user.

Download the `shdw-node` binary to the `dagger` user directory:

```sh
wget -O ~/shdw-node https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/wield-latest
```

Make the `shdw-node` binary executable:

```sh
sudo chmod +x ~/shdw-node
```

Download the `shdw-keygen` utility to the `dagger` user directory:

```sh
wget -O ~/shdw-keygen https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/shdw-keygen-latest
```

Make the `shdw-keygen` utility executable:

```sh
sudo chmod +x shdw-keygen
```

Create a new unique keypair ID using the `shdw-keygen` utility, write down your unique seed phrase and back up the id.json file to another location:

```sh
./shdw-keygen new -o ~/id.json
```

{% hint style="danger" %}
**IMPORTANT: You must back up your seed phrase and id.json keypair file to a secure location other than your node! Your node performance and rewards are tied to this identity. If you need to reinstall your node, you can restore from this backup to maintain continuity of your node ID. If you lose your keypair we cannot help you recover it!**

**If you need to reinstall or update your node, DO NOT create a new keypair, simply retain or copy over your original `id.json` file.**
{% endhint %}

To display your pubkey, you can run:

```sh
./shdw-keygen pubkey ~/id.json
```

Create a config file with `nano config.toml` and paste the the below contents into the file:

```toml
trusted_nodes = ["184.154.98.116:2030", "184.154.98.117:2030", "184.154.98.118:2030", "184.154.98.119:2030", "184.154.98.120:2030"]
dagger = "JoinAndRetrieve"

[node_config]
socket = 2030
keypair_file = "id.json"

[storage]
peers_db = "dbs/peers.db"
```

Create the `shdw-node` startup script with `nano start_shdwnode.sh` and paste the below contents into the file. NOTE: These parameters are based on a 16 thread processor. It is recommended to set `--processor-threads` and `--global-threads` equal to the total thread count of your machine, and to leave `--comms-threads` set to `2`. For tuning parameters on different hardware please consult the output of `wield --help`:

```bash
#!/bin/bash
PATH=/home/dagger
exec shdw-node \
--processor-threads 16 \
--global-threads 16 \
--comms-threads 2 \
--log-level info \
--history-db-path /mnt/dag/historydb \
--config-toml config.toml \
```

Make the script executable with `sudo chmod +x start_shdwnode.sh`

Create a location to store the `historydb` on a disk with at least 200GB of available space (preparing and mounting disks is beyond the scope of this document based on the many variables possible with different hardware configurations, but Google or ChatGPT should be able to get you sorted). This location must match what is specified in the `start_shdwnode.sh` startup script `--history-db-path` flag. In our case, a spare disk is mounted to `/mnt/dag` and the `historydb` directory is created there:

```sh
sudo mkdir -p /mnt/dag/historydb
```

Change owner of `historydb` location to `dagger` user:

```sh
sudo chown -R dagger:dagger /mnt/dag/*
```

You will also need a `snapshots` directory at the same location as your `shdw-node` binary. This should be your home directory if you've followed along thus far. You can created by doing the following:

```sh
mkdir ~/snapshots
```

Create a system service for `shdw-node` with `sudo nano /etc/systemd/system/shdw-node.service` and paste the below contents into the file:

```sh
[Unit]
Description=shdwNode Service
After=network.target

[Service]
User=dagger
WorkingDirectory=/home/dagger
ExecStart=/home/dagger/start_shdwnode.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

Register the service and start the `shdw-node` process with:

```sh
sudo systemctl enable --now shdw-node.service
```

Verify proper operation by tailing the log with `tail -f config.log`. It may take a moment for Wield to begin writing to the log file, as various startup tasks run in the background prior to the software being initialized. You can verify proper node operation by checking your log file for bundles finalizng `tail -f config.log | grep "finalized"`.

To ensure log files do not become too large, it is recommended to add a `logrotate` entry for `shdw-node` with `sudo nano /etc/logrotate.d/shdw-node.conf` and paste the below contents into the file:

```/home/dagger/config.log
/home/dagger/config.log {
    su dagger dagger
    daily
    rotate 5
    size 10M
    missingok
    copytruncate
    delaycompress
    compress
}
```

Restart the `logrotate` service with `sudo systemctl restart logrotate` and then verify that the `shdw-node.conf` configuration is working correctly with `sudo logrotate -d /etc/logrotate.d/shdw-node.conf` to check for any errors.

You may also wish to ensure that your `syslog` log file is set up to rotate properly by running `sudo logrotate -d /etc/logrotate.d/rsyslog` and checking for and correcting any errors. As an example, the configuration we have had to use for `/etc/logrotate.d/rsyslog` is as follows:

```
/var/log/syslog
/var/log/mail.info
/var/log/mail.warn
/var/log/mail.err
/var/log/mail.log
/var/log/daemon.log
/var/log/kern.log
/var/log/auth.log
/var/log/user.log
/var/log/lpr.log
/var/log/cron.log
/var/log/debug
/var/log/messages
{
        su syslog syslog
	rotate 4
	weekly
	missingok
	notifempty
	compress
	delaycompress
	sharedscripts
	postrotate
		/usr/lib/rsyslog/rsyslog-rotate
	endscript
}
```

#### **4. Node maintenance**

To stop your node, use `sudo systemctl stop shdw-node`. At this point, you may proceed with any necessary maintenance, including upgrading `shdw-node` to the latest version by simply downloading the current binaries:

```
wget -O ~/shdw-node https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/wield-latest
```

Note that while it is not strictly necessary, it is recommended to upgrade to the latest `shdw-keygen` utility as well:

```
wget -O ~/shdw-keygen https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/shdw-keygen-latest
```

If all that is required is a `shdw-node` upgrade, you can then re-start your with `sudo systemctl start shdw-node`.

If a full cluster restart is required, it is recommended to follow any instructions given in the appropriate Discord channels.

#### What should I do after setting up my shdwNode?

Now that you've got your node up and running, review wallet [verification ](https://docs.shdwdrive.com/operate#discord-verification)and then review the [monitoring ](https://docs.shdwdrive.com/operate/monitoring-stack)guide for how to set up a monitoring stack to view and monitor your node's performance.

\*Disclaimer: By operating a Wield Node on GenesysGo's D.A.G.G.E.R. Testnet Phase 1, you acknowledge that you do so voluntarily and at your own risk. GenesysGo provides the Testnet software "as is" without any warranties, and accepts no responsibility for any direct, indirect, incidental, or consequential damages you may incur. You are responsible for the security of your own system and data. GenesysGo shall not be liable for any losses or damages resulting from your participation in the Testnet. By using the software, you agree to indemnify GenesysGo against any claims or disputes related to your node operation. This agreement is binding upon download and operation of a Wield Node. GenesysGo may alter or discontinue the Testnet without notice. This includes, but is not limited to, the hardware requirements to operate a D.A.G.G.E.R. Wield Node in Testnet Phase 1. If you disagree with these terms, do not participate in the Testnet.
