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

*   Docker resources usage - `docker info`
*   know how much space is taken by a particular container `docker container ls -s`
*   Know how much spaces is used by Docker Root Dir `du -h --max-depth=1 /var/lib/docker`
*   Docker storage usage `docker system df`
*   Docker list volumes `docker volume ls`
*   Docker list images that are locally stored with the Docker Engine `docker image ls`
*   Docker inspect volumes `docker volume inspect VOLUME NAME`
*   Remove a group of images - `docker images | grep "<none>" | awk '{print $3}' | xargs docker rmi`
*   Remove all untagged containers - `docker rm $(docker ps -aq --filter status=exited)`
*   Remove all untagged images - `docker rmi $(docker images -q --filter dangling=true)`
*   Remove old (dangling) Docker volumes - `docker volume rm $(docker volume ls -qf dangling=true)`
*   Docker remove redundant objects at once `docker system prune`
*   Install on Ubuntu - `curl -sSL https://get.docker.com/ubuntu/ | sudo sh`
*   Get stats from all containers on a host - `docker ps -q | xargs docker stats`
*   Tail last 300 lines of logs for a container - `docker logs --tail=300 -f <container_id>`
*   Build an image from the Dockerfile in thecurrent directory and tag the image `docker build -t myimage:1.0 .`
*   Pull an image from a registry `docker pull myimage:1.0`
*   Retag a local image with a new image name and tag `docker tag myimage:1.0 myrepo/myimage:2.0`
*   Push an image to a registry `docker push myrepo/myimage:2.0`
*   Run a container from the Alpine version 3.9 image, name the running container “web” and expose port 5000 externally, mapped to port 80 inside the container `docker container run --name web -p 5000:80 alpine:3.9`
*   Stop a running container through SIGTERM `docker container stop web`
*   Stop a running container through SIGKILL `docker container kill web`
*   List the networks `docker network ls`
*   Copy Docker images from one host to another without using a repository

 #Step1 - Save the Docker image as a tar file
 docker save -o <path for generated tar file> <image name> 
 
 #Example
 docker save -o c:/myfile.tar centos:16
 
 #Step2 - copy your image to a new system with regular file transfer tools such as cp, scp or rsync(preferred for big files)
 
 #Step3 - load the image into Docker
 docker load -i <path to image tar file>

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

#### Shell Scripts
    
*   Shell Script to Install Docker on Ubuntu
    
    #!/bin/bash
    set -e
    #Uninstall old versions
    sudo apt-get remove docker docker-engine docker.io containerd runc
    #Update the apt package index:
    sudo apt-get update
    #Install packages to allow apt to use a repository over HTTPS:
    sudo apt-get install -y \\
        apt-transport-https \\
        ca-certificates \\
        curl \\
        gnupg-agent \\
        software-properties-common
    # Add docker's package signing key
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    # Add repository
    sudo add-apt-repository -y \\
      "deb \[arch=amd64\] https://download.docker.com/linux/ubuntu \\
      $(lsb\_release -cs) \\
      stable"
    # Install latest stable docker stable version
    sudo apt-get update
    sudo apt-get -y install docker-ce
    # Enable & start docker
    sudo systemctl enable docker
    sudo systemctl start docker
    # add current user to the docker group to avoid using sudo when running docker
    sudo usermod -a -G docker $USER
     # Output current version
    docker -v
    
*   Shell Script to Install Docker on Centos
    
       #!/bin/bash
       #Get Docker Engine - Community for CentOS + docker compose
       set -e
       #Uninstall old versions
       sudo yum remove docker docker-common docker-selinux docker-engine-selinux docker-engine docker-ce
       #Update the packages:
       sudo yum update -y
        #Install needed packages
       sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    # Configure the docker-ce repo:
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    # Install the latest docker-ce
    sudo yum install docker-ce
    # Enable & start docker
    sudo systemctl enable docker.service
    sudo systemctl start docker.service
    # add current user to the docker group to avoid using sudo when running docker
    sudo usermod -a -G docker $(whoami)
    # Output current version
    docker -v
    
*   Shell Script to Install Docker on AWS linux
    
        #!/bin/bash
        #Get Docker Engine - Community for CentOS + docker compose
        set -e
        #Uninstall old versions
        sudo yum remove docker docker-common docker-selinux docker-engine-selinux docker-engine docker-ce
        #Update the packages:
        sudo yum update -y
        #Install the most recent Docker Community Edition package.
        sudo amazon-linux-extras install docker -y
        # Enable & start docker
        sudo service docker start
        # add current user to the docker group to avoid using sudo when running docker
        #sudo usermod -a -G docker ec2-user
        sudo usermod -a -G docker $(whoami)
        # Output current version
         docker -v
    

**Docker Compose**

*   Shell Script to Install the latest version of docker-compose
    
    #!/bin/bash
    # get latest docker compose released tag
    COMPOSE\_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag\_name' | cut -d\\" -f4)
    sudo curl -L "https://github.com/docker/compose/releases/download/${COMPOSE\_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod a+x /usr/local/bin/docker-compose
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
    # Output the  version
    docker-compose -v
    

**Dockerfile**

*   Dockerizing a simple nodeJs app

FROM node:4.6
WORKDIR /app
ADD ./app
RUN npm install
EXPOSE 3000
CMD npm start

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
    
