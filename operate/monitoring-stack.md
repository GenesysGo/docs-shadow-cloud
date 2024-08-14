---
description: >-
  The following instructions are for running in a Docker environment. The
  supplied docker-compose.yaml file includes a monitoring stack to allow you to
  monitor your node.
hidden: true
---

# Monitor

{% hint style="warning" %}
Working knowledge of Docker is recommended. GenesysGo will not be providing Docker support.
{% endhint %}

### Head over to [https://github.com/GenesysGo/dagger-monitoring](https://github.com/GenesysGo/dagger-monitoring) for the official dagger-monitoring code + instructions.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

**Docker install for 22.04 taken from DigitalOcean -** [**https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04**](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)

**Please note the below instructions are for 22.04, if you are installing this on another system, please review for your specific OS**

### Step 1: Installing Docker

The Docker installation package available in the official Ubuntu repository may not be the latest version. To ensure we get the latest version, we’ll install Docker from the official Docker repository. To do that, we’ll add a new package source, add the GPG key from Docker to ensure the downloads are valid, and then install the package.

First, update your existing list of packages:

```sh
sudo apt update
```

Next, install a few prerequisite packages which let apt use packages over HTTPS:

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Then add the GPG key for the official Docker repository to your system:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Add the Docker repository to APT sources:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

This will also update our package database with the Docker packages from the newly added repo.

Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:

```basic
sudo apt update && sudo apt install docker-ce
```

Finally, install Docker:

```bash
sudo apt-cache policy docker-ce 
```

Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:

```bash
sudo systemctl status docker
```

The output should be similar to the following, showing that the service is active and running:

```
Output
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2020-05-19 17:00:41 UTC; 17s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 24321 (dockerd)
      Tasks: 8
     Memory: 46.4M
     CGroup: /system.slice/docker.service
             └─24321 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client. We’ll explore how to use the docker command later in this tutorial.
```

### Step 2: Executing the Docker Command Without Sudo (Optional but Recommended)

By default, the docker command can only be run the root user or by a user in the docker group, which is automatically created during Docker’s installation process. If you attempt to run the docker command without prefixing it with sudo or without being in the docker group, you’ll get an output like this:

Output docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?. See 'docker run --help'. If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:

```bash
sudo usermod -aG docker ${USER}
```

To apply the new group membership, log out of the server and back in, or type the following:

```bash
su - ${USER}
```

You will be prompted to enter your user’s password to continue.

Confirm that your user is now added to the docker group by typing:

```bash
groups
```

Output

```
dagger sudo docker
```

If you need to add a user to the docker group that you’re not logged in as, declare that username explicitly using:

```bash
sudo usermod -aG docker <username>
```

**The rest of this article assumes you are running the docker command as a user in the docker group. If you choose not to, please prepend the commands with sudo.**

**Installing Docker Compose - Taken from DigitalOcean with GG edits to support current version-** [**https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04**](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04)

### Step 3: Installing Docker Compose & Starting Up

To make sure we obtain the most updated stable version of Docker Compose, we’ll download this software from its official GitHub repository.

First, confirm the latest version available in their releases page. At the time of this writing, the most current stable version is 2.23.3.

The following command will download the 2.23.3 release and save the executable file which will make this software globally accessible as docker-compose:

```bash
mkdir -p ~/.docker/cli-plugins/
```

```sh
curl -SL https://github.com/docker/compose/releases/download/v2.23.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
```

Next, set the correct permissions so that the docker-compose command is executable:

```sh
chmod +x ~/.docker/cli-plugins/docker-compose
```

To verify that the installation was successful, you can run:

```sh
docker compose version
```

You’ll see output similar to this:

```
Output
docker-compose version v2.23.3
```

Create a docker storage volume for Grafana so that its persistent during reboots:

```sh
docker volume create grafana-storage
```

_**Clone the Repo from Github**_

Run the following:

```
git clone https://github.com/GenesysGo/dagger-monitoring && cd dagger-monitoring
```

_**Run Docker Compose and turn up the monitoring**_

Ensure you're in the proper directory

```
/home/dagger/dagger-monitoring
```

and you see the docker-compose.yaml file.

```sh
docker compose up -d
```

Connect to Grafana by going to the IP of the server with port 3000 in your browser:

Example:

```
http://1.2.3.4:3000
```

Login with default credentials

```
admin/admin
```

**Importing Prometheus as a dataset to Grafana**

Hover over the gear button on the left panel near the bottom of the column and click **Data Sources**. Click **Add Data Source** in the upper right hand side. Select **Prometheus** from the list (will most likely be near the top). [http://prometheus:9090](http://prometheus:9090/) (the hostname prometheus will work because of the shadow docker-compose built docker network). **ENSURE PORT IS 9090, NOT 9000** Add and save at the bottom.

Navigate on the left to the Dashboards button. This looks like 4 squares stacked. Click import in the upper right of this screen. Under import via grafana.com, type: `1860`

Once this loads, click import at the bottom.

You now have a dashboard for your wield node and should start to see your metrics populate the dashboard soon.

**NOTE: There's a glitch with the latest version of Grafana. If any of the charts/gauges look like they're cut off, you just have to click + drag to expand them again. This is just a visual bug with Grafana.**
