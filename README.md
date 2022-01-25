# Loftwah's Cheatsheet

![LOFTWAH'S](https://user-images.githubusercontent.com/19922556/150899356-b3930a05-6b65-43c4-a492-f5b7e5f94b39.png)

This is a repo with a bunch of stuff I use regularly.

## Links

[AWS ap-southeast-2](https://ap-southeast-2.console.aws.amazon.com/console/home?region=ap-southeast-2) | [Canva](https://canva.com) | [CloudFlare](https://dash.cloudflare.com/) | [Cloudflare Developers](https://developers.cloudflare.com/) | [CyberChef](https://gchq.github.io/CyberChef/)
 | [DevDocs.io](https://devdocs.io/) | [PhotoPea](https://photopea.com) | 
 
### Google

| [Admin Console](https://admin.google.com/ac/home?hl=en) | [Cloud Platform](https://console.cloud.google.com/home/dashboard) | [Drive](https://drive.google.com/drive/u/0/) | [Gmail](https://mail.google.com/) | [Sheets](https://sheets.google.com/) | [How Search Works](https://www.google.com/search/howsearchworks/?fg=1) |

### [Microsoft 365](https://www.office.com/)

| [Admin Console](https://admin.microsoft.com/Adminportal/Home) | [Outlook](https://outlook.office365.com/mail/inbox) | [Teams](https://teams.microsoft.com/) |

## Docker

[Docs](https://docs.docker.com/)

```bash
# To start the docker daemon:
docker -d
# To start a container with an interactive shell:
docker run -ti <image-name> /bin/bash
# To "shell" into a running container (docker-1.3+):
docker exec -ti <container-name> bash
# To inspect a running container:
docker inspect <container-name> (or <container-id>)
# To get the process ID for a container:
docker inspect --format {{.State.Pid}} <container-name-or-id>
# To list (and pretty-print) the current mounted volumes for a container:
docker inspect --format='{{json .Volumes}}' <container-id> | python -mjson.tool
# To copy files/folders between a container and your host:
docker cp foo.txt mycontainer:/foo.txt
# To list currently running containers:
docker ps
# To list all containers:
docker ps -a
# To remove all stopped containers:
docker rm $(docker ps -qa)
# To list all images:
docker images
# To remove all untagged images:
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
# To remove all volumes not used by at least one container:
docker volume prune
```

Migrate image without registry

```bash
#Step1 - Save the Docker image as a tar file
docker save -o <path for generated tar file> <image name>

#Example
docker save -o c:/myfile.tar centos:16

#Step2 - copy your image to a new system with regular file transfer tools such as cp, scp or rsync(preferred for big files)

#Step3 - load the image into Docker
docker load -i <path to image tar file>
```

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

- Shell Script to Install Docker on Ubuntu

```bash
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
```

- Shell Script to Install Docker on Centos

```bash
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
```

- Shell Script to Install Docker on AWS linux

```bash
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
```

**Docker Compose**

- Shell Script to Install the latest version of docker-compose

```bash
#!/bin/bash
# get latest docker compose released tag
COMPOSE\_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag\_name' | cut -d\\" -f4)
sudo curl -L "https://github.com/docker/compose/releases/download/${COMPOSE\_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod a+x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
# Output the  version
docker-compose -v
```

**Dockerfile**

- Dockerizing a simple nodeJs app

```dockerfile
FROM node:4.6
WORKDIR /app
ADD ./app
RUN npm install
EXPOSE 3000
CMD npm start
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

**Git**

- Remove deleted files from repo - `git rm $(git ls-files --deleted)`
- Reset git repo (dangerous) - `git reset --hard HEAD`
- Reset and remove untracked changes in repo - `git clean -xdf`
- Ignore certificates when cloning via HTTPS - `git config --global http.sslVerify false`
- Pull changes and remove stale branches - `git pull --prune`
- Grab the diff of a previous version of a file - `git diff HEAD@{1} ../../production.hosts`
- Grab the diff of a staged change - `git diff --cached <file>`
- Undo a commit to a branch - `git reset --soft HEAD~1`
- View files changed in a commit - `git log --stat`
- Pull latest changes stashing changes first - `git pull --autostash`
- Make an empty commit (good for CI) - `git commit --allow-empty -m "Trigger notification"`
- Change remote repository URL - `git remote set-url origin git://new.location"`
- fix ".gitignore not working" issue

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

## Jenkins

- Setup Jenkins on Amazon Linux 2

```bash
#!/bin/bash
sudo yum update -y
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
sudo yum install java-1.8.0 -y
sudo yum install jenkins -y
sudo service jenkins start
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

## Linux

| [Ubuntu Server Download](https://ubuntu.com/download/server) | [Linux Basics](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-basics) | [Set Up SSH Keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04) | [SSH Shortcut](https://www.digitalocean.com/community/tutorials/how-to-create-an-ssh-shortcut) | [Self Signed Certificate with Custom Root CA](https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309) |

## Nginx

| [Config Tool](https://www.digitalocean.com/community/tools/nginx) | [Nginx Proxy Manager](https://nginxproxymanager.com/setup/) |

- Check installed modules - `nginx -V`
- Pretty print installed modules - `2>&1 nginx -V | xargs -n1`
- Test a configuration without reloading - `nginx -t`
- Stop all nginx processes - `nginx -s stop`
- Start all nginx processes - `nginx -s start`
- Restart all nginx processes - `nginx -s restart`
- Realod nginx configuration (without restarting) - `nginx -s reload`

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

[Crontab Guru Examples](https://crontab.guru/examples.html)

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
du -h <dir> | grep [0-9\.]\+G
# Look for growing directories
watch -n 10 df -ah
# Ncurses based disk usage
ncdu -q
# Exlcude directories in find
find /tmp -not \( -path /tmp/dir -prune \) -type p -o -type b
# Remove files over 30 days old
find . -mtime +30 | xargs rm -rf
# Remove files older than 7 day starting with 'backup'
find . -type f -name "backup*" -mtime +7 -exec rm {} \;
# Generate generic ssh key pair
ssh-keygen -q -t rsa -f ~/.ssh/<name> -N '' -C <name>
# AWS PEM key to ssh PUB key
ssh-keygen -y -f eliarms.pem > eliarms.pub
```

#### Grep

- Look through all files in current dir for word “foo” - `grep -R "foo” .`
- View last ten lines of output - `grep -i -C 10 "invalid view source” /var/log/info.log`
- Display line number of message - `grep -n “pattern” <file>`

#### ps

- Show process tree of all PIDs - `ps auxwf`
- Show all process info and hierarchy (same as above)- `ps -efH`
- Show orphaned processes for - `ps -ef|awk '$3=="1" && /pandora/ { print $2 }'`
- Show all orphaned processes (could be daemons) - `ps -elf | awk '{if ($5 == 1){print $4" "$5" "$15}}'`
- Show zombie processes - `ps aux | grep Z`

#### Networking

[Packetlife Cheatsheets](https://packetlife.net/library/cheat-sheets/)

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
# Check nat rules for ip redirection
iptables -nvL -t nat
```

#### cURL

```bash
# Download a file and specify a new filename.
curl http://example.com/file.zip -o new_file.zip
# Download multiple files.
curl -O URLOfFirstFile -O URLOfSecondFile
# Download all sequentially-numbered files (1-24).
curl http://example.com/pic[1-24].jpg
# Download a file, following [L]ocation redirects, and automatically [C]ontinuing (resuming) a previous file transfer:
curl -O -L -C - http://example.com/filename
# Download a file and follow redirects.
curl -L http://example.com/file
# Download a file and pass HTTP Authentication.
curl -u username:password URL
# Download a file with a Proxy.
curl -x proxysever.server.com:PORT http://addressiwantto.access
# Download a file from FTP.
curl -u username:password -O ftp://example.com/pub/file.zip
# Resume a previously failed download.
curl -C - -o partial_file.zip http://example.com/file.zip
# Fetch only the HTTP headers from a response.
curl -I http://example.com
# Fetch your external IP and network info as JSON.
curl http://ifconfig.me/all/json
# Limit the rate of a download.
curl --limit-rate 1000B -O http://path.to.the/file
# POST to a form.
curl -F "name=user" -F "password=test" http://example.com
# POST JSON Data.
curl -H "Content-Type: application/json" -X POST -d '{"user":"bob","pass":"123"}' http://example.com
# Send a request with an extra header, using a custom HTTP method:
curl -H 'X-My-Header: 123' -X PUT http://example.com
# Pass a user name and password for server authentication:
curl -u myusername:mypassword http://example.com
# Pass client certificate and key for a resource, skipping certificate validation:
curl --cert client.pem --key key.pem --insecure https://example.com
```

##### Nmap

```bash
# Check single port on single host
nmap -p <port> <host/IP>
# Intrusive port scan on a single host
nmap -sS <host/IP>
# Top ten port on a single host
nmap --top-ports 10 <host/IP>
# Scan from a list of targets:
nmap -iL [list.txt]
# OS detection:
nmap -O --osscan_guess [target]
# Save output to text file:
nmap -oN [output.txt] [target]
# Save output to xml file:
nmap -oX [output.xml] [target]
# Scan a specific port:
nmap -p [port] [target]
# Do an aggressive scan:
nmap -A [target]
# Speedup your scan: # -n => disable ReverseDNS # --min-rate=X => min X packets / sec
nmap -T5 --min-parallelism=50 -n --min-rate=300 [target]
# Traceroute:
nmap -traceroute [target]
# Example: Ping scan all machines on a class C network
nmap -sP 192.168.0.0/24
# Force TCP scan: -sT # Force UDP scan: -sU
# Discover DHCP information on an interface
nmap --script broadcast-dhcp-discover -e eth0
## Port Status Information
- Open: This indicates that an application is listening for connections on this port.
- Closed: This indicates that the probes were received but there is no application listening on this port.
- Filtered: This indicates that the probes were not received and the state could not be established. It also indicates that the probes are being dropped by some kind of filtering.
- Unfiltered: This indicates that the probes were received but a state could not be established.
- Open/Filtered: This indicates that the port was filtered or open but Nmap couldn’t establish the state.
- Closed/Filtered: This indicates that the port was filtered or closed but Nmap couldn’t establish the state.
## Additional Scan Types
nmap -sn: Probe only (host discovery, not port scan)
nmap -sS: SYN Scan
nmap -sT: TCP Connect Scan
nmap -sU: UDP Scan
nmap -sV: Version Scan
nmap -O: Used for OS Detection/fingerprinting
nmap --scanflags: Sets custom list of TCP using `URG ACK PSH RST SYN FIN` in any order
### Nmap Scripting Engine Categories
The most common Nmap scripting engine categories:
- auth: Utilize credentials or bypass authentication on target hosts.
- broadcast: Discover hosts not included on command line by broadcasting on local network.
- brute: Attempt to guess passwords on target systems, for a variety of protocols, including http, SNMP, IAX, MySQL, VNC, etc.
- default: Scripts run automatically when -sC or -A are used.
- discovery: Try to learn more information about target hosts through public sources of information, SNMP, directory services, and more.
- dos: May cause denial of service conditions in target hosts.
- exploit: Attempt to exploit target systems.
- external: Interact with third-party systems not included in target list.
- fuzzer: Send unexpected input in network protocol fields.
- intrusive: May crash target, consume excessive resources, or otherwise impact target machines in a malicious fashion.
- malware: Look for signs of malware infection on the target hosts.
- safe: Designed not to impact target in a negative fashion.
- version: Measure the version of software or protocols on the target hosts.
- vul: Measure whether target systems have a known vulnerability.
```

#### Password generation

- Create hash from password - `openssl passwd -crypt <password>`
- Generate random 8 character password (Ubuntu) - `makepasswd -count 1 -minchars 8`
- Create .passwd file with user and random password - `sudo htpasswd -c /etc/nginx/.htpasswd <user>`

#### Openssl

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

#### Tail log with colored output

- `grc tail -f /var/log/filename`

## Searching

[GitHub repositories with more than 2500 stars sorted by recently updated](https://github.com/search?o=desc&q=stars%3A%3E2500&s=updated&type=Repositories)
### Test a WebSocket using cURL

```bash
curl --include \
     --no-buffer \
     --header "Connection: Upgrade" \
     --header "Upgrade: websocket" \
     --header "Host: example.com:80" \
     --header "Origin: http://example.com:80" \
     --header "Sec-WebSocket-Key: SGVsbG8sIHdvcmxkIQ==" \
     --header "Sec-WebSocket-Version: 13" \
     http://example.com:80/
```

### Minimal safe Bash script template

```bash
#!/usr/bin/env bash

set -Eeuo pipefail
trap cleanup SIGINT SIGTERM ERR EXIT

script_dir=$(cd "$(dirname "${BASH_SOURCE[0]}")" &>/dev/null && pwd -P)

usage() {
  cat <<EOF
Usage: $(basename "${BASH_SOURCE[0]}") [-h] [-v] [-f] -p param_value arg1 [arg2...]
Script description here.
Available options:
-h, --help      Print this help and exit
-v, --verbose   Print script debug info
-f, --flag      Some flag description
-p, --param     Some param description
EOF
  exit
}

cleanup() {
  trap - SIGINT SIGTERM ERR EXIT
  # script cleanup here
}

setup_colors() {
  if [[ -t 2 ]] && [[ -z "${NO_COLOR-}" ]] && [[ "${TERM-}" != "dumb" ]]; then
    NOFORMAT='\033[0m' RED='\033[0;31m' GREEN='\033[0;32m' ORANGE='\033[0;33m' BLUE='\033[0;34m' PURPLE='\033[0;35m' CYAN='\033[0;36m' YELLOW='\033[1;33m'
  else
    NOFORMAT='' RED='' GREEN='' ORANGE='' BLUE='' PURPLE='' CYAN='' YELLOW=''
  fi
}

msg() {
  echo >&2 -e "${1-}"
}

die() {
  local msg=$1
  local code=${2-1} # default exit status 1
  msg "$msg"
  exit "$code"
}

parse_params() {
  # default values of variables set from params
  flag=0
  param=''

  while :; do
    case "${1-}" in
    -h | --help) usage ;;
    -v | --verbose) set -x ;;
    --no-color) NO_COLOR=1 ;;
    -f | --flag) flag=1 ;; # example flag
    -p | --param) # example named parameter
      param="${2-}"
      shift
      ;;
    -?*) die "Unknown option: $1" ;;
    *) break ;;
    esac
    shift
  done

  args=("$@")

  # check required params and arguments
  [[ -z "${param-}" ]] && die "Missing required parameter: param"
  [[ ${#args[@]} -eq 0 ]] && die "Missing script arguments"

  return 0
}

parse_params "$@"
setup_colors

# script logic here

msg "${RED}Read parameters:${NOFORMAT}"
msg "- flag: ${flag}"
msg "- param: ${param}"
msg "- arguments: ${args[*]-}"
```

### Docker-Compose in GitHub Action

```yaml
name: Test

on:
  push:
    branches:
    - main
    - features/**
    - dependabot/**
  pull_request:
    branches:
    - main

jobs:
  docker:
    timeout-minutes: 10
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v1

    - name: Start containers
      run: docker-compose -f "docker-compose.yml" up -d --build

    - name: Install node
      uses: actions/setup-node@v1
      with:
        node-version: 14.x

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm run test

    - name: Stop containers
      if: always()
      run: docker-compose -f "docker-compose.yml" down
```

### Stack data structure in Bash

```bash
class Stack {
    constructor() {
        this.items = []
        this.count = 0
    }

    // Add element to top of stack
    push(element) {
        this.items[this.count] = element
        console.log(`${element} added to ${this.count}`)
        this.count += 1
        return this.count - 1
    }

    // Return and remove top element in stack
    // Return undefined if stack is empty
    pop() {
        if(this.count == 0) return undefined
        let deleteItem = this.items[this.count - 1]
        this.count -= 1
        console.log(`${deleteItem} removed`)
        return deleteItem
    }

    // Check top element in stack
    peek() {
        console.log(`Top element is ${this.items[this.count - 1]}`)
        return this.items[this.count - 1]
    }

    // Check if stack is empty
    isEmpty() {
        console.log(this.count == 0 ? 'Stack is empty' : 'Stack is NOT empty')
        return this.count == 0
    }

    // Check size of stack
    size() {
        console.log(`${this.count} elements in stack`)
        return this.count
    }

    // Print elements in stack
    print() {
        let str = ''
        for(let i = 0; i < this.count; i++) {
            str += this.items[i] + ' '
        }
        return str
    }

    // Clear stack
    clear() {
        this.items = []
        this.count = 0
        console.log('Stack cleared..')
        return this.items
    }
}

const stack = new Stack()

stack.isEmpty()

stack.push(100)
stack.push(200)

stack.peek()

stack.push(300)

console.log(stack.print())

stack.pop()
stack.pop()

stack.clear()

console.log(stack.print())

stack.size()

stack.isEmpty()
```

### HTTPS Request with Node.js and TypeScript

```bash
import https, { RequestOptions } from 'https';

export interface HttpRequest extends RequestOptions {
  url: string;
  body?: string;
  method: 'GET' | 'POST' | 'PUT' | 'DELETE' | 'PATCH' | 'OPTIONS';
}

interface Response<T = Buffer | any> {
  body: T;
  statusCode: number;
}

export default async function httpRequest<T = Buffer | any>(
  options: HttpRequest,
): Promise<Response<T>> {
  const { url, body, ...rest } = options;

  if (!rest.method) {
    rest.method = 'GET';
  }

  const response = await new Promise<Response>((resolve, reject) => {
    const chunks: any[] = [];

    const request = https.request(url, rest, httpResponse => {
      httpResponse.on('error', reject);
      httpResponse.on('data', chunk => chunks.push(chunk));

      httpResponse.on('end', () => {
        let buffer = Buffer.concat(chunks);

        try {
          buffer = JSON.parse(buffer.toString());
        } catch {
          //
        }

        resolve({
          body: buffer,
          statusCode: httpResponse.statusCode ?? 500,
        });
      });
    });

    request.on('error', reject);

    if (['POST', 'PUT', 'PATCH'].includes(rest.method) && body) {
      request.write(body);
    }

    request.end();
  });

  if (response.statusCode >= 400) {
    throw new Error(`Unable to make request in ([${rest.method}] ${url}).`);
  }

  return response;
}
```

### Python function to calculate aspect ratio of an image

```python
def calculate_aspect(width: int, height: int) -> str:
    def gcd(a, b):
        """The GCD (greatest common divisor) is the highest number that evenly divides both width and height."""
        return a if b == 0 else gcd(b, a % b)

    r = gcd(width, height)
    x = int(width / r)
    y = int(height / r)

    return f"{x}:{y}"
    
calculate_aspect(1920, 1080) # '16:9'
```

### HTML Simple Maintenance Page

```html
<!doctype html>
<title>Site Maintenance</title>
<style>
  body { text-align: center; padding: 150px; }
  h1 { font-size: 50px; }
  body { font: 20px Helvetica, sans-serif; color: #333; }
  article { display: block; text-align: left; width: 650px; margin: 0 auto; }
  a { color: #dc8100; text-decoration: none; }
  a:hover { color: #333; text-decoration: none; }
</style>

<article>
    <h1>We&rsquo;ll be back soon!</h1>
    <div>
        <p>Sorry for the inconvenience but we&rsquo;re performing some maintenance at the moment. If you need to you can always <a href="mailto:#">contact us</a>, otherwise we&rsquo;ll be back online shortly!</p>
        <p>&mdash; The Team</p>
    </div>
</article>
```

### Download the latest release from GitHub

```bash
curl -s https://api.github.com/repos/jgm/pandoc/releases/latest \
| grep "browser_download_url.*deb" \
| cut -d : -f 2,3 \
| tr -d \" \
| wget -qi -
```

### SMTP Settings for common providers

#### Microsoft 365

To send emails using Office365 server enter these details:

```bash
SMTP Host: smtp.office365.com
SMTP Port: 587
SSL Protocol: OFF
TLS Protocol: ON
SMTP Username: (your Office365 username)
SMTP Password: (your Office365 password)

POP3 Host: outlook.office365.com
POP3 Port: 995
TLS Protocol: ON
POP3 Username: (your Office365 username)
POP3 Password: (your Office365 password)

IMAP Host: outlook.office365.com
IMAP Port: 993
Encryption: SSL
IMAP Username: (your Office365 username)
IMAP Password: (your Office365 password)
```

#### Amazon SES

Just go to the [docs](https://docs.aws.amazon.com/ses/index.html).

#### Google GSuite | Workspace (why did they rename this? lol)

```bash
# IMAP
imap.gmail.com
Requires SSL: Yes
Port: 993

# SMTP
smtp.gmail.com
Requires SSL: Yes
Requires TLS: Yes (if available)
Requires Authentication: Yes (I got this off the website, I use it without auth all the time. You can set it up in Gmail, or in Google Admin Console)
Port for SSL: 465
Port for TLS/STARTTLS: 587
```

### Awesome (Topic)

| [Azure Policy](https://github.com/globalbao/awesome-azure-policy) | [Data Science](https://github.com/academic/awesome-datascience) | [GitHub Actions](https://github.com/sdras/awesome-actions) | [Golang](https://github.com/avelino/awesome-go) | [GraphQL](https://github.com/chentsulin/awesome-graphql) | [JavaScript Mini Projects](https://github.com/thinkswell/javascript-mini-projects) | [Leading and managing](https://github.com/LappleApple/awesome-leading-and-managing) | [Naming](https://github.com/gruhn/awesome-naming) | [No login web apps](https://github.com/aviaryan/awesome-no-login-web-apps) | [Nodejs Security](https://github.com/lirantal/awesome-nodejs-security) | [Notebooks](https://github.com/jupyter-naas/awesome-notebooks) | [OSINT](https://github.com/jivoi/awesome-osint) | [Pentest](https://github.com/enaqx/awesome-pentest) | [Privacy](https://github.com/pluja/awesome-privacy) | [Prometheus Alerts](https://github.com/samber/awesome-prometheus-alerts) | [Python](https://github.com/vinta/awesome-python) | [Python Scripts](https://github.com/prathimacode-hub/Awesome_Python_Scripts) | [Rust](https://github.com/rust-unofficial/awesome-rust) | [SaaS Boilerplates](https://github.com/smirnov-am/awesome-saas-boilerplates) | [Self Hosted](https://github.com/awesome-selfhosted/awesome-selfhosted) | [Software Architecture](https://github.com/mehdihadeli/awesome-software-architecture) | [Tech Blogs](https://github.com/markodenic/awesome-tech-blogs) | [Technical Writing](https://github.com/BolajiAyodeji/awesome-technical-writing) |
