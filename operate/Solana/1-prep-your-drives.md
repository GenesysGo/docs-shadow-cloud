# 1 - Prep Your Drives

It's hard to pinpoint which of your server resources is most important for running a Solana RPC. Is it CPU? Is it RAM? Is it storage? Is it network? To be totally honest, all of them are equally important and critical to run a Solana RPC. Maybe you could skimp a little bit on a validator node, but not RPCs. RPCs get clobbered.

It is dead certain that you need nVME storage, though, and a lot of it. Both Solana RPCs and Validators do the exact same thing under the hood. The only difference is RPCs serve lookup requests, and validators vote on proposed blocks on the blockchain. Everything else is the same.

So whether you are building an RPC or a validator, you begin your journey with prepping your storage.

### Solana melts through your storage in 4 different ways:

* Accounts database
* The Ledger
* Snapshots
* Logs

In a perfect world, all four of these items would be stored on their own dedicated drive with boatloads of storage. This world is not perfect as evidenced by the fact that Cardano exists, and if you are renting a server from a data center provider, you will probably only have an OS drive and 2 big nVME drives,

**Our recommendation is to put logs on your OS drive, Accounts on one of the nVME drives, and the Ledger and snapshots on the second nVME drive.** Here is how to do it.

Let's pretend you have a 256 GB OS drive, and 2x 3.9 TB nVME drives. Let's further pretend your server is provisioned, _**but only the OS drive has a partition created and a filesystem mounted. The nVME drives are just sitting there, completely blank. This is common for nearly every deployment.**_

We have to first bring the nVME drives to life by creating partitions and then adding a filesystem to them.

Let's get through the absolute basics first, though. We need to patch the server, and create a user account that we can run our Solana processes under. **Make absolutely sure you create the sol user, and execute the last command to act as the sol user for all subsequent tasks.**

```bash
apt update
apt upgrade -y
apt dist-upgrade
# now create the user account sol
adduser sol
# type in a password for the user sol
usermod -aG sudo sol
# important that you use the below command to switch to the sol user
# we will perform all subsequent commands under the context of sol
su - sol
# type in the password if prompted
```

**Now let's set the hostname for your server.** Shadow Operators are preferred to use the following format for setting their hostname:\
`shadow-region-discorduser-discordnumber-servernumber`

i.e.,: `shadow-na-knoxtrades-2784-1`

Use the following command to set your hostname, replacing the shadow-region... with your own hostname,

```bash
sudo hostnamectl set-hostname shadow-region-discorduser-discordnumber-servernumber
```

**Now we will move on to creating the partitions on our nVME drives. Pay close attention so you don't brick your server.**

When you reserved your server, you most likely reserved one with three drives. What is tricky is figuring out which drives are your empty nVME drives. For instance, your server may have been deployed with 3 physical nVME drives, one of which is used for the OS. We need to find the other two nVME drives and partition them without touching the OS drive.

```bash
ls /dev | grep "nvme"
nvme0
nvme0n1
nvme1
nvme1n1
nvme2
nvme2n1
nvme2n1p1
```

See how nvme2 on my server has a "p1"? That means it has a partition already on it and it's safe to assume that is the OS drive. The other drives do not have a p1, meaning they are empty and need to be partitioned. Perhaps your OS drive is actually an SSD, and it would not be returned in the results of the above command at all, in which case you are safe to proceed

My objective is threefold:

* On nvme0n1, I want to create a 480GB partition that can be used for swap
* Then, on nvme0n1, I want to create a second partition to be used for accounts that consumes the remainder of the drive
* Then, on nvme1n1, I want to create a partition for the ledger and snapshots that consumes the entire drive

We use gdisk to accomplish this task. Gdisk is a guided CLI partitioning tool. You will answer a series of questions to partition the drives.

```bash
sudo gdisk /dev/nvme0n1
# create the swap partition first
# we will create this as partition 2 (ie, ends with p2)
> n #new partition
> 2 #partition 2
> (enter) # press enter to accept default first sector
> +480G # basically saying create a 480GB partition
> 8200 # code 8200 tells linux this is a swap partition
> n # move on to create the next partition on nvme0n1
> 1 # partition 1
> (enter) # press enter, first available sector
> (enter) # press enter, last available sector consumes rest of drive
> 8300 # tells linux this is a filesystem
> w # write it to disk
> y # confirm and exit
```

Now create the last partition on nvme1n1

```bash
sudo gdisk /dev/nvme1n1
> n # create the next partition on nvme1n1
> 1 # partition 1
> (enter) # press enter, first available sector
> (enter) # press enter, last available sector consumes rest of drive
> 8300 # tells linux this is a filesystem
> w # write it to disk
> y # confirm and exit
```

