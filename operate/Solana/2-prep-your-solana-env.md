# 2 - Prep Your Solana Env

You made it through that storage stuff, huh? Congrats - that was the hard part. Now we get to build out our Solana environment.

**Let's get started by creating all of the folders Solana needs to work: the accounts folder, the ledger folder, and the logs folder. Remember these are all on separate disks and that was the whole point of the storage setup.**

**REMEMBER THIS NEEDS TO BE DONE AS THE sol USER ACCOUNT!! (su - sol)**

<pre class="language-bash"><code class="lang-bash"># create the folders
sudo mkdir -p /extmt/ledger/validator-ledger
sudo mkdir -p /mt/accounts/solana-accounts
sudo mkdir ~/log
# Change ownership to the sol user
<strong>sudo chown -R sol:sol /mt/*
</strong>sudo chown -R sol:sol /extmt/*
sudo chown sol:sol ~/log
</code></pre>

Install and enable the firewall. Pretty straightforward, except we will open some ports that Solana needs to operate.

```bash
sudo snap install ufw
sudo ufw allow ssh
sudo ufw allow 8000:10000/udp
sudo ufw allow 8899/tcp
sudo ufw allow 8900/tcp
sudo ufw enable
# type y to accept the prompt
```

**Install the Solana CLI. NOTE**: It is strongly recommended that you join the Solana Discord and look in the #mb-announcements channel to find the suggested version that Solana is currently recommending to validators and RPCs. You can also find what version the majority of the cluster is operating on [at the bottom of this website](https://www.validators.app/cluster-stats?locale=en\&network=mainnet), though it may not be what Solana recommends if they have just suggested a new release.

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.13.3/install)"
```

After install completes, immediately follow that up with this command:

```bash
export PATH="/home/sol/.local/share/solana/install/active_release/bin:$PATH"
```

Now, our RPC node needs a couple keypairs to identify itself and it's participation on the Solana network. Let's generate those keypairs

```bash
# this generates what is known as the identity keypair - this is how your RPC identifes itself
solana-keygen new -o ~/validator-keypair.json
# type a secure password/string to generate the keypair
# Next, this sets your solana environment to default use the identity keypair just created for all commands
solana config set --keypair ~/validator-keypair.json
# this is an additional keypair primarily used by validators for signing votes. You still need it even though you wont vote
solana-keygen new -o ~/vote-account-keypair.json
# type a secure password/string to generate the keypair
```

**Now to the big show. This is where all the magic happens - the start-validator.sh script.** This is where we start our RPC server and define how it should operate.

```bash
sudo nano ~/start-validator.sh
```

Then we will paste in these contents.

```bash
#!/bin/bash
PATH=/home/sol/.local/share/solana/install/active_release/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
export RUST_BACKTRACE=1
export RUST_LOG=solana=info
exec solana-validator \
--identity ~/validator-keypair.json \
--entrypoint entrypoint.mainnet-beta.solana.com:8001 \
--entrypoint entrypoint2.mainnet-beta.solana.com:8001 \
--entrypoint entrypoint3.mainnet-beta.solana.com:8001 \
--entrypoint entrypoint4.mainnet-beta.solana.com:8001 \
--entrypoint entrypoint5.mainnet-beta.solana.com:8001 \
--rpc-port 8899 \
--dynamic-port-range 8002-8099 \
--no-port-check \
--gossip-port 8001 \
--no-untrusted-rpc \
--no-voting \
--private-rpc \
--rpc-bind-address 0.0.0.0 \
--enable-cpi-and-log-storage \
--account-index program-id \
--enable-rpc-transaction-history \
--no-duplicate-instance-check \
--wal-recovery-mode skip_any_corrupted_record \
--vote-account ~/vote-account-keypair.json \
--log ~/log/solana-validator.log \
--accounts /mt/accounts/solana-accounts \
--ledger /extmt/ledger/validator-ledger \
--limit-ledger-size 400000000 \
--rpc-send-default-max-retries 3 \
--rpc-send-service-max-retries 3 \
--rpc-send-retry-ms 2000 \
--full-rpc-api \
--accounts-index-memory-limit-mb 350 \
--account-index-exclude-key kinXdEcpDQeHPEuQnqmUgtYykqKGVFq6CeVX5iAHJq6 \
--tpu-use-quic \
--known-validator PUmpKiNnSVAZ3w4KaFX6jKSjXUNHFShGkXbERo54xjb \
--known-validator Ninja1spj6n9t5hVYgF3PdnYz2PLnkt7rvaw3firmjs \
--known-validator CXPeim1wQMkcTvEHx9QdhgKREYYJD8bnaCCqPRwJ1to1 \
--known-validator A4hyMd3FyvUJSRafDUSwtLLaQcxRP4r1BRC9w2AJ1to2 \
--known-validator 23U4mgK9DMCxsv2StC4y2qAptP25Xv5b2cybKCeJ1to3 \
--known-validator Ei8VLKR3chZAhJzWwj8PopeuedpQiths2ovVCQ2BCvK7 \
--known-validator DiGifdKABxzru2KsjN3YkZZmWP9mVMYK8HWadjtPtJit \
```

**You need to take a moment to pause and read through these params. You don't have to recognize them all.**&#x20;

You want to pay attention to things that have paths (like the accounts path, the ledger path, the logs path) and make sure they are accurate. You also want to look into `limit-ledger-size` if you have smaller drives than 3.9TB. Also if you have less than 512GB RAM, you should remove the `accounts-index-memory-limit-mb` option. This also just scratches on the surface for the number of params that can be specified to run a node. You may want to look into these params and options further to optimize for your server!

Now make this file executable and change the ownership.

```bash
sudo chmod +x ~/start-validator.sh
sudo chown sol:sol start-validator.sh
```

Now we will turn our startup script into a service.

```bash
sudo nano /etc/systemd/system/sol.service
```

And paste in these contents (no need to edit anything here)

```bash
[Unit]
Description=Solana Validator
After=network.target
Wants=systuner.service
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=on-failure
RestartSec=1
LimitNOFILE=1000000
LogRateLimitIntervalSec=0
User=sol
Environment=PATH=/bin:/usr/bin:/home/sol/.local/share/solana/install/active_release/bin
Environment=SOLANA_METRICS_CONFIG=host=https://metrics.solana.com:8086,db=mainnet-beta,u=mainnet-beta_write,p=password
ExecStart=/home/sol/start-validator.sh

[Install]
WantedBy=multi-user.target
```

Type Ctrl+X, Y to save and quit the nano editor. Now we move on to create the systuner service, which is built in to Solana runtime.

```bash
sudo nano /etc/systemd/system/systuner.service
```

And paste in these contents (no need to edit anything here)

```bash
[Unit]
Description=Solana System Tuner
After=network.target
[Service]
Type=simple
Restart=on-failure
RestartSec=1
LogRateLimitIntervalSec=0
ExecStart=/home/sol/.local/share/solana/install/active_release/bin/solana-sys-tuner --user sol
[Install]
WantedBy=multi-user.target
```

Reload all of your services now that these two have been created.

```bash
sudo systemctl daemon-reload
```

**Now comes an important one.** Solana logs consume a lot of space. If you've been paying attention, you may have noticed that logs are going to be stored on our OS disk in \~/log. This will fill up the OS disk after a few days unless we delete old logs. So we have a logrotate daemon that will archive logs everyday, and then delete logs after they are a few days old.

```bash
sudo nano /etc/logrotate.d/solana
```

And copy in these contents (no need to edit)

```bash
/home/sol/log/solana-validator.log {
  su sol sol
  daily
  rotate 1
  missingok
  postrotate
    systemctl kill -s USR1 sol.service
  endscript
}
```

Now restart the logrotate daemon.&#x20;

```bash
sudo systemctl restart logrotate
```

**And the final setup task, edit the system config**.

```bash
sudo nano /etc/sysctl.conf
```

Paste in these contents at the bottom of the file

```bash
# set minimum, default, and maximum tcp buffer sizes (10k, 87.38k (linux default), 12M resp)
net.ipv4.tcp_rmem=10240 87380 12582912
net.ipv4.tcp_wmem=10240 87380 12582912
# Enable TCP westwood for kernels greater than or equal to 2.6.13
net.ipv4.tcp_congestion_control=westwood
net.ipv4.tcp_fastopen=3
net.ipv4.tcp_timestamps=0
net.ipv4.tcp_sack=1
net.ipv4.tcp_low_latency=1
# Enable fast recycling TIME-WAIT sockets
net.ipv4.tcp_tw_recycle = 1
# don't cache ssthresh from previous connection
net.ipv4.tcp_no_metrics_save = 1
net.ipv4.tcp_moderate_rcvbuf = 1

# kernel Tunes
kernel.timer_migration=0
kernel.hung_task_timeout_secs=30
# A suggested value for pid_max is 1024 * <# of cpu cores/threads in system>
kernel.pid_max=49152

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

# solana systuner
net.core.rmem_max=134217728
net.core.rmem_default=134217728
net.core.wmem_max=134217728
net.core.wmem_default=134217728
```

Type Ctrl+X, Y to save and you are done with setup. Congrats!