**Git**

*   Remove deleted files from repo - `git rm $(git ls-files --deleted)`
*   Reset git repo (dangerous) - `git reset --hard HEAD`
*   Reset and remove untracked changes in repo - `git clean -xdf`
*   Ignore certificates when cloning via HTTPS - `git config --global http.sslVerify false`
*   Pull changes and remove stale branches - `git pull --prune`
*   Grab the diff of a previous version of a file - `git diff HEAD@{1} ../../production.hosts`
*   Grab the diff of a staged change - `git diff --cached <file>`
*   Undo a commit to a branch - `git reset --soft HEAD~1`
*   View files changed in a commit - `git log --stat`
*   Pull latest changes stashing changes first - `git pull --autostash`
*   Make an empty commit (good for CI) - `git commit --allow-empty -m "Trigger notification"`
*   Change remote repository URL - `git remote set-url origin git://new.location"`
*   fix ".gitignore not working" issue

```bash
Update .gitignore with the folder/file name you want to ignore. You can use anyone of the formats mentioned below (prefer format1)
### Format1  ###
node\_modules/
node/

### Format2  ###
\*\*/frontend/node\_modules/\*\*
\*\*/frontend/node/\*\*

Commit all the changes to git. Exclude the folder/files you dont want commit, in my case node\_modules
Execute the following command to clear the cache

git rm -r --cached .

Execute git status command and it should output node\_modules and sub directories marked for deletion
Now execute

git add .

git commit -m "fixed untracked files" 

git push
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

## Operations

### Checking Ports

```bash
# Show port and PID
netstat -tulpn
# Show process and listening port
ss -ltp
Show ports that are listening
ss -ltn
Show real time TCP and UDP ports
ss -stplu
Show all established connections
lsof -i
Show listening connections
lsof -ni | grep LISTEN
```

### General Linux

```bash
Copy the content of a folder to an existing folder
cp -a /source/. /dest/
Delete everything in a directory
rm /path/to/dir/*
Remove all sub-directories and files
rm -r /path/to/dir/*
Find and replace whole words in vim
:%s/\<word\>\C/newword/g
# Replace all occurrences of string in a directory
Find and replace string - grep -rl "oldstring" ./ | xargs sed -i "" "s/oldstring/newstring/g"
# Listing Running Services Under SystemD in Linux
systemctl list-units --type=service# Listing Running Services Under SystemD in Linux
systemctl list-units --type=service
# Sort disk usage by most first
df -h | tail -n +2 | sort -rk5
# Check the size of a top level dicectory
du -h --max-depth=1 /tmp/
# Top 50 file sizes
du -ah / | sort -n -r | head -n 50
# Show directory sizes (must not be in root directory)
du -sh *
# Check disk usage per directory
du -h <dir> | grep '[0-9\.]\+G’
# Look for growing directories
watch -n 10 df -ah
# Ncurses based disk usage
ncdu -q
# Exlcude directories in find
find /tmp -not \( -path /tmp/dir -prune \) -type p -o -type b
```

### Openssl

```bash
# verify if TLS 1.2 is supported
openssl s_client -connect google.com:443 -tls1_2
# Generate a new private key and Certificate Signing Request
openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout privateKey.key
# Generate a self-signed certificate
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt
# Generate a certificate signing request (CSR) for an existing private key
openssl req -out CSR.csr -key privateKey.key -new
# Generate a certificate signing request based on an existing certificate
openssl x509 -x509toreq -in certificate.crt -out CSR.csr -signkey privateKey.key
# Remove a passphrase from a private key
openssl rsa -in privateKey.pem -out newPrivateKey.pem
# Check a Certificate Signing Request (CSR)
openssl req -text -noout -verify -in CSR.csr
# Check a private key
openssl rsa -in privateKey.key -check
# Check a certificate
openssl x509 -in certificate.crt -text -noout
# Check a PKCS#12 file (.pfx or .p12)
openssl pkcs12 -info -in keyStore.p12
# Convert a DER file (.crt .cer .der) to PEM
openssl x509 -inform der -in certificate.cer -out certificate.pem
# Convert a PEM file to DER
openssl x509 -outform der -in certificate.pem -out certificate.der
# Convert a PKCS#12 file (.pfx .p12) containing a private key and certificates to PEM
openssl pkcs12 -in keyStore.pfx -out keyStore.pem -nodes
# Convert a PEM certificate file and a private key to PKCS#12 (.pfx .p12)
openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile CACert.crt
# Convert a .PEM file to a value that can be passed in a JSON string
awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' your_private_key.pem > output.txt
```

### Networking

```bash
# Check your public IP
curl http://whatismyip.org/
curl ifconfig.me
curl icanhazip.com
# Return the IP of an interface
ifconfig en0 | grep --word-regexp inet | awk '{print $2}'
ip add show eth0 | awk '/inet/ {print $2}' | cut -d/ -f1 | head -1
ip -br a sh eth0 | awk '{ print $3 }' (returns netmask)
ip route show dev eth0 | awk '{print $7}'
hostname -I (return ip only)
Check domain with specific NS
dig <example.com> @<ns-server>
Get NS records for a site
dig <example.com> ns
```

### Cron

[Crontab Guru Examples](https://crontab.guru/examples.html)
