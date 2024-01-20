---
description: Frequently Asked Questions
---

# FAQ

## Testnet 2

### **Q: What are the requirements to run a node on the Testnet?**

A: For hardware, you will need a bare metal server or a VPS with at least 16 threads (8 cores), 32 GB RAM, and sufficient SSD storage (2 TB SSD or better recommended). Generally, expect to conduct many restarts, handle outages, bugs, report issues and share logs with the core team.

For the rest just follow this guide: [Step for operating a shdwNode](https://docs.shdwdrive.com/operate#to-operate-a-shdwnode-you-must)

### **Q: How do I associate my node ID with my wallet and get the shdwOperator role?**

A: The process for associating your node ID with your wallet is explained in the [verification guide](./#discord-verification).

Our system that checks your stake, creates and send the cNFT (shdwOperator NFT), and assigns the Discord role will take a few minutes to process. It is designed to be scalable and thus orderly queues database, Solana RPC, and Discord API calls with retries for robustness. Please be patient and give the cranks ample time to process your verification (several minutes).

### **Q: How are the 150 incentivized operators selected?**

A: The ongoing selection process for incentivized operators will be based on various criteria, including node performance and uptime. Upon initialization of the testnet, a global restart will be requested and the first 150 that join will be on a first come first serve. After the initial global restart, testnet 2 will be governed by entirely programmatic on-chain smart contracts monitoring performance, proper version, error states and uptime. You will want to achieve as much consistent uptime as possible.

### Q: How do I know if I am connected to the network properly and achieving uptime?

A: Look for "finalized" in your logs - if it's there then it's working. You can monitor the finalization process by using the command `tail -f config.log | grep "finalized"`. This will filter the log entries to show you only the lines that mention "finalized," allowing you to track the finalization process in real-time. If your logs are reporting "finalized" then it's working!

If you need more help tracking logs or errors then review the general FAQ section more help.&#x20;

### Q: What does it mean to be in queue for earnings, and how is the queue managed?

A: Your place in the queue is governed by the testnet network trustlessly reporting the state of your shdwNode. The main source of data and truth for operator uptime comes from the D.A.G.G.E.R. ledger that is trustlessly agreed upon and gossiped between all shdwNodes (read more about this in the [litepaper](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf)). These shdwOperator metrics are therefore "on-ledger" and tamperproof, and are scraped by a shdwOracle that integrates with a Solana smart contract. The Solana smart contract is a second economic layer that is a tamperproof and trustless verifier of shdwOperator stake in the D.A.G.G.E.R. testnet. This forms a consensus backbone for uptime, with the physical state of shdwDrive infrastructure validated by D.A.G.G.E.R. and the economic state by Solana.

Uptime is achieved the moment your shdwNode successfully performs its first sync request within the comms module (see [litepaper](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf)). Specifically, when it successfully handles an incoming sync request or a response to a sync request to another peer without error. This sync becomes a gossip event that is published to the ledger, agreed upon by a "shadowy council" that it is a strongly seen event (valid) and published in the logs. This is a collectible metric directly from the D.A.G.G.E.R. runtime by an Oracle. Failing to perform these syncs places your node in a flagged status (a grace period). After two full D.A.G.G.E.R. epochs have passed with in a flagged status, the node is internally marked as down and therefore reported to the shdwOracle as disconnected. This is why it is important to restart and resolve errors on your shdwNode within 2 epochs.

The queue is formed based on the network's global agreed upon order of success/failure of an individual shdwNode's effectiveness at syncing. This total ordering happens first across all nodes with the appropriate amount of stake. Assuming everyone has the appropriate amount of stake, there is one total order of shdwNodes where the first 150 are actively earning and those that come after the first 150 are in queue waiting for a shdwNode ahead of them to timeout, get evicted, or somehow fail to reconnect to the network after 2 epochs. Nodes without the appropriate amount of stake, or who have failed to verify their stake through Discord verification, are actively participating in the network and in it's consensus however are not placed in any queue.

This is a widely varying dynamic queue. Why? Everyone's network hardware, DNS, local conditions, luck and skills are completely different. Decentralized network are highly heterogenous and everyone's experience with uptime will differ. Furthermore, this is a testnet. Things will change, break and often act differently than expected thereby potentially impacting some more than others in regards to uptime. This is a learning process, and embracing the flux while trying to maintain uptime in the chaos is designed to be rewarded. The queue as you can see is straightforward, but the manner in which it emerges through the testnet framework and unpredictable network conditions make it seemingly less predictable. Focus on learning your shdwNodes behavior, meet the staked minimum, and be consistent in maintaining uptime and you will move up in the queue replacing those ahead of you who fail to do as good of a job. There are many restarts and bugs in testnets, and therefore many chances for the diligent to work their way into the 150.

### Q: How can I see the queue or is there a way for me collect the queue data on my own?

A: The best way to see the queue is to look at the [leaderboard](https://testnet.shdwdrive.com/uptime-leaderboard) and locate your shdwNode ID. There is currently no collectible queue metric that is packaged as a public facing API. We have intentionally decided to not enable this feature as we deem it lower priority at this stage of a testnet. This is best left as a future feature release being that we will evolve and improve the internal metric collection over time.

### Q: How do I track my progress and the progress of the network?

A: Operators can visit the [leaderboard](https://testnet.shdwdrive.com/uptime-leaderboard) to check their status. A [public dashboard](https://dashboard.shdwdrive.com/d/b14b5606-fb9c-4a2a-84fc-80887f144965/dagger-public-data?orgId=1\&refresh=30m) is available for overall network monitoring.

### Q: How are earnings paid out and how do I claim my shdwOperator earnings?

A: The shdwOperator incentives are calculated in real time (milliseconds) on-chain via D.A.G.G.E.R.'s internal consensus runtime. This real time available ledger data is then scraped by a shdwOracle server and packaged for Solana smart contract integration. This trustlessly and transparently powers the data behind the shdwOperator [leaderboard](https://testnet.shdwdrive.com/uptime-leaderboard), network [dashboard](https://dashboard.shdwdrive.com/d/b14b5606-fb9c-4a2a-84fc-80887f144965/dagger-public-data?orgId=1\&refresh=30m), and portal for claiming operator rewards.

This has been explain in detail in the [Testnet 2 blog](https://www.shdwdrive.com/blog/shdwdrive-v2-incentivized-testnet). You can read the sections "Rewards Details for shdwOperators" and "Expected Earnings and Costs" for an general understanding of how earnings will be handled.

IMPORTANT CHANGE - We have previously stated that staking SHDW through the shdwNode wallet gives preferential treatment for rejoining the network, **however this is no longer the case.**

Instead, **you need to have 100 SHDW at minimum across all wallets that are verified with the Discord verification system in order to maintain your verified status and therefore your place in the network. This SHDW may be staked without causing issue to your eligibility!**

### **Q: What are common issues with locating and importing my private key?**

* **Incorrect Format Error:** If you receive an "incorrect format" error when pasting the private key, ensure that you're copying the key correctly without any additional characters or line breaks. The key should be a continuous string without brackets, commas, or percentage symbols.
* **Command Not Found Error:** If you encounter a "command not found" error when running the `shdw-keygen` command, ensure that you're in the correct directory and that you have the necessary permissions to execute the binary. You may need to use `./` before the command (e.g., `./shdw-keygen privkey ~/id.json`).
* **Outdated `shdw-keygen` Version:** If the `privkey` subcommand isn't recognized, you may have an outdated version of `shdw-keygen`. Download the latest version from the official source, as indicated in the chat history, and try running the command again.
* **File Permission Issues:** Ensure that the `shdw-keygen` binary has the correct permissions set. If necessary, use the `chmod` command to make it executable (e.g., `chmod +x shdw-keygen`).
* **Incorrect Keypair Path:** Double-check the path to your keypair file. If you've moved the `id.json` file from its default location or renamed it, you'll need to provide the correct path when using the `shdw-keygen` command.

### Q: Do I have to wait a certain amount of epochs before restarting?

A: There had been a previous recommendation of waiting 5 epochs between each restart, **however this is no longer the case**. You may attempt restarts as quickly as you can throughout the 2 epoch grace period. The timeout period for getting evicted from the network is 2 epochs. Eviction means you will no longer accumulate uptime and reduce the likelihood of being in the top 150 earners. If you are evicted then you can just restart and rejoin the earnings queue and still continue to be an active participant of the network.

As a best practice, we recommend automated your restart process through scripting that detects both node errors and restart announcements (many have shared methods in the shdwOperator Discord on how to do this), and paying very close attention to all announcements that call for restarts.

### Q: Why am I not able to withdraw immediately from the staking site?

A: You are able to withdraw after waiting approximately 53 hours for Solana epoch cycles. We have chosen to align with Solana epoch cycles for several technical reasons that can be generalized by our overall intent for shdwDrive to run in harmony with Solana. Also, much like Solana, having waiting cycles based on epochs strengthen the network against certain type of vulnerabilities, and since SHDW is a native Solana SPL we are able to utilize Solana epochs rather than shdwDrive v2 epochs thereby freeing our testnet 2 for epoch experimentations without impact on staking withdraw cycles.

Please keep in mind we are constantly making improvements to user experience and if you have a feature idee or feedback you would like us to consider please open a Github issue suing think link: https://github.com/genesysgo/dagger-testnet-issues

### Q: What are the important links for Testnet 2, rewards, and staking?

A: See the following links:

* Staking: [https://testnet.shdwdrive.com/](https://testnet.shdwdrive.com/)
* Operator rewards: [https://testnet.shdwdrive.com/operator-rewards](https://testnet.shdwdrive.com/operator-rewards)
* Leaderboard: [https://testnet.shdwdrive.com/uptime-leaderboard](https://testnet.shdwdrive.com/uptime-leaderboard)
* Public dashboard: [https://dashboard.shdwdrive.com/](https://dashboard.shdwdrive.com/)
* Announcements: ⁠[https://discord.gg/genesysgo](https://discord.gg/genesysgo)

## **General**

### **Q: Where can I find the roadmap for D.A.G.G.E.R.?**

A: The roadmap can be found on our official [blog](https://www.shdwdrive.com/blog/dagger-roadmap) or [here](../token/roadmap.md), which outlines the plans for upcoming testnet phases.

### **Q: How many testnet phases will there be, and when is the estimated Mainnet launch?**

A: The number of testnet phases and the Mainnet launch date are still to be determined. The team is focused on ensuring stability and performance before setting any deadlines.

### **Q: Can I set up a shdwNode if I have never used Linux before?**

A: Yes, with a willingness to learn and some research, such as consulting YouTube tutorials or using resources like ChatGPT for Linux guidance, you can set up a node. The community and the engineering team are also available to help with questions.

### **Q: Where can I rent a server to run a shdwNode?**

A: Most cloud providers offer suitable instances for running a shdwNode. Many operators currently use Contabo. Digital Ocean is a solid standard for comparison, but always ensure the provider meets the system requirements for running a node.

### **Q: What hardware do I need to set up a shdwNode?**

A: The recommended system requirements are 8 cores or 16 vCPU (16 threads) , 32GB RAM, running Ubuntu 22.04 LTS with kernel version 5.15.0 or newer. You will want 2 TB or more at this testnet stage to avoid frequent restarts, although smaller storage drives will not prevent a node from running. Alternatively, you can use parts from an old gaming machine or any custom server setup that meets these minimum requirements.

### **Q: Can I use a virtual machine to run a shdwNode?**

A: Yes, shdwNodes have been successfully tested on virtual machines like VirtualBox or VMware, as well as on various cloud platforms. Ensure that the VM meets the recommended system requirements for optimal performance.

### **Q: Can I run a node in Docker?**

A: Running a node in Docker is possible, and some community members may share images or guides. However, there's no official long-term or stress-tested Docker image provided by GenesysGo at the moment.

### **Q: Is it safe to upgrade from Ubuntu 20.04 to 22.04?**

A: Yes, many users have successfully performed an in-place upgrade from Ubuntu 20.04 to 22.04 without issues.

### **Q: What should I do if I encounter issues with the Wield service or during the setup process?**

A: First, ensure your `config.toml` is set up correctly and that you've given execution permissions to `start_wield.sh` with `chmod +x`. Check the service status with `sudo journalctl -u wield` for more details. If `wield.service` shows as failed, try adjusting the settings in `start_wield.sh` or use the `--help` command for guidance. Share any errors or issues in the support channel for further assistance.

### **Q: How can I troubleshoot errors or issues with my shdwNode?**

A: For troubleshooting, check the system logs, review the node's configuration files for errors, and ensure that all dependencies are installed correctly. You can also seek help from the Discord community support channels by sharing specific error messages or logs.

### **Q: How do I check the status and log files for my shdwNode?**

A: Log files can be checked using command-line tools like `cat`, `less`, `tail`, or `grep`. The specific log file location may vary based on your node configuration. Use `journalctl` to check systemd service logs, for example, `sudo journalctl -u wield.service`. You can check the status of your node by running the command `sudo systemctl status wield`. This will give you an overview of your node's current operational status. You can monitor the finalization process by using the command `tail -f config.log | grep "finalized"`. This will filter the log entries to show you only the lines that mention "finalized," allowing you to track the finalization process in real-time. If your logs are reporting "finalized" then it's working!

### **Q: Where can I find the log file on my server?**

A: The log file is usually located in `/home/dagger/config.log`.

### **Q: How do I find my node ID?**

A: Run `./shdw-keygen pubkey id.json` in the `/home/dagger` directory, assuming you have the `shdw-keygen` program downloaded. The latest version of shdw-keygen also has a privkey function described [here](wallet.md#importing-usdshdw-accounts-into-solana-wallets).

### **Q: How do I check my node's version and upgrade if necessary?**

A: You can check your node's version with `./wield --version`. To upgrade, you can use the interactive installer script, which now includes an option to upgrade.

### **Q: How do I update to the latest version of the software?**

A: You can update your node by stopping the service, downloading the latest binary, and then restarting the service. Detailed commands are provided by users in the chat.

### **Q: What is log rotation and how do I set it up for my node?**

A: Log rotation is a system for managing log files so they don't consume too much disk space. You can set up log rotation using the `logrotate` utility on Linux, configuring it to rotate your node's log files based on size or time.

### **Q: Can I run a shdwNode on a system with less than the recommended specs for testing purposes?**

A: You can try running a node on a system with lower specs, but it may not perform optimally and may crash, especially if the RAM and CPU power do not meet the minimum requirements. The team may learn from your experience, but there's no guarantee it will work smoothly.

### **Q: How can I check system usage on my server?**

A: On an Ubuntu server, you can check system usage by opening the Ubuntu System Monitor or by using command-line tools such as `htop`, `top`, or `vmstat`. These tools provide information on CPU, memory, and disk usage.

### **Q: What is an epoch on the D.A.G.G.E.R. network?**

A: An epoch on the D.A.G.G.E.R. network is a time period that can be as short as 5-10 minutes currently, depending on network activity.

### **Q: How long is an epoch and where can I monitor the progress?**

A: An epoch is now 64 bundles in length.

### **Q: Can I close the terminal window after starting a node on a VPS?**

A: Yes, you can close the terminal window if you are using a service manager like `systemd` to run your node. The service will continue to run in the background. Use `systemctl status <service_name>` to check the status of your node.

### **Q: What should I do if I encounter a 'Resource temporarily unavailable' error when starting my node?**

A: This error usually means that a required resource, such as a database file, is locked because it is in use by another process. Make sure that no other instance of the node is running. You may need to stop the running service before starting it manually.

### **Q: What should I do if I receive an error about an invalid epoch in my logs?**

A: This could indicate that either the peer or your node is very behind. Make sure your node is up-to-date and has restarted correctly. You may have to wait several epochs before attempting to start your node again.

### **Q: What should I do if I encounter handshake errors or max handshake duration exceeded messages?**

A: Ensure you have the correct trusted peer values in your `config.toml` and that you have waited the full 5 epochs after an update before restarting your node.

### **Q: Is there a way to configure my node to use more memory or CPU resources?**

A: The node software will automatically use the resources it needs up to the limits of what is available on your system. If you notice low resource usage, it could be due to network idle times or inefficiencies in the node software that may be addressed in future updates.

### **Q: What happens if I accidentally try to start a second instance of my node?**

A: If you try to start a second instance while one is already running, you may encounter errors related to locked resources or port conflicts. Ensure that only one instance is running at any given time.

### **Q: Can I deploy a shdwNode on a server that's already hosting another node?**

A: It's possible, but ensure that the server has enough resources to handle both nodes without affecting performance. Monitor resource usage closely.

### **Q: How can I monitor the progress of epochs on the network?**

A: The progress of epochs can typically be monitored through the node's logs or through the [dashboard](https://dashboard.shdwdrive.com/) provided by the D.A.G.G.E.R. team.

### **Q: What should I do if I notice the network has slowed down or is not progressing?**

A: Network slowdowns can occur for various reasons. Stay updated with official announcements from the D.A.G.G.E.R. team for information on network status and any actions you may need to take.

### **Q: What if I encounter a GLIBC not found error when setting up my node or generating a keypair?**

A: This error usually indicates that you're not on the correct version of Ubuntu or the Linux kernel. Ensure you are using Ubuntu 22.04 with kernel 5.15.0 or newer. You may need to perform system updates (`sudo apt update` and `sudo apt upgrade`) and possibly reboot your machine.

### **Q: How can I ensure my node stays live after closing the terminal?**

A: To ensure your node remains active after closing the terminal, run it as a background service using `systemd` or `screen`. This way, the process isn't tied to your terminal session.

### **Q: How can I increase the limits of my node's resource usage?**

A: Limits on resource usage are generally defined by the system's capabilities and the node's software. There may be configuration options that allow for tuning, but these should be used with caution to avoid instability.

### **Q: How do I determine the profitability of running a node?**

A: Profitability will depend on various factors, including the rewards structure, operational costs, and network performance. Details on operator rewards, including allowances and penalties for downtime, will be provided by the D.A.G.G.E.R. team as the testnet progresses.

### **Q: What should I do if I encounter unfamiliar terms or need further assistance?**

A: Don't hesitate to ask for clarification or assistance in the Discord support channel. The community and the core engineering team are there to help you through the process.

### **Q: How do I share my server log file?**

A: You can download your log file from your server using the `scp` command or a similar method, and then upload it to a sharing service.

### **Q: Can I use a hardware wallet like Ledger with my node?**

A: No, you need a keypair on your node, and using a hardware wallet like Ledger will not work for node operations.
