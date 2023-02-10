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
7. After it finishes installing all the updates, it will ask for a restart. Do that 
8. Run ```sudo apt update``` in the terminal, and then when it finishes finding all updates, run ```sudo apt upgrade``` and accept
9. Restart OS (not really necessary but makes my goofy Windows-user self feel better)

## Install Docker
