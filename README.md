# Loftwah's Cheatsheet

This is a repo with a bunch of stuff I use regularly.

## Amazon Web Services

[ap-southeast-2](https://ap-southeast-2.console.aws.amazon.com/console/home?region=ap-southeast-2)

## [Canva](https://canva.com)

Can’t forget about graphics and design. I like Canva so I use it.

[](https://www.canva.com/)

## [CloudFlare](https://dash.cloudflare.com/)

[Developers](https://developers.cloudflare.com/)

## [CyberChef](https://gchq.github.io/CyberChef/)

CyberChef - The Cyber Swiss Army Knife

## [DevDocs.io](https://devdocs.io/)

## Docker 

[Docs](https://docs.docker.com/)

### Clean up Docker before starting

```bash
docker kill $(docker ps -q)
docker rm $(docker ps -a -q)
docker rmi $(docker images -q)
```

### Install Docker on Amazon Linux 2

[Docker basics for Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html)

```bash
sudo yum update -y
sudo amazon-linux-extras install docker -y
sudo service docker start
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user
```

### Install Docker on Ubuntu

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
sudo service docker start
sudo systemctl enable docker
sudo usermod -a -G docker <username>
```

### Install Docker-Compose

```bash
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version
```

## Git and GitHub

### Initial Git configuration

```bash
git config --global user.name "Dean Lofts"
git config --global user.email "dean@deanlofts.xyz"
```

### Clone and work with the [Apptizle.io](http://Apptizle.io) repository (assumes VSCode)

```bash
git clone https://github.com/loftwah/apptizle.io && cd apptizle.io
code .
```

## Google

| [Admin Console](https://admin.google.com/ac/home?hl=en) | [Cloud Platform](https://console.cloud.google.com/home/dashboard) | [Drive](https://drive.google.com/drive/u/0/) | [Gmail](https://mail.google.com/) | [Sheets](https://sheets.google.com/) | [How Search Works](https://www.google.com/search/howsearchworks/?fg=1) |

## Linux

### How to install Linux?

We’re going to be using Ubuntu for our example and you can download it here. 

[Get Ubuntu Server | Download | Ubuntu](https://ubuntu.com/download/server)

Linux Basics

[An Introduction to Linux Basics | DigitalOcean](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-basics)

### How to set up and use SSH keys?

[How to Set Up SSH Keys on Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04)

[How to Create an SSH Shortcut | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-create-an-ssh-shortcut)

### How to set up a self signed certificate with a Custom Root CA

[Self Signed Certificate with Custom Root CA](https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309)

## [Microsoft 365](https://www.office.com/)

| [Admin Console](https://admin.microsoft.com/Adminportal/Home) | [Outlook](https://outlook.office365.com/mail/inbox) | [Teams](https://teams.microsoft.com/) |

## Node Version Manager

### Install NVM

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
## Only use below if you need it in ~/.zshrc or something
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
nvm install --lts

```

## Piping Server

Piping Server transfers data to POST /hello or PUT /hello into GET /hello. The path /hello can be anything such as /mypath or /mypath/123/. A sender and receivers who specify the same path can transfer. Both the sender and the recipient can start the transfer first. The first one waits for the other.

You can also use Web UI like https://ppng.io on your browser. A more modern UI is found in https://piping-ui.org, which supports E2E encryption.

**Stream**
The most important thing is that the data are streamed. This means that you can transfer any data infinitely. The demo below transfers an infinite text stream with seq inf.

### Install or Run the Piping Server

```bash
docker run -p 8181:8080 nwtgck/piping-server-rust
```

### How to use the Piping Server

```bash
Transfer
Piping Server is simple. You can transfer as follows.

# Transmit
cat <filename> | curl -T - https://pipe.apptizle.io/<filename>

# Receive
curl https://pipe.apptizle.io/<filename> > <filename>
```

## Portainer

[https://docs.portainer.io/v/ce-2.11/start/install/server/docker/linux](https://docs.portainer.io/v/ce-2.11/start/install/server/docker/linux)

### Install Portainer

```bash
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:2.11.0
```

### Templates

The following URLs can be used for Templates in Portainer: 

[](https://raw.githubusercontent.com/technorabilia/portainer-templates/main/lsio/templates/templates-2.0.json)

## Visual Studio Code

To keep things consistent I will be using Visual Studio Code for this. You can use any text editor, but this is what the examples will expect.

[Download Visual Studio Code - Mac, Linux, Windows](https://code.visualstudio.com/download)

[Visual Studio Code for the Web](https://vscode.dev/)
