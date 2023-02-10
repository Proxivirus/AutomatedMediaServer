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
4. Plug in the USB to the media server machine and install Ubuntu (minimal installation, download updates + 3rd party drivers)
5. Load into the OS and navigate through the setup pop-up
    - don't connect any online accounts
    - don't setup Livepatch
    - I send system info, but it's not necessary
    - location services off
    - click done
6. Software Updater pop-up will appear(you might need to give it a sec). Click install now
7. After it finishes installing all the updates, it will ask for a restart. Press "restart now"
8. Run ```sudo apt update``` in the terminal, and then when it finishes finding all updates, run ```sudo apt upgrade``` and accept
9. Restart OS (not really necessary but makes my goofy Windows-user self feel better)

<details><summary>Some things I do personally in settings to make the experience better:</summary>
<p>
    
- Go in Appearance and switch it to dark
- Go in Background and change it to something darker 
- Go in Power and change Screen Blank to 15 minutes or never 
- Go in Displays, Night Light at the top, Turn Night Light on, swap to Manual Schedule, and change the times to the same number to make it always enabled 
- Go into Date & Time and change Time Format to AM/PM 
- Go into About and make sure all my hardware shows up
</p>
</details>

## Install Docker
It's going to be installed through the repository as I personally find that to be the simplest way. These instructions are copied straight from the official Docker Documentation[^1].

### Set up the repository

1\. Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

    sudo apt-get update
    sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release

2\. Add Docker’s official GPG key:

    sudo mkdir -m 0755 -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

3\. Use the following command to set up the repository:

    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

### Install Docker Engine

1\. Update the `apt` package index:

    sudo apt-get update

[^1]: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
