# AutomatedMediaServer
Guide for myself detailing the process of how to setup my Jellyfin server with automated media collection

Hardware used: 
- Optiplex 7010 SFF
- i7-3770
- 24GB DDR3 1600
- Samsung 860 QVO 1TB 2.5" SATA SSD (Boot Drive)
- Nvidia Quadro K620

## Setup the OS
1. Go to [the official Ubuntu website](https://ubuntu.com/download/desktop) and grab the latest ISO for 22.04.1 LTS 
2. Go the [the official Balena Etcher website](https://www.balena.io/etcher#download-etcher) and download the proper version for your OS
3. Plug in the USB and flash the ISO onto it
4. Plug in the USB to the media server machine and install Ubuntu (minimal installation, download updates, + install 3rd-party software)
5. Load into the OS and navigate through the setup pop-up
    - don't connect any online accounts
    - don't setup Livepatch
    - I send system info, but it's not necessary
    - location services off
    - click done
6. Software Updater pop-up will appear(you might need to give it a sec). Click **Install Now**
7. After it finishes installing all the updates, the Software Updater icon on the left will get rid of the progress bar. Click the icon and press **Restart Now**
8. Run ```sudo apt update && sudo apt upgrade``` in the terminal and accept
9. Restart OS (not really necessary but makes my goofy Windows-user self feel better)

<details><summary>Some things I do personally in settings to make the experience better:</summary>
<p>
    
- Go in Appearance and switch it to dark
- Go in Background and change it to something darker 
- Go in Power and change Screen Blank to 15 minutes or never 
- Go in Displays, Night Light at the top, Turn Night Light on, swap to Manual Schedule, and change the times to the same number to make it always enabled 
- Go into Date & Time and change Time Format to AM/PM 
- Go into About and make sure all my hardware shows up
- Unpin unnecessary applications from the sidebar, and pin Terminal and System Monitor
</p>
</details>

## Setup Docker
It's going to be installed through the repository as I personally find that to be the simplest way. These instructions are copied straight from the official Docker Documentation[^1].

### Set up the repository

**1\. Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:**

    sudo apt-get update
    sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release

**2\. Add Docker’s official GPG key:**

    sudo mkdir -m 0755 -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

**3\. Use the following command to set up the repository:**

    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

### Install Docker Engine

**1\. Update the `apt` package index:**

    sudo apt-get update
    
_<details><summary>Click this if you get a GPG error</summary>_
<p>
Your default umask may be incorrectly configured, preventing detection of the repository public key file. Try granting read permission for the Docker public key file before updating the package index:

    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    sudo apt-get update

</p>
</details>

**2\. Install Docker Engine, containerd, and Docker Compose.**

To install the latest version, run:
    
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

**3\. Verify that the Docker Engine installation is successful by running the `hello-world` image:**

    sudo docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.

### Manage Docker as a non-root user
By default it’s the `root` user that owns the Unix socket, and other users can only access it using `sudo`. The Docker daemon always runs as the `root` user. The next few steps will add the current running user to the `docker` group that was automatically created during the Docker Install steps above. These steps are also copied straight from the official Docker Documentation[^2]

**1\. Fix permissions.**

    sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
    sudo chmod g+rwx "$HOME/.docker" -R

_<details><summary>Explanation of why we had to do that</summary>_
<p>

We initially ran a Docker CLI command (the `hello-world` test) using `sudo` before adding ourselves to the `docker` group, which would cause the following error:

>WARNING: Error loading config file: /home/user/.docker/config.json -<br>stat /home/user/.docker/config.json: permission denied

This error indicates that the permission settings for the `~/.docker/` directory are incorrect, due to having used the `sudo` command earlier.

Running the permission fix command above allows us to do the following steps without having issues.
    
</p>
</details>

**2\. Add your user to the docker group.**

    sudo usermod -aG docker $USER
**3\. Log out and log back in so that your group membership is re-evaluated.**

**4\. Verify that you can run `docker` commands without `sudo`.**

    docker run hello-world
Docker will automatically start during system boot when installed on Debian and Ubuntu, so we're done with post-install setup.

[^1]: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
[^2]: https://docs.docker.com/engine/install/linux-postinstall/
