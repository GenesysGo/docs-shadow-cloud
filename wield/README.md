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

Running a Wield Node will allow you to trustlessly participate in the testnets which powers the Hammer interface for shdwDrive located [here](https://dagger-hammer.shdwdrive.com/). We encourage node operators to review our blog articles for full context on the role of Wield Nodes and the purpose of D.A.G.G.E.R. Hammer.

Wield Node operators will be handling thousands of live user test transactions and trustlessly executing all modules within D.A.G.G.E.R. that are requires to erasure code and store files uploaded to the shdwDrive test interface.

## Node Requirements:

**Operating system requirements are Ubuntu 22.04 LTS kernel 5.15.0. Other Linux x86 distributions may work but are not \_officially**\_\*\* supported at this time.\*\*

**Minimum hardware requirements\* to operate a D.A.G.G.E.R. Wield Node for Testnet Phase 1 are as follows:**

* **16 CPU threads**
* **32 GB RAM**
* **250 GB SSD for data storage and high speed i/o operations**
* **100mbps up/down network connection is the bare minimum**

## Option 1 - Guided install + startup script


{% hint style="info" %}
It is highly recommended to, at the very least, review the manual install steps below prior to determining how you would like to proceed. If you are comfortable with basic Linux commands and would like to be involved in the configuration of your node, follow the steps outlined in Option 2 - Manual Install below.
{% endhint %}

If you'd like a more guided experience, we have created a special installer script that will:

* Check your system's hardware to ensure it meets the minimum hardware specified
* Apply some kernel tunes to allow for improved traffic flow over tcp/udp
* Download `wield` and `shdw-keygen` binaries
* Generate a keypair file
* Generate a config file and startup script based on your machine's specs
* Generate and configure a system service
* Start up the wield service

To do this, all you have to do is run the following command from your terminal:

```sh
wget -O wield-installer.sh https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/wield-installer.sh && chmod +x wield-installer.sh && ./wield-installer.sh
```

If you have any issues with the above script, please give Option 2 a shot, below.

## Option 2 - Manual Install

### 1. Node Requirements - 16 CPU threads, 32 GB of RAM, 250 GB of SSD storage, 100 Mbps up/down network. Operating system requirements are Ubuntu 22.04 LTS kernel 5.15.0. Other Linux x86 distributions may work but are not officially supported at this time.

### 2. Operating system configuration.

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

### 3. Initial Node configuration:

If you have not already done so, it is recommended to create a dedicated user to run the application. In this case, we create the `dagger` user with `sudo adduser dagger` (create a password of your choosing) and then add the `dagger` user to the `sudo` usergroup with `sudo usermod -aG sudo dagger`. Switch to the `dagger` user with `sudo su - dagger`. All remaining tasks will be ran as the `dagger` user.

Download the Wield binary to the `dagger` user directory:

```sh
wget -O ~/wield https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/wield-latest
```

Make the Wield binary executable:

```sh
sudo chmod +x ~/wield
```

Download the Shdw-Keygen utility to the `dagger` user directory:

```sh
wget -O ~/shdw-keygen https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/shdw-keygen-latest
```

Make the Shdw-Keygen utility executable:

```sh
sudo chmod +x shdw-keygen
```

Create a new unique keypair ID using the Shdw-Keygen utility, write down your unique seed phrase and back up the id.json file to another location:

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

Create the Wield startup script with `nano start_wield.sh` and paste the below contents into the file. NOTE: These parameters are based on a 16 thread processor. It is recommended to set `--processor-threads` and `--global-threads` equal to the total thread count of your machine, and to leave `--comms-threads` set to `2`. For tuning parameters on different hardware please consult the output of `wield --help`:

```bash
#!/bin/bash
PATH=/home/dagger
exec wield \
--processor-threads 16 \
--global-threads 16 \
--comms-threads 2 \
--log-level info \
--history-db-path /mnt/dag/historydb \
--config-toml config.toml \
```

Make the script executable with `sudo chmod +x start_wield.sh`

Create a location to store the `historydb` on a disk with at least 200GB of available space (preparing and mounting disks is beyond the scope of this document based on the many variables possible with different hardware configurations, but Google or ChatGPT should be able to get you sorted). This location must match what is specified in the `start_wield.sh` startup script `--history-db-path` flag. In our case, a spare disk is mounted to `/mnt/dag` and the `historydb` directory is created there:

```sh
sudo mkdir -p /mnt/dag/historydb
```

Change owner of `historydb` location to `dagger` user:

```sh
sudo chown -R dagger:dagger /mnt/dag/*
```

You will also need a `snapshots` directory at the same location as your `wield` binary. This should be your home directory if you've followed along thus far. You can created by doing the following:

```sh
mkdir ~/snapshots
```

Create a system service for `wield` with `sudo nano /etc/systemd/system/wield.service` and paste the below contents into the file:

```sh
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

```sh
sudo systemctl enable --now wield.service
```

Verify proper operation by tailing the log with `tail -f config.log`. It may take a moment for Wield to begin writing to the log file, as various startup tasks run in the background prior to the software being initialized. You can verify proper node operation by checking your log file for bundles finalizng `tail -f config.log | grep "finalized"`.

To ensure log files do not become too large, it is recommended to add a `logrotate` entry for `wield` with `sudo nano /etc/logrotate.d/wield.conf` and paste the below contents into the file:

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

Restart the `logrotate` service with `sudo systemctl restart logrotate` and then verify that the `wield.conf` configuration is working correctly with `sudo logrotate -d /etc/logrotate.d/wield.conf` to check for any errors.

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

### **4. Node maintenance**

If you need to perform maintenance on your node during normal network operations, it is imperative that you wait five (5) D.A.G.G.E.R. Epochs before attempting to rejoin the network, otherwise your node will be excluded. You can monitor D.A.G.G.E.R. progress here: [https://dagger-hammer.shdwdrive.com/explorer](https://dagger-hammer.shdwdrive.com/explorer)

To stop your node, use `sudo systemctl stop wield`. At this point, you may proceed with any necessary maintenance, including upgrading `wield` to the latest version by simply downloading the current binaries:

```
wget -O ~/wield https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/wield-latest
```

Note that while it is not strictly necessary, it is recommended to upgrade to the latest `shdw-keygen` utility as well:

```
wget -O ~/shdw-keygen https://shdw-drive.genesysgo.net/4xdLyZZJzL883AbiZvgyWKf2q55gcZiMgMkDNQMnyFJC/shdw-keygen-latest
```

If all that is required is a `wield` upgrade, you can then re-start your node after 5 epochs with `sudo systemctl start wield`.

If a full cluster restart is required, it is recommended to follow any instructions given in the appropriate Discord channels.

\*Disclaimer: By operating a Wield Node on GenesysGo's D.A.G.G.E.R. Testnet Phase 1, you acknowledge that you do so voluntarily and at your own risk. GenesysGo provides the Testnet software "as is" without any warranties, and accepts no responsibility for any direct, indirect, incidental, or consequential damages you may incur. You are responsible for the security of your own system and data. GenesysGo shall not be liable for any losses or damages resulting from your participation in the Testnet. By using the software, you agree to indemnify GenesysGo against any claims or disputes related to your node operation. This agreement is binding upon download and operation of a Wield Node. GenesysGo may alter or discontinue the Testnet without notice. This includes, but is not limited to, the hardware requirements to operate a D.A.G.G.E.R. Wield Node in Testnet Phase 1. If you disagree with these terms, do not participate in the Testnet.

### Where to go from here?

Now that you've got your node up and running, check out [monitoring-stack.md](monitoring-stack.md "mention") for how to set up a monitoring stack to view and monitor your node's performance.

### Frequently Asked Questions (FAQ) for D.A.G.G.E.R. Testnet and Wield Nodes

## **Q: Where can I find the roadmap for D.A.G.G.E.R.?**

A: The roadmap can be found on the official blog, which outlines the plans for upcoming testnet phases.

## **Q: How many testnet phases will there be, and when is the estimated Mainnet launch?**

A: The number of testnet phases and the Mainnet launch date are still to be determined. The team is focused on ensuring stability and performance before setting any deadlines.

## **Q: Can I set up a Wield Node if I have never used Linux before?**

A: Yes, with a willingness to learn and some research, such as consulting YouTube tutorials or using resources like ChatGPT for Linux guidance, you can set up a node. The community and the engineering team are also available to help with questions.

## **Q: Where can I rent a server to run a Wield Node?**

A: Most cloud providers offer suitable instances for running a Wield Node. Digital Ocean is a solid standard for comparison, but always ensure the provider meets the system requirements for running a node.

## **Q: What hardware do I need to set up a Wield Node?**

A: The recommended system requirements are an AWS EC2 t2.2xlarge instance or equivalent with 16 vCPU, 32GB RAM, running Ubuntu 22.04 LTS with kernel version 5.15.0 or newer. Alternatively, you can use parts from an old gaming machine or any setup that meets these minimum requirements.

## **Q: Can I use a virtual machine to run a Wield Node?**

A: Yes, Wield Nodes have been successfully tested on virtual machines like VirtualBox or VMware, as well as on various cloud platforms. Ensure that the VM meets the recommended system requirements for optimal performance.

## **Q: Is it safe to upgrade from Ubuntu 20.04 to 22.04?**

A: Yes, many users have successfully performed an in-place upgrade from Ubuntu 20.04 to 22.04 without issues.

## **Q: What should I do if I encounter issues with the Wield service or during the setup process?**

A: First, ensure your `config.toml` is set up correctly and that you've given execution permissions to `start_wield.sh` with `chmod +x`. Check the service status with `sudo journalctl -u wield` for more details. If `wield.service` shows as failed, try adjusting the settings in `start_wield.sh` or use the `--help` command for guidance. Share any errors or issues in the support channel for further assistance.

## **Q: How can I troubleshoot errors or issues with my Wield Node?**

A: For troubleshooting, check the system logs, review the node's configuration files for errors, and ensure that all dependencies are installed correctly. You can also seek help from the Discord community support channels by sharing specific error messages or logs.

## **Q: How do I check the status and log files for my Wield Node?**

A: Log files can be checked using command-line tools like `cat`, `less`, `tail`, or `grep`. The specific log file location may vary based on your node configuration. Use `journalctl` to check systemd service logs, for example, `sudo journalctl -u wield.service`. You can check the status of your node by running the command `sudo systemctl status wield`. This will give you an overview of your node's current operational status. You can monitor the finalization process by using the command `tail -f config.log | grep "finalized"`. This will filter the log entries to show you only the lines that mention "finalized," allowing you to track the finalization process in real-time.

## **Q: Where can I find the log file on my server?**

A: The log file is usually located in `/home/dagger/config.log`.

## **Q: How do I find my node ID?**

A: Run `./shdw-keygen pubkey id.json` in the `/home/dagger` directory, assuming you have the `shdw-keygen` program downloaded.

## **Q: How do I check my node's version and upgrade if necessary?**

A: You can check your node's version with `./wield --version`. To upgrade, you can use the interactive installer script, which now includes an option to upgrade.

## **Q: How do I update to the latest version of the software?**

A: You can update your node by stopping the service, downloading the latest binary, and then restarting the service. Detailed commands are provided by users in the chat.

## **Q: What is log rotation and how do I set it up for my node?**

A: Log rotation is a system for managing log files so they don't consume too much disk space. You can set up log rotation using the `logrotate` utility on Linux, configuring it to rotate your node's log files based on size or time.

## **Q: Can I run a Wield Node on a system with less than the recommended specs for testing purposes?**

A: You can try running a node on a system with lower specs, but it may not perform optimally and may crash, especially if the RAM and CPU power do not meet the minimum requirements. The team may learn from your experience, but there's no guarantee it will work smoothly.

## **Q: How can I check system usage on my server?**

A: On an Ubuntu server, you can check system usage by opening the Ubuntu System Monitor or by using command-line tools such as `htop`, `top`, or `vmstat`. These tools provide information on CPU, memory, and disk usage.

## **Q: What is an epoch on the D.A.G.G.E.R. network?**

A: An epoch on the D.A.G.G.E.R. network is a time period that can be as short as 5-10 minutes currently, depending on network activity.

## **Q: How long is an epoch and where can I monitor the progress?**

A: An epoch is now 64 bundles in length.

## **Q: Can I close the terminal window after starting a node on a VPS?**

A: Yes, you can close the terminal window if you are using a service manager like `systemd` to run your node. The service will continue to run in the background. Use `systemctl status <service_name>` to check the status of your node.

## **Q: What should I do if I encounter a 'Resource temporarily unavailable' error when starting my node?**

A: This error usually means that a required resource, such as a database file, is locked because it is in use by another process. Make sure that no other instance of the node is running. You may need to stop the running service before starting it manually.

## **Q: What should I do if I receive an error about an invalid epoch in my logs?**

A: This could indicate that either the peer or your node is very behind. Make sure your node is up-to-date and has restarted correctly. You may have to wait several epochs before attempting to start your node again.

## **Q: What should I do if I encounter handshake errors or max handshake duration exceeded messages?**

A: Ensure you have the correct trusted peer values in your `config.toml` and that you have waited the full 5 epochs after an update before restarting your node.

## **Q: Is there a way to configure my node to use more memory or CPU resources?**

A: The node software will automatically use the resources it needs up to the limits of what is available on your system. If you notice low resource usage, it could be due to network idle times or inefficiencies in the node software that may be addressed in future updates.

## **Q: What happens if I accidentally try to start a second instance of my node?**

A: If you try to start a second instance while one is already running, you may encounter errors related to locked resources or port conflicts. Ensure that only one instance is running at any given time.

## **Q: Can I deploy a Wield Node on a server that's already hosting another node?**

A: It's possible, but ensure that the server has enough resources to handle both nodes without affecting performance. Monitor resource usage closely.

## **Q: How can I monitor the progress of epochs on the network?**

A: The progress of epochs can typically be monitored through the node's logs or through any network-monitoring tools provided by the D.A.G.G.E.R. team.

## **Q: What should I do if I notice the network has slowed down or is not progressing?**

A: Network slowdowns can occur for various reasons. Stay updated with official announcements from the D.A.G.G.E.R. team for information on network status and any actions you may need to take.

## **Q: What if I encounter a GLIBC not found error when setting up my node or generating a keypair?**

A: This error usually indicates that you're not on the correct version of Ubuntu or the Linux kernel. Ensure you are using Ubuntu 22.04 with kernel 5.15.0 or newer. You may need to perform system updates (`sudo apt update` and `sudo apt upgrade`) and possibly reboot your machine.

## **Q: How can I ensure my node stays live after closing the terminal?**

A: To ensure your node remains active after closing the terminal, run it as a background service using `systemd` or `screen`. This way, the process isn't tied to your terminal session.

## **Q: How can I increase the limits of my node's resource usage?**

A: Limits on resource usage are generally defined by the system's capabilities and the node's software. There may be configuration options that allow for tuning, but these should be used with caution to avoid instability.

## **Q: Where can I find announcements and updates for the testnet?**

A: Announcements and updates for the testnet are typically posted in the official channels provided by the D.A.G.G.E.R. team. Keep an eye on these channels for the latest information.

## **Q: How do I determine the profitability of running a node?**

A: Profitability will depend on various factors, including the rewards structure, operational costs, and network performance. Details on operator rewards, including allowances and penalties for downtime, will be provided by the D.A.G.G.E.R. team as the testnet progresses.

## **Q: What should I do if I encounter unfamiliar terms or need further assistance?**

A: Don't hesitate to ask for clarification or assistance in the Discord support channel. The community and the core engineering team are there to help you through the process.

## **Q: How do I share my server log file?**

A: You can download your log file from your server using the `scp` command or a similar method, and then upload it to a sharing service.
