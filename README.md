# Loftwah's Cheatsheet

![LOFTWAH'S](https://user-images.githubusercontent.com/19922556/150899356-b3930a05-6b65-43c4-a492-f5b7e5f94b39.png)

This is a repo with a bunch of stuff I use regularly. If you would like to know a little bit more about me please visit my [website](https://lofts.sh).

## Links

The links section here is for my own use. If you would like to customize it please make your own fork and update this section as your own.

| [AWS ap-southeast-2](https://ap-southeast-2.console.aws.amazon.com/console/home?region=ap-southeast-2) | [AWS Guide](https://github.com/open-guides/og-aws) | [Canva](https://canva.com) | [CloudFlare](https://dash.cloudflare.com/) | [CyberChef](https://gchq.github.io/CyberChef/)
|

### Google

I use Google Workspace for a lot of my work.

| [Admin Console](https://admin.google.com/ac/home?hl=en) | [Cloud Platform](https://console.cloud.google.com/home/dashboard) | [Drive](https://drive.google.com/drive/u/0/) | [Gmail](https://mail.google.com/) | [Sheets](https://sheets.google.com/) | [How Search Works](https://www.google.com/search/howsearchworks/?fg=1) |

## Docker

[Docs](https://docs.docker.com/)

Docker is a containerization platform that provides a simple way to build, deploy, and manage software containers. Docker containers are isolated from each other and from the host operating system.

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

### How to migrate a Docker image without registry

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
sudo usermod -aG docker ec2-user
```

### Install Docker on Linux

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
sudo service docker start
sudo systemctl enable docker
sudo usermod -aG docker <username>
```

A reboot was required for this to work on Fedora.

### Install Docker-Compose

Note: use `bash` for this one, with `zsh` I get a parse error and I don't know why yet.

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
sudo usermod -aG docker $USER
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
sudo usermod -aG docker $(whoami)
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
#sudo usermod -aG docker ec2-user
sudo usermod -aG docker $(whoami)
# Output current version
 docker -v
```

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

There is now a plugin version available for `Docker Compose`, which adds aome ambiguity because it removes the `-` from the command.

For the `apt` package manager installing the plugin looks something like this.

```bash
sudo apt-get install docker-compose-plugin
```

### Dockerfile

- Dockerizing a simple NodeJS app

```dockerfile
FROM node:latest
WORKDIR /app
ADD ./app
RUN npm install
EXPOSE 3000
CMD npm start
```

## Git and GitHub

Git is a version control system for tracking changes in files and coordinating work among multiple people. GitHub is a web-based Git repository hosting service.

### Initial Git configuration

```bash
git config --global user.name "Dean Lofts"
git config --global user.email "dean@deanlofts.xyz"
```

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

### Git on Oh My Zsh

```bash
g – git
gst – git status
gl – git pull
gup – git pull --rebase
gp – git push
gd – git diff
gdc – git diff --cached
gdv – git diff -w "$@" | view -
gc – git commit -v
gc! – git commit -v --amend
gca – git commit -v -a
gca! – git commit -v -a --amend
gcmsg – git commit -m
gco – git checkout
gcm – git checkout master
gr – git remote
grv – git remote -v
grmv – git remote rename
grrm – git remote remove
gsetr – git remote set-url
grup – git remote update
grbi – git rebase -i
grbc – git rebase --continue
grba – git rebase --abort
gb – git branch
gba – git branch -a
gcount – git shortlog -sn
gcl – git config --list
gcp – git cherry-pick
glg – git log --stat --max-count=10
glgg – git log --graph --max-count=10
glgga – git log --graph --decorate --all
glo – git log --oneline --decorate --color
glog – git log --oneline --decorate --color --graph
gss – git status -s
ga – git add
gm – git merge
grh – git reset HEAD
grhh – git reset HEAD --hard
gclean – git reset --hard && git clean -dfx
gwc – git whatchanged -p --abbrev-commit --pretty=medium
gsts – git stash show --text
gsta – git stash
gstp – git stash pop
gstd – git stash drop
ggpull – git pull origin $(current_branch)
ggpur – git pull --rebase origin $(current_branch)
ggpush – git push origin $(current_branch)
ggpnp – git pull origin $(current_branch) && git push origin $(current_branch)
glp – _git_log_prettily
```

#### Generate SSH Keys

Generate an SSH key with the following:

```bash
ssh-keygen -t rsa -b 4096 -C "dean@deanlofts.xyz"
cat ~/.ssh/id_rsa.pub
```

Put your SSH key into GitHub if you intend to use it this way.

#### Generate and auto-sign your commits with GPG

You can automatically sign your commits with a `gpg` key giving you a `verified` badge on your GitHub commits. Make sure you don't set a passphrase for your `gpg` key. I couldn't work out how to get `git` to work well with a passphrase on the `gpg` key.

```bash
# Generate a key
gpg --gen-key
# See your gpg public key:
gpg --armor --export YOUR_KEY_ID
# YOUR_KEY_ID is the hash in front of `sec` in previous command. (for example sec 4096R/234FAA343232333 => key id is: 234FAA343232333)
# Set a gpg key for git:
git config --global user.signingkey your_key_id
# To sign a single commit:
git commit -S -a -m "Test a signed commit"
# Auto-sign all commits globaly
git config --global commit.gpgsign true
```

Be sure to actually add your key to your `GitHub` account, I always forget this part because it involves actually opening my browser and I haven't figured out a better way to do it yet.

## Kubernetes

Kubernetes is a containerization platform that allows you to run applications on a cluster of machines

### Kompose

Install Kompose with the following

```bash
curl -L https://github.com/kubernetes/kompose/releases/download/v1.26.1/kompose-linux-amd64 -o kompose
```

### ArgoCD

How to install ArgoCD

### Install by script

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl port-forward svc/argocd-server -n argocd 8080:443
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

### Install by helm

- [general parameters](https://github.com/argoproj/argo-helm/tree/master/charts/argo-cd#general-parameters)

```bash
helm repo add argocd https://argoproj.github.io/argo-cd
helm install my-release argo/argo-cd
```

## Linux

| [Ubuntu Server Download](https://ubuntu.com/download/server) | [Linux Basics](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-basics) | [Set Up SSH Keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04) | [SSH Shortcut](https://www.digitalocean.com/community/tutorials/how-to-create-an-ssh-shortcut) | [Self Signed Certificate with Custom Root CA](https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309) |

I like to install a few appliocations on top of Linux with my own custom configuration.

### Linuxbrew

Install Linuxbrew by following these instructions:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### zsh

Install zsh by following these instructions:

```bash
sudo apt install zsh -y
sudo chsh -s $(which zsh)
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sudo apt install fonts-powerline -y
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

You need to update the `~/.zshrc` file to add the plugins. If you're using a file stored somewhere else create a link to the file in your home directory with `ln -s /dotfiles/.zshrc ~/.zshrc`. This will create a symlink to the file so you can store it in a `Git` repository.

```bash
plugins=(
git
zsh-autosuggestions
zsh-syntax-highlighting
)
```

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

## AWS S3

```bash
# List s3 bucket permissions and keys
aws s3api get-bucket-acl --bucket examples3bucketname
aws s3api get-object-acl --bucket examples3bucketname --key dir/file.ext
aws s3api list-objects --bucket examples3bucketname
aws s3api list-objects-v2 --bucket examples3bucketname
aws s3api get-object --bucket examples3bucketname --key dir/file.ext localfilename.ext
aws s3api put-object --bucket examples3bucketname --key dir/file.ext --body localfilename.ext
```

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
<!DOCTYPE html>
<title>Site Maintenance</title>
<style>
  body {
    text-align: center;
    padding: 150px;
  }
  h1 {
    font-size: 50px;
  }
  body {
    font: 20px Helvetica, sans-serif;
    color: #333;
  }
  article {
    display: block;
    text-align: left;
    width: 650px;
    margin: 0 auto;
  }
  a {
    color: #dc8100;
    text-decoration: none;
  }
  a:hover {
    color: #333;
    text-decoration: none;
  }
</style>

<article>
  <h1>We&rsquo;ll be back soon!</h1>
  <div>
    <p>
      Sorry for the inconvenience but we&rsquo;re performing some maintenance at
      the moment. If you need to you can always
      <a href="mailto:#">contact us</a>, otherwise we&rsquo;ll be back online
      shortly!
    </p>
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

### Upload URL to Google Drive

```javascript
function uploadFiles(url) {
  var response =  UrlFetchApp.fetch(url)
  var fileName  = getFilenameFromURL(url)
  var folder = DriveApp.getFolderById('1IxMiswEfi67ovoBf8ZH1RV7qVPx1Ks6l');
  var blob = response.getBlob();
  var file = folder.createFile(blob)
  file.setName(fileName)
  file.setDescription("Download from the " + url)
  return file.getUrl();
}

function getFilenameFromURL(url) {
  //(host-ish)/(path-ish/)(filename)
  var re = /^https?:\/\/([^\/]+)\/([^?]*\/)?([^\/?]+)/;
  var match = re.exec(url);
  if (match) {
    return unescape(match[3]);
  }
  return null;
}

function doGet(e){
var html  =  HtmlService.createHtmlOutputFromFile('index.html')
return html.setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)
}

/*create index.html on the Google app project and put the below code and remove this comment*/

<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <title>Upload Files</title>
  </head>
  <body>
    <h1>Upload files to Google drive from URL</h1>
    <form>
      <label>Enter the URL</label>
      <input type="text" name="myFile" id="url" style="height:5%; width:70%">
      <br>
      <br>
      <input type="button" id="submitBtn" value="Upload Files">
      <label id="resp"><label>
    </form>
    <script>
      document.getElementById('submitBtn').addEventListener('click',
      function(e){
        var url= document.getElementById("url").value;
        google.script.run.withSuccessHandler(onSuccess).uploadFiles(url)
      
      })
      
      function onSuccess(url){
        document.getElementById('resp').innerHTML = "File uploaded to path" + url;
      }
    </script>
  </body>
</html>
```

### HTTPS certificate for localhost

This focuses on generating the certificates for loading local virtual hosts hosted on your computer, for development only.

### Create a Certificate authority (CA)

Generate `RootCA.pem`, `RootCA.key` & `RootCA.crt`:

```bash
openssl req -x509 -nodes -new -sha256 -days 1024 -newkey rsa:2048 -keyout RootCA.key -out RootCA.pem -subj "/C=US/CN=Example-Root-CA"
openssl x509 -outform pem -in RootCA.pem -out RootCA.crt
```

- Note that `Example-Root-CA` is an example, you can customize the name.

### Domain name certificate

Let's say you have two domains `loftwah1.local` and `loftwah2.local` that are hosted on your local machine
for development (using the `hosts` file to point them to `127.0.0.1`).

First, create a file `domains.ext` that lists all your local domains:

```bash
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1 = localhost
DNS.2 = loftwah1.local
DNS.3 = loftwah2.local
```

Generate `localhost.key`, `localhost.csr`, and `localhost.crt`:

```bash
openssl req -new -nodes -newkey rsa:2048 -keyout localhost.key -out localhost.csr -subj "/C=US/ST=YourState/L=YourCity/O=Example-Certificates/CN=localhost.local"
openssl x509 -req -sha256 -days 1024 -in localhost.csr -CA RootCA.pem -CAkey RootCA.key -CAcreateserial -extfile domains.ext -out localhost.crt
```

Note that the country / state / city / name in the first command  can be customized.

You will need to configure it with your webserver. Click [here](http://nginx.org/en/docs/http/configuring_https_servers.html) for instructions on configuring Nginx.

### Trust the local CA

At this point, the site would load with a warning about self-signed certificates.
In order to get a green lock, your new local CA has to be added to the trusted Root Certificate Authorities.

#### Windows: Chrome, & Edge

- Windows recognizes `.crt` files, so you can right-click on `RootCA.crt` > `Install` to open the import dialog.
- Make sure to select "Trusted Root Certification Authorities" and confirm.
- You should now get a green lock in Chrome, and Edge.

#### Windows: Firefox

- There are two ways to get the CA trusted in Firefox.
- The simplest is to make Firefox use the Windows trusted Root CAs by going to `about:config`,
and setting `security.enterprise_roots.enabled` to `true`.
- The other way is to import the certificate by going
to `about:preferences#privacy` > `Certificats` > `Import` > `RootCA.pem` > `Confirm for websites`.

### HTTPS with Let's Encrypt for Nginx

This is how to setup Let's Encrypt for Nginx.

#### Virtual hosts

Let's say you want to host domains `first.com` and `second.com`.

Create folders for their files:

```bash
mkdir /var/www/first
mkdir /var/www/second
```

Create a text file `/etc/nginx/sites-available/first.conf` containing:

```conf
server {
  listen 80 default_server;
  listen [::]:80 default_server ipv6only=on;

  server_name first.com www.first.com;
  root /var/www/first;

  index index.html;
  location / {
    try_files $uri $uri/ =404;
  }
}
```

Create a text file `/etc/nginx/sites-available/second.conf` containing:

```conf
server {
  listen 80;
  listen [::]:80;

  server_name second.com www.second.com;
  root /var/www/second;

  index index.html;
  location / {
    try_files $uri $uri/ =404;
  }
}
```

Note that **only the first domain** has the keywords `default_server` and `ipv6only=on` in the `listen` lines.

Replace the default virtual host:

```bash
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/first.conf /etc/nginx/sites-enabled/first.conf
sudo ln -s /etc/nginx/sites-available/second.conf /etc/nginx/sites-enabled/second.conf
sudo nginx -t
sudo systemctl stop nginx
sudo systemctl start nginx
```

Check that Nginx is running:

```bash
sudo systemctl status nginx
```

Expected results at this stage:

- `http://first.com` and `http://www.first.com` serve the files from `/var/www/first`
- `http://second.com` and `http://www.second.com` serve the files from `/var/www/second`
- `https://www.first.com` and `https://www.second.com` don't work yet

#### Certbot

Install Certbot for Nginx:

```bash
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot python-certbot-nginx
```

Setup the certificates & convert Virtual Hosts to HTTPS:

```bash
sudo certbot --nginx
```

It will ask for:

- an email address
- agreeing to its Terms of Service
- which domains to use HTTPS for (it detects the list using `server_name` lines in your Nginx config)
- whether to redirect HTTP to HTTPS (recommended) or not

**You could stop here if all you want is HTTPS** as this already gives you an `A` rating and maintains itself.

Test your site with SSL Labs using `https://www.ssllabs.com/ssltest/analyze.html?d=www.YOUR-DOMAIN.com`

Expected results at this stage:

- `http://first.com` redirects to `https://first.com`
- `http://second.com` redirects to `https://second.com`
- `http://www.first.com` redirects to `https://www.first.com`
- `http://www.second.com` redirects to `https://www.second.com`
- `https://first.com` and `https://www.first.com` serve the files from `/var/www/first`
- `https://second.com` and `https://www.first.com`serve the files from `/var/www/second`

#### Automatic renewal

**There is nothing to do**, Certbot installed a cron task to automatically renew certificates about to expire.

You can [check renewal works](https://certbot.eff.org/docs/using.html#re-creating-and-updating-existing-certificates) using:

```bash
sudo certbot renew --dry-run
```

You can also [check what certificates exist](https://certbot.eff.org/docs/using.html#managing-certificates) using:

```bash
sudo certbot certificates
```

#### HTTP/2

`first.conf` should now look something like this, now that Certbot edited it:

```conf
server {
  server_name first.com www.first.com;
  root /var/www/first.com;

  index index.html;
  location / {
    try_files $uri $uri/ =404;
  }

  listen [::]:443 ssl ipv6only=on; # managed by Certbot
  listen 443 ssl; # managed by Certbot
  ssl_certificate /etc/letsencrypt/live/first.com/fullchain.pem; # managed by Certbot
  ssl_certificate_key /etc/letsencrypt/live/first.com/privkey.pem; # managed by Certbot
  include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
  if ($host = www.first.com) {
    return 301 https://$host$request_uri;
  } # managed by Certbot

  if ($host = first.com) {
    return 301 https://$host$request_uri;
  } # managed by Certbot

  listen 80 default_server;
  listen [::]:80 default_server;

  server_name first.com www.first.com;
  return 404; # managed by Certbot
}
```

Certbot didn't add HTTP/2 support when it created the new server blocks, so replace these lines:

```conf
listen [::]:443 ssl ipv6only=on;
listen 443 ssl;
```

with this:

```conf
listen [::]:443 ssl http2 ipv6only=on;
listen 443 ssl http2;
gzip off;
```

There is [already an open Github issue](https://github.com/certbot/certbot/issues/3646)
requesting Certbot to add `http2` automatically, so hopefully this step can soon be removed.

#### Stronger settings for A+

Follow these steps to get a stronger A+ rating:

##### Trusted certificate

The HTTPS `server` blocks in `first.conf` and `second.conf` contain these lines, added by Certbot:

```bash
ssl_certificate /etc/letsencrypt/live/YOUR-DOMAIN/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/YOUR-DOMAIN/privkey.pem;
```

The stronger settings use **OCSP Stapling**, so each virtual host will need a `ssl_trusted_certificate` as well.

**Add this line** (using the folder name that Certbot generated for your domain) after the `ssl_certificate_key` line:

```bash
ssl_trusted_certificate /etc/letsencrypt/live/YOUR-DOMAIN/chain.pem;
```

#### SSL

Now let's **edit the shared SSL settings** at `/etc/letsencrypt/options-ssl-nginx.conf`.

It most likely looks like this initially:

```conf
ssl_session_cache shared:le_nginx_SSL:1m;
ssl_session_timeout 1440m;

ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS";
```

If you tested with SSL Labs, you probably noticed that quite a few ciphers were flagged as "weak".

So **replace the contents of the file** with:

```conf
ssl_session_cache shared:le_nginx_SSL:1m;
ssl_session_timeout 1d;
ssl_session_tickets off;

ssl_protocols TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
ssl_ecdh_curve secp384r1;

ssl_stapling on;
ssl_stapling_verify on;

add_header Strict-Transport-Security "max-age=15768000; includeSubdomains; preload;";
add_header Content-Security-Policy "default-src 'none'; frame-ancestors 'none'; script-src 'self'; img-src 'self'; style-src 'self'; base-uri 'self'; form-action 'self';";
add_header Referrer-Policy "no-referrer, strict-origin-when-cross-origin";
add_header X-Frame-Options SAMEORIGIN;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
```

Now **restart Nginx**, and test the domain again with SSL Labs using `https://www.ssllabs.com/ssltest/analyze.html?d=www.YOUR-DOMAIN.com&latest`:

it should now be rated `A+`.

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

### Environmental Variables

```bash
# AWS
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AMAZON_AWS_ACCESS_KEY_ID
AMAZON_AWS_SECRET_ACCESS_KEY
# Azure
AZURE_CLIENT_ID
AZURE_CLIENT_SECRET
AZURE_USERNAME
AZURE_PASSWORD
MSI_ENDPOINT
MSI_SECRET
# DigitalOcean
DIGITALOCEAN_ACCESS_TOKEN
# Dockerhub
DOCKERHUB_PASSWORD
# Facebook
FACEBOOK_APP_ID
FACEBOOK_APP_SECRET
FACEBOOK_ACCESS_TOKEN
# GitHub
GH_TOKEN
GITHUB_TOKEN
GH_ENTERPRISE_TOKEN
GITHUB_ENTERPRISE_TOKEN
# Google Cloud
GOOGLE_APPLICATION_CREDENTIALS
GOOGLE_API_KEY
# Mailgun
MAILGUN_API_KEY
# MongoDB
MCLI_PRIVATE_API_KEY
MCLI_PUBLIC_API_KEY
# NPM
NPM_TOKEN
# Slack
SLACK_TOKEN
# Square
SQUARE_ACCESS_TOKEN
SQUARE_OAUTH_TOKEN
# Stripe
STRIPE_API_KEY
STRIPE_DEVICE_NAME
# Twilio
TWILIO_ACCOUNT_SID
TWILIO_AUTH_TOKEN
# Twitter
CONSUMER_KEY
CONSUMER_SECRET
```

### Awesome (Topic)

| [A11Y](https://github.com/brunopulis/awesome-a11y) | [Agile](https://github.com/lorabv/awesome-agile) | [Authentication](https://github.com/casbin/awesome-auth) | [Automation Scripts](https://github.com/python-geeks/Automation-scripts) | [Argo](https://github.com/terrytangyuan/awesome-argo) | [AWS](https://github.com/donnemartin/awesome-aws) | [Azure Policy](https://github.com/globalbao/awesome-azure-policy) | [Bash](https://github.com/awesome-lists/awesome-bash) | [Books](https://github.com/hackerkid/Mind-Expanding-Books) | [Business Intelligence](https://github.com/thenaturalist/awesome-business-intelligence) | [Chaos Engineering](https://github.com/dastergon/awesome-chaos-engineering) |[Cheatsheets](https://github.com/LeCoupa/awesome-cheatsheets) | [CI](https://github.com/ligurio/awesome-ci) | [Cloud Native](https://github.com/rootsongjc/awesome-cloud-native) | [Cloud Security](https://github.com/4ndersonLin/awesome-cloud-security) | [CTO](https://github.com/kuchin/awesome-cto) | [Data Science](https://github.com/academic/awesome-datascience) | [Dataset Tools](https://github.com/jsbroks/awesome-dataset-tools) | [DevOps](https://github.com/wmariuss/awesome-devops) | [DevSecOps](https://github.com/sottlmarek/DevSecOps) | [Discord Communities](https://github.com/mhxion/awesome-discord-communities) | [Docker](https://github.com/veggiemonk/awesome-docker) | [Docker Compose](https://github.com/docker/awesome-compose) | [eBPF](https://github.com/zoidbergwill/awesome-ebpf) | [GitHub Actions](https://github.com/sdras/awesome-actions) | [Golang](https://github.com/avelino/awesome-go) | [GraphQL](https://github.com/chentsulin/awesome-graphql) | [gRPC](https://github.com/grpc-ecosystem/awesome-grpc) | [Guidelines](https://github.com/Kristories/awesome-guidelines) | [Home Kubernetes](https://github.com/k8s-at-home/awesome-home-kubernetes) | [JavaScript Mini Projects](https://github.com/thinkswell/javascript-mini-projects) | [json](https://github.com/burningtree/awesome-json) | [k8s Resources](https://github.com/tomhuang12/awesome-k8s-resources) | [Kubernetes](https://github.com/ramitsurana/awesome-kubernetes) | [Linux Containers](https://github.com/Friz-zy/awesome-linux-containers) | [Linux Software](https://github.com/luong-komorebi/Awesome-Linux-Software) | [Leading and managing](https://github.com/LappleApple/awesome-leading-and-managing) | [Naming](https://github.com/gruhn/awesome-naming) | [Network Automation](https://github.com/networktocode/awesome-network-automation) | [Newsletters](https://github.com/zudochkin/awesome-newsletters) | [No login web apps](https://github.com/aviaryan/awesome-no-login-web-apps) | [No/Low Code](https://github.com/kairichard/awesome-nocode-lowcode) | [Nodejs](https://github.com/sindresorhus/awesome-nodejs) | [Nodejs Security](https://github.com/lirantal/awesome-nodejs-security) | [Notebooks](https://github.com/jupyter-naas/awesome-notebooks) | [OSINT](https://github.com/jivoi/awesome-osint) | [PaaS](https://github.com/debarshibasak/awesome-paas) | [Pentest](https://github.com/enaqx/awesome-pentest) | [Personal Security Checklist](https://github.com/Lissy93/personal-security-checklist) | [Privacy](https://github.com/pluja/awesome-privacy) | [Product Design](https://github.com/ttt30ga/awesome-product-design) | [Productivity](https://github.com/jyguyomarch/awesome-productivity) | [Prometheus Alerts](https://github.com/samber/awesome-prometheus-alerts) | [Python](https://github.com/vinta/awesome-python) | [Python Scripts](https://github.com/prathimacode-hub/Awesome_Python_Scripts) | [Raspberry Pi](https://github.com/thibmaek/awesome-raspberry-pi) | [README Template](https://github.com/othneildrew/Best-README-Template) | [REST API](https://github.com/marmelab/awesome-rest) | [Rust](https://github.com/rust-unofficial/awesome-rust) | [SaaS Boilerplates](https://github.com/smirnov-am/awesome-saas-boilerplates) | [Scalability](https://github.com/binhnguyennus/awesome-scalability) | [Security Hardening](https://github.com/decalage2/awesome-security-hardening) | [Self Hosted](https://github.com/awesome-selfhosted/awesome-selfhosted) | [Shell](https://github.com/alebcay/awesome-shell) | [Scripts](https://github.com/codePerfectPlus/awesomeScripts) | [Software Architecture](https://github.com/mehdihadeli/awesome-software-architecture) | [Stacks](https://github.com/ethibox/awesome-stacks) | [Startpage](https://github.com/jnmcfly/awesome-startpage) | [Startup](https://github.com/KrishMunot/awesome-startup) | [Storage](https://github.com/okhosting/awesome-storage) | [Tech Blogs](https://github.com/markodenic/awesome-tech-blogs) | [Technical Writing](https://github.com/BolajiAyodeji/awesome-technical-writing) | [Threat Detection](https://github.com/0x4D31/awesome-threat-detection) | [Tips](https://github.com/jbhuang0604/awesome-tips) | [UUID](https://github.com/grantcarthew/awesome-unique-id) | [VSCode](https://github.com/viatsko/awesome-vscode) | [WP Speed Up](https://github.com/lukecav/awesome-wp-speed-up) |

# Google dork cheatsheet

## [](#search-filters)Search filters

| Filter                                  | Description                                                                                       | Example                                               |
| --------------------------------------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| allintext                               | Searches for occurrences of all the keywords given.                                               | `allintext:"keyword"`                                 |
| intext                                  | Searches for the occurrences of keywords all at once or one at a time.                            | `intext:"keyword"`                                    |
| inurl                                   | Searches for a URL matching one of the keywords.                                                  | `inurl:"keyword"`                                     |
| allinurl                                | Searches for a URL matching all the keywords in the query.                                        | `allinurl:"keyword"`                                  |
| intitle                                 | Searches for occurrences of keywords in title all or one.                                         | `intitle:"keyword"`                                   |
| allintitle                              | Searches for occurrences of keywords all at a time.                                               | `allintitle:"keyword"`                                |
| site                                    | Specifically searches that particular site and lists all the results for that site.               | `site:"www.google.com"`                               |
| filetype                                | Searches for a particular filetype mentioned in the query.                                        | `filetype:"pdf"`                                      |
| link                                    | Searches for external links to pages.                                                             | `link:"keyword"`                                      |
| numrange                                | Used to locate specific numbers in your searches.                                                 | `numrange:321-325`                                    |
| before/after                            | Used to search within a particular date range.                                                    | `filetype:pdf & (before:2000-01-01 after:2001-01-01)` |
| allinanchor (and also inanchor)         | This shows sites which have the keyterms in links pointing to them, in order of the most links.   | `inanchor:rat`                                        |
| allinpostauthor (and also inpostauthor) | Exclusive to blog search, this one picks out blog posts that are written by specific individuals. | `allinpostauthor:"keyword"`                           |
| related                                 | List web pages that are “similar” to a specified web page.                                        | `related:www.google.com`                              |
| cache                                   | Shows the version of the web page that Google has in its cache.                                   | `cache:www.google.com`                                |

## [](#examples)Examples

```
intext:"index of /"
Nina Simone intitle:”index.of” “parent directory” “size” “last modified” “description” I Put A Spell On You (mp4|mp3|avi|flac|aac|ape|ogg) -inurl:(jsp|php|html|aspx|htm|cf|shtml|lyrics-realm|mp3-collection) -site:.info
Bill Gates intitle:”index.of” “parent directory” “size” “last modified” “description” Microsoft (pdf|txt|epub|doc|docx) -inurl:(jsp|php|html|aspx|htm|cf|shtml|ebooks|ebook) -site:.info
parent directory DVDRip -xxx -html -htm -php -shtml -opendivx -md5 -md5sums
parent directory MP3 -xxx -html -htm -php -shtml -opendivx -md5 -md5sums
parent directory Name of Singer or album -xxx -html -htm -php -shtml -opendivx -md5 -md5sums
filetype:config inurl:web.config inurl:ftp
“Windows XP Professional” 94FBR
ext:(doc | pdf | xls | txt | ps | rtf | odt | sxw | psw | ppt | pps | xml) (intext:confidential salary | intext:"budget approved") inurl:confidential
ext:(doc | pdf | xls | txt | ps | rtf | odt | sxw | psw | ppt | pps | xml) (intext:confidential salary | intext:”budget approved”) inurl:confidential

```

## [](#operators)Operators

#### [](#search-term)Search Term

This operator searches for the exact phrase within speech marks only. This is ideal when the phrase you are using to search is ambiguous and could be easily confused with something else, or when you’re not quite getting relevant enough results back. For example:

```
"Tinned Sandwiches"

```

#### [](#or)OR

This self explanatory operator searches for a given search term OR an equivalent term.

```
site:facebook.com | site:twitter.com

```

#### [](#and)AND

```
site:facebook.com & site:twitter.com

```

#### [](#operators-combinaison)Operators combinaison

```
(site:facebook.com | site:twitter.com) & intext:"login"
(site:facebook.com | site:twitter.com) (intext:"login")

```

#### [](#include-results)Include results

This will order results by the number of occurrence of the keyword.

```
-site:facebook.com +site:facebook.*

```

#### [](#exclude-results)Exclude results

```
site:facebook.* -site:facebook.com

```

#### [](#synonyms)Synonyms

Adding a tilde to a search word tells Google that you want it to bring backsynonyms for the term as well. For example, entering “~set” will bring back results that include words like “configure”, “collection” and “change” which are all synonyms of “set”. Fun fact: “set” has the most definitions of any word in the dictionary.

```
~set

```

#### [](#glob-pattern-)Glob pattern (\*)

Putting an asterisk in a search tells Google ‘I don’t know what goes here’. Basically, it’s really good for finding half remembered song lyrics or names of things.

```
site:*.com
```

### MySQL Cheat Sheet

> Help with SQL commands to interact with a MySQL database

#### MySQL Locations

* Mac             */usr/local/mysql/bin*
* Windows         */Program Files/MySQL/MySQL _version_/bin*
* Xampp           */xampp/mysql/bin*

#### Add mysql to your PATH

```bash
# Current Session
export PATH=${PATH}:/usr/local/mysql/bin
# Permanantly
echo 'export PATH="/usr/local/mysql/bin:$PATH"' >> ~/.bash_profile
```

On [Windows](https://www.qualitestgroup.com/resources/knowledge-center/how-to-guide/add-mysql-path-windows/)

#### Login

```bash
mysql -u root -p
```

#### Show Users

```sql
SELECT User, Host FROM mysql.user;
```

#### Create User

```sql
CREATE USER 'someuser'@'localhost' IDENTIFIED BY 'somepassword';
```

#### Grant All Priveleges On All Databases

```sql
GRANT ALL PRIVILEGES ON * . * TO 'someuser'@'localhost';
FLUSH PRIVILEGES;
```

#### Show Grants

```sql
SHOW GRANTS FOR 'someuser'@'localhost';
```

#### Remove Grants

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'someuser'@'localhost';
```

#### Delete User

```sql
DROP USER 'someuser'@'localhost';
```

#### Exit

```sql
exit;
```

#### Show Databases

```sql
SHOW DATABASES
```

#### Create Database

```sql
CREATE DATABASE acme;
```

#### Delete Database

```sql
DROP DATABASE acme;
```

#### Select Database

```sql
USE acme;
```

#### Create Table

```sql
CREATE TABLE users(
id INT AUTO_INCREMENT,
   first_name VARCHAR(100),
   last_name VARCHAR(100),
   email VARCHAR(50),
   password VARCHAR(20),
   location VARCHAR(100),
   dept VARCHAR(100),
   is_admin TINYINT(1),
   register_date DATETIME,
   PRIMARY KEY(id)
);
```

#### Delete / Drop Table

```sql
DROP TABLE tablename;
```

#### Show Tables

```sql
SHOW TABLES;
```

#### Insert Row / Record

```sql
INSERT INTO users (first_name, last_name, email, password, location, dept, is_admin, register_date) values ('Brad', 'Traversy', 'brad@gmail.com', '123456','Massachusetts', 'development', 1, now());
```

#### Insert Multiple Rows

```sql
INSERT INTO users (first_name, last_name, email, password, location, dept,  is_admin, register_date) values ('Fred', 'Smith', 'fred@gmail.com', '123456', 'New York', 'design', 0, now()), ('Sara', 'Watson', 'sara@gmail.com', '123456', 'New York', 'design', 0, now()),('Will', 'Jackson', 'will@yahoo.com', '123456', 'Rhode Island', 'development', 1, now()),('Paula', 'Johnson', 'paula@yahoo.com', '123456', 'Massachusetts', 'sales', 0, now()),('Tom', 'Spears', 'tom@yahoo.com', '123456', 'Massachusetts', 'sales', 0, now());
```

#### Select

```sql
SELECT * FROM users;
SELECT first_name, last_name FROM users;
```

#### Where Clause

```sql
SELECT * FROM users WHERE location='Massachusetts';
SELECT * FROM users WHERE location='Massachusetts' AND dept='sales';
SELECT * FROM users WHERE is_admin = 1;
SELECT * FROM users WHERE is_admin > 0;
```

#### Delete Row

```sql
DELETE FROM users WHERE id = 6;
```

#### Update Row

```sql
UPDATE users SET email = 'freddy@gmail.com' WHERE id = 2;

```

#### Add New Column

```sql
ALTER TABLE users ADD age VARCHAR(3);
```

#### Modify Column

```sql
ALTER TABLE users MODIFY COLUMN age INT(3);
```

#### Order By (Sort)

```sql
SELECT * FROM users ORDER BY last_name ASC;
SELECT * FROM users ORDER BY last_name DESC;
```

#### Concatenate Columns

```sql
SELECT CONCAT(first_name, ' ', last_name) AS 'Name', dept FROM users;

```

#### Select Distinct Rows

```sql
SELECT DISTINCT location FROM users;

```

#### Between (Select Range)

```sql
SELECT * FROM users WHERE age BETWEEN 20 AND 25;
```

#### Like (Searching)

```sql
SELECT * FROM users WHERE dept LIKE 'd%';
SELECT * FROM users WHERE dept LIKE 'dev%';
SELECT * FROM users WHERE dept LIKE '%t';
SELECT * FROM users WHERE dept LIKE '%e%';
```

#### Not Like

```sql
SELECT * FROM users WHERE dept NOT LIKE 'd%';
```

#### IN

```sql
SELECT * FROM users WHERE dept IN ('design', 'sales');
```

#### Create & Remove Index

```sql
CREATE INDEX LIndex On users(location);
DROP INDEX LIndex ON users;
```

#### New Table With Foreign Key (Posts)

```sql
CREATE TABLE posts(
id INT AUTO_INCREMENT,
   user_id INT,
   title VARCHAR(100),
   body TEXT,
   publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
   PRIMARY KEY(id),
   FOREIGN KEY (user_id) REFERENCES users(id)
);
```

#### Add Data to Posts Table

```sql
INSERT INTO posts(user_id, title, body) VALUES (1, 'Post One', 'This is post one'),(3, 'Post Two', 'This is post two'),(1, 'Post Three', 'This is post three'),(2, 'Post Four', 'This is post four'),(5, 'Post Five', 'This is post five'),(4, 'Post Six', 'This is post six'),(2, 'Post Seven', 'This is post seven'),(1, 'Post Eight', 'This is post eight'),(3, 'Post Nine', 'This is post none'),(4, 'Post Ten', 'This is post ten');
```

#### INNER JOIN

```sql
SELECT
  users.first_name,
  users.last_name,
  posts.title,
  posts.publish_date
FROM users
INNER JOIN posts
ON users.id = posts.user_id
ORDER BY posts.title;
```

#### New Table With 2 Foriegn Keys

```sql
CREATE TABLE comments(
	id INT AUTO_INCREMENT,
    post_id INT,
    user_id INT,
    body TEXT,
    publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY(id),
    FOREIGN KEY(user_id) references users(id),
    FOREIGN KEY(post_id) references posts(id)
);
```

#### Add Data to Comments Table

```sql
INSERT INTO comments(post_id, user_id, body) VALUES (1, 3, 'This is comment one'),(2, 1, 'This is comment two'),(5, 3, 'This is comment three'),(2, 4, 'This is comment four'),(1, 2, 'This is comment five'),(3, 1, 'This is comment six'),(3, 2, 'This is comment six'),(5, 4, 'This is comment seven'),(2, 3, 'This is comment seven');
```

#### Left Join

```sql
SELECT
comments.body,
posts.title
FROM comments
LEFT JOIN posts ON posts.id = comments.post_id
ORDER BY posts.title;

```

#### Join Multiple Tables

```sql
SELECT
comments.body,
posts.title,
users.first_name,
users.last_name
FROM comments
INNER JOIN posts on posts.id = comments.post_id
INNER JOIN users on users.id = comments.user_id
ORDER BY posts.title;

```

#### Aggregate Functions

```sql
SELECT COUNT(id) FROM users;
SELECT MAX(age) FROM users;
SELECT MIN(age) FROM users;
SELECT SUM(age) FROM users;
SELECT UCASE(first_name), LCASE(last_name) FROM users;

```

#### Group By

```sql
SELECT age, COUNT(age) FROM users GROUP BY age;
SELECT age, COUNT(age) FROM users WHERE age > 20 GROUP BY age;
SELECT age, COUNT(age) FROM users GROUP BY age HAVING count(age) >=2;

```

### Spam Template

This is a template from a spammer to give you an idea of what could be expected from a spammer.

```bash
{
{I have|I've} been {surfing|browsing} online more than {three|3|2|4} hours today, yet I never found any interesting article like yours. {It's|It
is} pretty worth enough for me. {In my opinion|Personally|In my view}, if all {webmasters|site owners|website owners|web owners} and bloggers made good content as
you did, the {internet|net|web} will be {much more|a lot more}
useful than ever before.|
I {couldn't|could not} {resist|refrain from} commenting. {Very well|Perfectly|Well|Exceptionally well} written!|
{I will|I'll} {right away|immediately} {take hold of|grab|clutch|grasp|seize|snatch}
your {rss|rss feed} as I {can not|can't} {in finding|find|to find} your {email|e-mail} subscription {link|hyperlink} or {newsletter|e-newsletter} service. Do {you have|you've} any?
{Please|Kindly} {allow|permit|let} me {realize|recognize|understand|recognise|know} {so that|in order that} I {may just|may|could} subscribe.
Thanks.|
{It is|It's} {appropriate|perfect|the best} time to make some plans for the future and {it is|it's} time to be happy.
{I have|I've} read this post and if I could I {want to|wish to|desire to} suggest you {few|some} interesting things or {advice|suggestions|tips}. {Perhaps|Maybe} you {could|can} write next articles referring to this article. I {want to|wish to|desire to} read {more|even more} things about it!|
{It is|It's} {appropriate|perfect|the best} time to make {a few|some} plans for {the future|the
longer term|the long run} and {it is|it's} time to be happy. {I have|I've} {read|learn} this {post|submit|publish|put up} and
if I {may just|may|could} I {want to|wish to|desire to} {suggest|recommend|counsel} you {few|some} {interesting|fascinating|attention-grabbing} {things|issues} or {advice|suggestions|tips}.
{Perhaps|Maybe} you {could|can} write {next|subsequent} articles {relating to|referring to|regarding} this article.
I {want to|wish to|desire to} {read|learn} {more|even
more} {things|issues} {approximately|about} it!|
{I have|I've} been {surfing|browsing} {online|on-line} {more than|greater than} {three|3} hours {these days|nowadays|today|lately|as of late}, {yet|but} I {never|by no means} {found|discovered} any {interesting|fascinating|attention-grabbing} article like yours. {It's|It is} {lovely|pretty|beautiful} {worth|value|price} {enough|sufficient} for
me. {In my opinion|Personally|In my view},
if all {webmasters|site owners|website owners|web owners} and bloggers made {just right|good|excellent} {content|content material} as {you did|you probably did}, the {internet|net|web} {will be|shall be|might
be|will probably be|can be|will likely be} {much
more|a lot more} {useful|helpful} than ever before.|
Ahaa, its {nice|pleasant|good|fastidious} {discussion|conversation|dialogue} {regarding|concerning|about|on the topic of} this {article|post|piece of writing|paragraph} {here|at this place} at this
{blog|weblog|webpage|website|web site}, I have read all
that, so {now|at this time} me also commenting {here|at this
place}.|
I am sure this {article|post|piece of writing|paragraph}
has touched all the internet {users|people|viewers|visitors}, its really really {nice|pleasant|good|fastidious} {article|post|piece of writing|paragraph} on
building up new {blog|weblog|webpage|website|web site}.
|
Wow, this {article|post|piece of writing|paragraph} is {nice|pleasant|good|fastidious}, my
{sister|younger sister} is analyzing {such|these|these kinds of} things, {so|thus|therefore} I am going to {tell|inform|let
know|convey} her.|
{Saved as a favorite|bookmarked!!}, {I really like|I
like|I love} {your blog|your site|your web site|your website}!
|
Way cool! Some {very|extremely} valid points! I
appreciate you {writing this|penning this} {article|post|write-up} {and the|and also the|plus the}
rest of the {site is|website is} {also very|extremely|very|also really|really} good.
|
Hi, {I do believe|I do think} {this is an excellent|this
is a great} {blog|website|web site|site}. I stumbledupon it ;) {I will|I am going
to|I'm going to|I may} {come back|return|revisit} {once again|yet again} {since I|since i have} {bookmarked|book marked|book-marked|saved as a favorite} it. Money and freedom {is the best|is the greatest} way to change, may you be rich and continue to {help|guide} {other people|others}.|
Woah! I'm really {loving|enjoying|digging} the template/theme of this {site|website|blog}.
It's simple, yet effective. A lot of times it's {very
hard|very difficult|challenging|tough|difficult|hard} to
get that "perfect balance" between {superb usability|user friendliness|usability} and {visual appearance|visual appeal|appearance}.
I must say {that you've|you have|you've} done a {awesome|amazing|very good|superb|fantastic|excellent|great} job with this.
{In addition|Additionally|Also}, the blog loads {very|extremely|super} {fast|quick} for me on {Safari|Internet explorer|Chrome|Opera|Firefox}.
{Superb|Exceptional|Outstanding|Excellent} Blog!
|
These are {really|actually|in fact|truly|genuinely} {great|enormous|impressive|wonderful|fantastic} ideas
in {regarding|concerning|about|on the topic of} blogging.
You have touched some {nice|pleasant|good|fastidious} {points|factors|things}
here. Any way keep up wrinting.|
{I love|I really like|I enjoy|I like|Everyone loves} what you guys
{are|are usually|tend to be} up too. {This sort
of|This type of|Such|This kind of} clever work and {exposure|coverage|reporting}!

Keep up the {superb|terrific|very good|great|good|awesome|fantastic|excellent|amazing|wonderful} works guys I've {incorporated||added|included} you guys to {|my|our||my personal|my own} blogroll.|
{Howdy|Hi there|Hey there|Hi|Hello|Hey}! Someone in my {Myspace|Facebook} group shared this {site|website} with us so I came to {give it a look|look it over|take a look|check it out}. I'm definitely {enjoying|loving} the information.
I'm {book-marking|bookmarking} and will be tweeting this to my followers! {Terrific|Wonderful|Great|Fantastic|Outstanding|Exceptional|Superb|Excellent} blog and {wonderful|terrific|brilliant|amazing|great|excellent|fantastic|outstanding|superb} {style and design|design and style|design}.|
{I love|I really like|I enjoy|I like|Everyone loves} what you guys {are|are usually|tend to be} up too. {This sort of|This type of|Such|This kind of} clever work and {exposure|coverage|reporting}! Keep up the {superb|terrific|very good|great|good|awesome|fantastic|excellent|amazing|wonderful} works guys I've {incorporated|added|included} you guys to
{|my|our|my personal|my own} blogroll.|
{Howdy|Hi there|Hey there|Hi|Hello|Hey} would you mind {stating|sharing} which blog platform you're {working with|using}? I'm {looking|planning|going} to start my own blog {in the near future|soon} but I'm having a {tough|difficult|hard} time {making a decision|selecting|choosing|deciding} between BlogEngine/Wordpress/B2evolution and Drupal. The reason I ask is because your {design and style|design|layout} seems different then most blogs and I'm looking for something {completely
unique|unique}.                  P.S {My apologies|Apologies|Sorry} for {getting|being} off-topic but
I had to ask!|
{Howdy|Hi there|Hi|Hey there|Hello|Hey} would you mind letting
me know which {webhost|hosting company|web host} you're {utilizing|working with|using}? I've
loaded your blog in 3 {completely different|different} {internet browsers|web browsers|browsers} and I must say this blog loads a lot {quicker|faster} then most.

Can you {suggest|recommend} a good {internet
hosting|web hosting|hosting} provider at a {honest|reasonable|fair}
price? {Thanks a lot|Kudos|Cheers|Thank you|Many thanks|Thanks}, I
appreciate it!|
{I love|I really like|I like|Everyone loves} it {when people|when individuals|when folks|whenever people} {come together|get
together} and share {opinions|thoughts|views|ideas}. Great {blog|website|site}, {keep it up|continue the good work|stick with
it}!|
Thank you for the {auspicious|good} writeup. It in fact was a amusement account it.
Look advanced to {far|more} added agreeable from you! {By the way|However}, how {can|could} we communicate?
|
{Howdy|Hi there|Hey there|Hello|Hey} just wanted to give you a quick heads up.

The {text|words} in your {content|post|article} seem to be running off
the screen in {Ie|Internet explorer|Chrome|Firefox|Safari|Opera}.
I'm not sure if this is a {format|formatting} issue or something to do with {web browser|internet browser|browser} compatibility but I {thought|figured} I'd
post to let you know. The {style and design|design
and style|layout|design} look great though! Hope you get the {problem|issue} {solved|resolved|fixed} soon.

{Kudos|Cheers|Many thanks|Thanks}|
This is a topic {that is|that's|which is} {close to|near to} my heart... {Cheers|Many thanks|Best wishes|Take care|Thank you}! {Where|Exactly where} are your contact details though?|
It's very {easy|simple|trouble-free|straightforward|effortless}
to find out any {topic|matter} on {net|web} as compared to {books|textbooks}, as I found this {article|post|piece of writing|paragraph} at this {website|web site|site|web
page}.|
Does your {site|website|blog} have a contact page?
I'm having {a tough time|problems|trouble} locating it but, I'd
like to {send|shoot} you an {e-mail|email}. I've got some {creative ideas|recommendations|suggestions|ideas} for your blog you might be interested in hearing. Either way, great {site|website|blog} and I look forward to seeing it {develop|improve|expand|grow} over time.|
{Hola|Hey there|Hi|Hello|Greetings}! I've been {following|reading} your {site|web site|website|weblog|blog} for {a long time|a while|some time} now and finally got the {bravery|courage} to go ahead and give you
a shout out from  {New Caney|Kingwood|Huffman|Porter|Houston|Dallas|Austin|Lubbock|Humble|Atascocita} {Tx|Texas}!
Just wanted to {tell you|mention|say} keep up the {fantastic|excellent|great|good}
{job|work}!|
Greetings from {Idaho|Carolina|Ohio|Colorado|Florida|Los angeles|California}!

I'm {bored to tears|bored to death|bored} at work so I decided to {check out|browse} your {site|website|blog} on my iphone during lunch break. I {enjoy|really like|love} the {knowledge|info|information} you {present|provide} here and can't wait to
take a look when I get home. I'm {shocked|amazed|surprised} at how {quick|fast} your blog loaded on my {mobile|cell phone|phone} .. I'm not even
using WIFI, just 3G .. {Anyhow|Anyways}, {awesome|amazing|very good|superb|good|wonderful|fantastic|excellent|great} {site|blog}!
|
Its {like you|such as you} {read|learn} my {mind|thoughts}!
You {seem|appear} {to understand|to know|to grasp} {so much|a lot} {approximately|about} this,
{like you|such as you} wrote the {book|e-book|guide|ebook|e book} in it or something.
{I think|I feel|I believe} {that you|that you simply|that you just} {could|can} do with
{some|a few} {%|p.c.|percent} to {force|pressure|drive|power} the message {house|home} {a bit|a little bit}, {however|but} {other than|instead of} that, {this is|that is} {great|wonderful|fantastic|magnificent|excellent} blog. {A great|An excellent|A fantastic} read. {I'll|I will} {definitely|certainly} be back.|
I visited {multiple|many|several|various} {websites|sites|web sites|web pages|blogs} {but|except|however} the audio {quality|feature} for audio songs {current|present|existing} at this {website|web site|site|web page} is {really|actually|in fact|truly|genuinely} {marvelous|wonderful|excellent|fabulous|superb}.|
{Howdy|Hi there|Hi|Hello}, i read your blog {occasionally|from time to time} and i own a similar one and i was just {wondering|curious} if you get a lot of spam {comments|responses|feedback|remarks}? If so how do you {prevent|reduce|stop|protect against} it, any plugin or anything you can {advise|suggest|recommend}? I get so much lately it's driving me {mad|insane|crazy} so any {assistance|help|support} is very much appreciated.|
Greetings! {Very helpful|Very useful} advice {within this|in this particular} {article|post}! {It is the|It's the} little changes {that make|which will make|that produce|that will make} {the biggest|the largest|the greatest|the most important|the most significant} changes. {Thanks a lot|Thanks|Many thanks} for sharing!|
{I really|I truly|I seriously|I absolutely} love {your blog|your site|your website}.. {Very nice|Excellent|Pleasant|Great} colors & theme. Did you {create|develop|make|build} {this website|this site|this web site|this amazing site} yourself? Please reply back as I'm {looking to|trying to|planning to|wanting to|hoping to|attempting to} create {my own|my very own|my own personal} {blog|website|site} and {would like to|want to|would love to} {know|learn|find out} where you got this from or {what the|exactly what the|just what the} theme {is called|is named}. {Thanks|Many thanks|Thank you|Cheers|Appreciate it|Kudos}!|
{Hi there|Hello there|Howdy}! This {post|article|blog post} {couldn't|could not} be written {any better|much better}! {Reading through|Looking at|Going through|Looking through} this {post|article} reminds me of my previous roommate! He {always|constantly|continually} kept {talking about|preaching about} this. {I will|I'll|I am going to|I most certainly will} {forward|send} {this article|this information|this post} to him. {Pretty sure|Fairly certain} {he will|he'll|he's going to} {have a good|have a very good|have a great} read. {Thank you for|Thanks for|Many thanks for|I appreciate you for} sharing!|
{Wow|Whoa|Incredible|Amazing}! This blog looks {exactly|just} like my old one! It's on a {completely|entirely|totally} different {topic|subject} but it has pretty much the same {layout|page layout} and design. {Excellent|Wonderful|Great|Outstanding|Superb} choice of colors!|
{There is|There's} {definately|certainly} {a lot to|a great deal to} {know about|learn about|find out about} this {subject|topic|issue}. {I like|I love|I really like} {all the|all of the} points {you made|you've made|you have made}.|
{You made|You've made|You have made} some {decent|good|really good} points there. I {looked|checked} {on the internet|on the web|on the net} {for more info|for more information|to find out more|to learn more|for additional information} about the issue and found {most individuals|most people} will go along with your views on {this website|this site|this web site}.|
{Hi|Hello|Hi there|What's up}, I {log on to|check|read} your {new stuff|blogs|blog} {regularly|like every week|daily|on a regular basis}. Your {story-telling|writing|humoristic} style is {awesome|witty}, keep {doing what you're doing|up the good work|it up}!|
I {simply|just} {could not|couldn't} {leave|depart|go away} your {site|web site|website} {prior to|before} suggesting that I {really|extremely|actually} {enjoyed|loved} {the standard|the usual} {information|info} {a person|an individual} {supply|provide} {for your|on your|in your|to your} {visitors|guests}? Is {going to|gonna} be {back|again} {frequently|regularly|incessantly|steadily|ceaselessly|often|continuously} {in order to|to} {check up on|check out|inspect|investigate cross-check} new posts|
{I wanted|I needed|I want to|I need to} to thank you for this {great|excellent|fantastic|wonderful|good|very good} read!! I {definitely|certainly|absolutely} {enjoyed|loved} every {little bit of|bit of} it. {I have|I've got|I have got} you {bookmarked|book marked|book-marked|saved as a favorite} {to check out|to look at} new {stuff you|things you} post�\
```

### Dump ENV variables to JSON using Python

```python
#!/usr/bin/env python
import json
import sys

try:
    dotenv = sys.argv[1]
except IndexError as e:
    dotenv = '.env'

with open(dotenv, 'r') as f:
    content = f.readlines()

# removes whitespace chars like '\n' at the end of each line
content = [x.strip().split('=') for x in content if '=' in x]
print(json.dumps(dict(content)))
```

.env:

```bash
SOME_ENV_VAR_DEBUG=True
SOME_EMPTY_VALUE=''
```

Usage combined with jq

```bash
python env-to-json.py | jq
```

output:

```json
{
  "SOME_ENV_VAR_DEBUG": "True",
  "SOME_EMPTY_VALUE": "''"
}
```

The script should be improved with more validity checks or maybe loading env variables somehow instead of splitting on =. Could add usage help, etc.

If you'd like to have "" instead of "''", you may edit the python script, but I personally used sed:

```bash
sed "s#\"''\"#\"\"#"
```

### Kill a process and make it look like an accident

Script to inject an exit(0) syscall into a running process. NB: only x86_64 for now!

```bash
gdb -p "$1" -batch -ex 'set {short}$rip = 0x050f' -ex 'set $rax=231' -ex 'set $rdi=0' -ex 'cont'
```
