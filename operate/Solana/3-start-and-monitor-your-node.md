# 3 - Start and Monitor Your Node

### **Does it work? Only one way to find out.**

**UNLEASH THE KRAKEN**

```bash
# Start systuner
sudo systemctl enable --now systuner.service
sudo systemctl status systuner.service
# Start the Solana RPC
sudo systemctl enable --now sol.service
sudo systemctl status sol.service
```

**AGAIN, RUN ALL OF THESE COMMANDS AS THE sol USER ACCOUNT!!**

If all is well, the Solana RPC service status shows active. There won't be much log output here, though, because logs are redirected to your logfile. Get to know this next command because you're going to use it a lot:

```bash
# This is the live streaming view of your logs
sudo tail -f ~/log/solana-validator.log
```

If you run this command right after starting your RPC service, you'll watch your RPC come to life. So what's going on here?

![](../.gitbook/assets/gip2hy.gif)

When an RPC or validator starts up, it downloads snapshots from other RPCs and validators. This is how it seeds the accounts and ledgers database. As it completes downloading the snapshots, it then unpacks the snapshot file and places the accounts and ledger data into their correct destination.

Once it feels like it's downloaded enough snapshots, it will then try to synchronize with the network in more of a live streaming method. Think about it this way - Solana moves really, really fast. 3 blocks are created every second. There's no way to just download the current state of the blockchain because it moves too fast.

So when your RPC server gets past the point downloading and unpacking snapshots, it will then try to aggressively "catch up" to the blockchain by synchronizing with other nodes in real time. This is known as the catch up phase, _**and this is where the bulk of your pain as an RPC operator will come in.**_

Validator nodes have no issue catching up. They can be "back on the tip" of the network in a matter of minutes. RPCs will take hours to catch up.&#x20;

**So how do you know when you are in the catchup phase?**

This is your second command to commit to memory.

```
solana catchup --our-localhost
```

If you are still in the unpacking phase (which takes awhile - don't get too antsy), your output will be `cannot connect to localhost` or something along those lines. If you are in catchup phase your output will show you some interesting information.

`4269 slots behind (our node is gaining 0.8 slots/second. ETA slot 420694206)`

The first thing to take in is how many slots behind you are. If you are more than 10,000 slots behind, your node will probably never catchup and it would be advisable to just restart the services.

The second thing to do is just sit there and watch it for a few minutes. If you are trending in the right direction, albeit slowly, you MIGHT be ok. I say might because the metaphorical winds can shift and your node could quickly start falling behind, too.&#x20;

The real kicker here is your node probably won't have any issues getting to the tip the first time you start it up. After it has been running for a couple months, though, we see nodes take longer and longer and longer to catch up until ultimately, they just don't. **What's the fix?**

**Go scorched earth on your data and recreate it all.**

```bash
sudo systemctl stop sol.service
sudo rm -r ~/log
sudo rm -r /extmt/ledger
sudo rm -r /mt/accounts
sudo mkdir ~/log
sudo mkdir -p /extmt/ledger/validator-ledger
sudo mkdir -p /mt/accounts/solana-accounts
sudo chown -R sol:sol /mt/*
sudo chown -R sol:sol /extmt/*
sudo chown sol:sol ~/log
sudo systemctl restart systuner.service
sudo systemctl start sol.service
```

One last thing I'll add here with monitoring your "slot distance" (aka, how far away from the tip you are). You can monitor this externally via your RPC's API, but only after unpacking is completed. Just replace the IP address with the IP of your RPC, or use localhost if done directly on the node.

`curl http://1.2.3.4:8899 -k -X POST -H "Content-Type: application/json" -d ' {"jsonrpc":"2.0","id":1, "method":"getHealth"} '`

Another thing you can try is try to preseed your incremental snapshot. GenesysGo uploads a snapshot every 15 minutes that you can download from the below URL. Simply navigate to the folder where your snapshots are stored (default is the validator-ledger folder) and download it.

```bash
cd /extmt/ledger/validator-ledger
wget https://shdw-drive.genesysgo.net/snapshots/latest-incremental
```

### Updating to a New Version of Solana

Every couple of weeks or so, Solana rolls out a new patched version of the Solana codebase for validators and RPCs to upgrade to. It's strongly recommended to get this done as quick as possible, because otherwise your node will be forked off of the blockchain.

Luckily, its easy to pull off and should only result in a couple hours of downtime (thanks to the aforementioned catchup period).

```bash
sudo systemctl stop sol.service
# solana-install init VERSION-GOES-HERE
# Example
solana-install init 1.13.2
sudo systemctl restart systuner.service
sudo systemctl start sol.service
```

Note: Every time you stop and restart the sol.service, you also need to restart systuner.service. Restart systuner before starting sol up again.

### Add a TIG Stack Monitoring Dashboard (the easy way)

It's nice to know what your node is up to and how healthy it is, and that's why Genesys Go has created a handy Docker container with a full TIG stack so that you can monitor your server's resources in near real time.&#x20;

![](<../.gitbook/assets/image (1) (1) (2).png>)

Ooo pretty colors. You want that for you, right? Well you can have it because you're awesome and you made it this far.&#x20;

**First we need to install Docker**

```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
sudo apt install docker-ce -y
sudo systemctl status docker
```

Next, rig it to where you don't have to type sudo for every docker command

```bash
sudo usermod -aG docker ${USER}
# logout and log back in
logout
su - sol
```

Now we need to install Docker-Compose.

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

Almost done! Now we need to clone the repository that contains the docker compose file and the configurations

```bash
cd $HOME
git clone https://github.com/Shadowy-Super-Coder-DAO/Shadow-RPC-Operator.git
cd ~/Shadow-RPC-Operator/shadow_monitoring
docker volume create grafana-storage
```

Lastly, open up your firewall and start the container

```bash
sudo ufw allow 3000/tcp
docker-compose up -d
```

You can now open your web browser and navigate to your RPC's Grafana endpoint listening on TPC 3000(ie `http://1.2.3.4:3000`), and login with creds `admin/admin`

At this point, if your node is caught up on the tip, you should be good to proceed on to registering your RPC node with the Shadow network so that you can begin receiving traffic!

``
