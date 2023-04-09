- [Ubuntu 22.04](#ubuntu-2204)
  - [0. youtube video](#0-youtube-video)
  - [1. update the drivers](#1-update-the-drivers)
  - [2. reboot](#2-reboot)
  - [3. install docker](#3-install-docker)
  - [4. docker \& container toolkit](#4-docker--container-toolkit)
  - [5. clone the repo](#5-clone-the-repo)
  - [6. prepare models](#6-prepare-models)
  - [7. prepare .env file](#7-prepare-env-file)
  - [8. startup docker container](#8-startup-docker-container)
- [Manjaro](#manjaro)
  - [update the drivers](#update-the-drivers)
  - [reboot](#reboot)
  - [docker \& container toolkit](#docker--container-toolkit)
  - [continue with ubuntu task](#continue-with-ubuntu-task)
- [Windows](#windows)
- [notes](#notes)

# Ubuntu 22.04

## 0. youtube video
A video walking you through the setup can be found here:

[![oobabooga text-generation-webui setup in docker on ubuntu 22.04](https://img.youtube.com/vi/ELkKWYh8qOk/0.jpg)](https://www.youtube.com/watch?v=ELkKWYh8qOk)


## 1. update the drivers
in the the “software updater” update drivers to the last version of the prop driver.

## 2. reboot
to switch using to new driver

## 3. install docker
```bash

sudo apt update
sudo apt-get install curl
sudo mkdir -m 0755 -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-compose -y
sudo usermod -aG docker $USER
newgrp docker
```

## 4. docker & container toolkit
```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://nvidia.github.io/libnvidia-container/stable/ubuntu22.04/amd64 /" | \
sudo tee /etc/apt/sources.list.d/nvidia.list > /dev/null 
sudo apt update
sudo apt install nvidia-docker2 nvidia-container-runtime -y
sudo systemctl restart docker
```

## 5. clone the repo
```
git clone https://github.com/oobabooga/text-generation-webui
cd text-generation-webui
```

## 6. prepare models
download and place the models inside the models folder. tested with:

4bit
https://github.com/oobabooga/text-generation-webui/pull/530#issuecomment-1483891617
https://github.com/oobabooga/text-generation-webui/pull/530#issuecomment-1483941105

8bit:
https://github.com/oobabooga/text-generation-webui/pull/530#issuecomment-1484235789

## 7. prepare .env file
edit .env values to your needs.
```bash
cp .env.example .env
nano .env
```

## 8. startup docker container
```bash
docker-compose up --build
```

# Manjaro
manjaro/arch is similar to ubuntu just the dependency installation is more convenient

## update the drivers
```bash
sudo mhwd -a pci nonfree 0300
```
## reboot
```bash
reboot
```
## docker & container toolkit
```bash
yay -S docker docker-compose buildkit gcc nvidia-docker
sudo usermod -aG docker $USER
newgrp docker
sudo systemctl restart docker # required by nvidia-container-runtime
```

## continue with ubuntu task
continue at [5. clone the repo](#5-clone-the-repo)

# Windows
coming soon


# notes

on older ubuntus you can manually install the docker compose plugin like this:
```
DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
mkdir -p $DOCKER_CONFIG/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.17.2/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
export PATH="$HOME/.docker/cli-plugins:$PATH"
```