Your devices should now look something like this:

```bash
ls /dev | grep nvme
nvme0
nvme0n1
nvme0n1p1
nvme0n1p2
nvme1
nvme1n1
nvme1n1p1
nvme2
nvme2n1
nvme2n1p1
```

You can also get more verbose details about your partitions using `sudo fdisk -l`.&#x20;

Now we need to format our partitions and mount the folders on the drives! We still have a ways to go to polish up storage, but we're getting there.

```bash
# Turn our data partitions into the ext4 filesystem
sudo mkfs -t ext4 /dev/nvme0n1p1
sudo mkfs -t ext4 /dev/nvme1n1p1
# create our main folders
# /mt will store accounts 
sudo mkdir /mt
sudo mount /dev/nvme0n1p1 /mt
# /extmt will store the ledger and snapshots
sudo mkdir /extmt
sudo mount /dev/nvme1n1p1 /extmt
#turn our spare nvme partition (the 480GB one) into swap
sudo mkswap /dev/nvme0n1p2
```

**Now we will change our swap directory to point to our new partition. Start by finding out what your current swap directory is set to.**

```bash
sudo swapon --show
```

It should be something like /dev/sdb2 or /dev/sdc2. It will also probably be something like 1.9GB but again, this varies from one data center provider to the next. Swap could just live on a file, like /swap.img. You'll have to think on your toes here. Now we turn it off, change our swappiness settings, and turn it back on to the new partition.

```bash
sudo swapoff /dev/sdb2
#edit swappiness settings to make machine less aggressive to use swap
echo 'vm.swappiness=30' | sudo tee --append /etc/sysctl.conf > /dev/null
#reload the config
sudo sysctl -p
#turn on the new swap partition
sudo swapon /dev/nvme0n1p2
```

**Now we're coming down the home stretch, but this is the trickiest part.** We need to make sure our Linux environment knows to enable all of these partitions on boot or reload.&#x20;

First, _**open notepad or a text editor on your computer.**_ Then run this command and view the output.

```bash
lsblk -f
```

Copy the entire nvme section to your text editor. It should look something like this, but your UUIDs will be different:

```
nvme1n1                                                                        
└─nvme1n1p1 ext4           5803ce75-5a18-4f08-9960-44ef9d99520b
nvme0n1                                                                        
├─nvme0n1p1 ext4           1dc4a46c-837c-4ecd-982e-77d252af3e85
└─nvme0n1p2 swap           02edc83b-5647-4db7-aee7-9000f33ea766
```

Now we are going to edit the boot and mount parameters. _**DO NOT RUSH IN AND OVERWRITE THE CONTENTS OF THIS FILE.**_&#x20;

```bash
sudo nano /etc/fstab
```

You should see something similar to this by default

```
UUID=37f9490c-38ab-4082-9b18-3a50e2567ba4       /       ext4    errors=remount-ro       0       1
UUID=d8218533-128c-4be6-8a7d-9d092e7b11ec       none    swap    none    0       0
```

This is the default configuration for your OS drive and your swap drive. Do NOT touch the OS drive (which you can see is indicated by the root folder "/"). **We want to replace the swap drive UUID with the UUID we copied into notepad - make sure to use YOUR UUID and not this example below:**

```
UUID=37f9490c-38ab-4082-9b18-3a50e2567ba4       /       ext4    errors=remount-ro       0       1
UUID=02edc83b-5647-4db7-aee7-9000f33ea766       none    swap    none    0       0
```

THEN, we want to add our additional 2 partitions to the bottom of this file. Again, make sure to use YOUR UUIDs and not the examples below.

```
UUID=37f9490c-38ab-4082-9b18-3a50e2567ba4       /       ext4    errors=remount-ro       0       1
UUID=02edc83b-5647-4db7-aee7-9000f33ea766       none    swap    none    0       0
#GenesysGo RPC Config
UUID=1dc4a46c-837c-4ecd-982e-77d252af3e85 /mt  auto nosuid,nodev,nofail 0 0
UUID=5803ce75-5a18-4f08-9960-44ef9d99520b /extmt auto nosuid,nodev,nofail 0 0
```

Press Ctrl+X, Y to save the file and exit nano.&#x20;

Your final storage task is to make sure everything is mounted.

```bash
sudo mount --all --verbose
```

(OPTIONAL) - Configure the CPU Governor to be in Performance mode. Proceed with caution on this one. We have seen significant improvement on nodes that run the Epyc 7443P. Also note, this will reboot your node.

```shell
sudo apt-get install cpufrequtils

echo 'GOVERNOR="performance"' | sudo tee /etc/default/cpufrequtils

sudo systemctl disable ondemand

sudo shutdown -r now
```

Now you may proceed to setting up your Solana environment!
