# Loftwah's Cheatsheet

![LOFTWAH'S](https://user-images.githubusercontent.com/19922556/150899356-b3930a05-6b65-43c4-a492-f5b7e5f94b39.png)

This is a repo with a bunch of stuff I use regularly. If you would like to know a little bit more about me please visit my [Website](https://lofts.sh), [My Video Portfolio](https://www.youtube.com/playlist?list=PLgr1VpT986yP4I9bKEWWWssKL2ajRubPM), [My Portfolio for Web Design and Development](https://lofts.sh/my-portfolio-web/) or my [Resume](https://lofts.sh/resume/).

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Links](#links)
  - [Google](#google)
- [Docker](#docker)
  - [How to migrate a Docker image without registry](#how-to-migrate-a-docker-image-without-registry)
  - [Clean up Docker before starting](#clean-up-docker-before-starting)
  - [Install Docker on Amazon Linux 2](#install-docker-on-amazon-linux-2)
  - [Install Docker on Linux](#install-docker-on-linux)
  - [Install Docker-Compose](#install-docker-compose)
    - [Shell Scripts](#shell-scripts)
  - [Dockerfile](#dockerfile)
- [Git and GitHub](#git-and-github)
  - [Initial Git configuration](#initial-git-configuration)
  - [Git on Oh My Zsh](#git-on-oh-my-zsh)
    - [Generate SSH Keys](#generate-ssh-keys)
    - [Generate and auto-sign your commits with GPG](#generate-and-auto-sign-your-commits-with-gpg)
- [Kubernetes](#kubernetes)
  - [Kompose](#kompose)
  - [ArgoCD](#argocd)
  - [Install by script](#install-by-script)
  - [Install by helm](#install-by-helm)
- [Linux](#linux)
  - [Linuxbrew](#linuxbrew)
  - [zsh](#zsh)
- [Nginx](#nginx)
- [Node Version Manager](#node-version-manager)
  - [Install NVM](#install-nvm)
- [Piping Server](#piping-server)
  - [Install or Run the Piping Server](#install-or-run-the-piping-server)
  - [How to use the Piping Server](#how-to-use-the-piping-server)
- [AWS S3](#aws-s3)
- [Visual Studio Code](#visual-studio-code)
- [Operations](#operations)
  - [Checking Ports](#checking-ports)
  - [General Linux](#general-linux)
    - [Grep](#grep)
    - [ps](#ps)
    - [Networking](#networking)
    - [cURL](#curl)
    - [Netcat](#netcat)
    - [Nmap](#nmap)
    - [Password generation](#password-generation)
    - [Openssl](#openssl)
    - [Tail log with colored output](#tail-log-with-colored-output)
- [Searching](#searching)
- [Test a WebSocket using cURL](#test-a-websocket-using-curl)
- [Minimal safe Bash script template](#minimal-safe-bash-script-template)
- [Stack data structure in JavaScript](#stack-data-structure-in-javascript)
- [HTTPS Request with Node.js and TypeScript](#https-request-with-nodejs-and-typescript)
- [Python function to calculate aspect ratio of an image](#python-function-to-calculate-aspect-ratio-of-an-image)
- [HTML Simple Maintenance Page](#html-simple-maintenance-page)
- [Download the latest release from GitHub](#download-the-latest-release-from-github)
- [HTTPS certificate for localhost](#https-certificate-for-localhost)
  - [Create a Certificate authority (CA)](#create-a-certificate-authority-ca)
  - [Domain name certificate](#domain-name-certificate)
  - [Trust the local CA](#trust-the-local-ca)
    - [Windows: Chrome, & Edge](#windows-chrome--edge)
    - [Windows: Firefox](#windows-firefox)
  - [HTTPS with Let's Encrypt for Nginx](#https-with-lets-encrypt-for-nginx)
    - [Virtual hosts](#virtual-hosts)
    - [Certbot](#certbot)
    - [Automatic renewal](#automatic-renewal)
    - [HTTP/2](#http2)
    - [Stronger settings for A+](#stronger-settings-for-a)
      - [Trusted certificate](#trusted-certificate)
    - [SSL](#ssl)
- [SMTP Settings for common providers](#smtp-settings-for-common-providers)
    - [Microsoft 365](#microsoft-365)
    - [Amazon SES](#amazon-ses)
    - [Google GSuite | Workspace (why did they rename this? lol)](#google-gsuite--workspace-why-did-they-rename-this-lol)
  - [Awesome (Topic)](#awesome-topic)
  - [Awesome Cheatsheets](#awesome-cheatsheets)
- [Google dork cheatsheet](#google-dork-cheatsheet)
  - [Search filters](#search-filters)
  - [Examples](#examples)
  - [Operators](#operators)
    - [Search Term](#search-term)
    - [OR](#or)
    - [AND](#and)
    - [Operators combinaison](#operators-combinaison)
    - [Include results](#include-results)
    - [Exclude results](#exclude-results)
    - [Synonyms](#synonyms)
    - [Glob pattern (\*)](#glob-pattern-%5C)
- [Dump ENV variables to JSON using Python](#dump-env-variables-to-json-using-python)
- [Kill a process and make it look like an accident](#kill-a-process-and-make-it-look-like-an-accident)
- [Run an ad-hoc web server](#run-an-ad-hoc-web-server)
- [Latency comparison](#latency-comparison)
- [Example async function JavaScript](#example-async-function-javascript)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Links

The links section here is for my own use. If you would like to customize it please make your own fork and update this section as your own.

[All Cybercrime IP Feeds](https://iplists.firehol.org/) | [Amazing Developers on YouTube](https://github.com/ErikCH/DevYouTubeList) | [APIs Guru](https://apis.guru/) | [APT & Cybercriminals Campaign Collection](https://github.com/CyberMonitor/APT_CyberCriminal_Campagin_Collections) |  [AWS ap-southeast-2](https://ap-southeast-2.console.aws.amazon.com/console/home?region=ap-southeast-2) | [AWS Guide](https://github.com/open-guides/og-aws) | [Canva](https://canva.com) | [Certificate Transparency Subdomains](https://github.com/internetwache/CT_subdomains) | [cheat.sh](https://cht.sh/) | [CloudFlare](https://dash.cloudflare.com/) | [ColorHub](https://colorhub.vercel.app/select-palette) | [CyberChef](https://gchq.github.io/CyberChef/) | [Data Center Server Rack Wiki](https://community.fs.com/blog/different-types-of-server-rack-used-in-data-center.html) | [Default Credentials Cheat Sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet) | [DevOps Ninja](https://github.com/eliarms/devops-ninja) |  [EC2 Pricing](https://ec2pricing.net/) | [ExtendsClass](https://extendsclass.com/) | [free-for.dev](https://free-for.dev/#/) | [HackTricks](https://book.hacktricks.xyz/welcome/readme) | [How Git Branches Work](https://www.freecodecamp.org/news/how-git-branches-work/) | [ISO/IEC 11801 - cabling](https://en.wikipedia.org/wiki/ISO/IEC_11801) | [JSON Placeholder](https://jsonplaceholder.typicode.com/) | [Linux Command Library One-liners](https://linuxcommandlibrary.com/basic/oneliners.html) | [Most Common Domain Prefix/Suffix List](https://gist.github.com/erikig/826f49442929e9ecfab6d7c481870700) | [Post Compromize Active Directory Checklist](https://www.pwndefend.com/2021/09/15/post-compromise-active-directory-checklist/) | [SPFToolbox](https://spftoolbox.com/) | [theme.park](https://theme-park.dev/) | [Ultimate Guide to Becoming a DevOps Engineer](https://www.contino.io/insights/devops-engineer-guide) | [WordPress Code Reference](https://developer.wordpress.org/reference/) | [WordPress Query Comprehensive Reference](https://luetkemj.github.io/wp-query-ref/) | [Zero Trust to the Endpoint](https://f.hubspotusercontent00.net/hubfs/5411606/Content/The%20CISO%E2%80%99s%20Guide%20to%20Extending%20Zero%20Trust%20to%20the%20Endpoint.pdf)

### Google

I use Google Workspace for a lot of my work.

[Admin Console](https://admin.google.com/ac/home?hl=en) | [Cloud Platform](https://console.cloud.google.com/home/dashboard) | [Drive](https://drive.google.com/drive/u/0/) | [Gmail](https://mail.google.com/) | [Sheets](https://sheets.google.com/) | [How Search Works](https://www.google.com/search/howsearchworks/?fg=1)

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

## Linux

| [Ubuntu Server Download](https://ubuntu.com/download/server) | [Linux Basics](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-basics) | [Scripts and Snippets](https://lofts.sh/scripts-and-snippets-you-should-use/) | [Set Up SSH Keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04) | [SSH Shortcut](https://www.digitalocean.com/community/tutorials/how-to-create-an-ssh-shortcut) | [Self Signed Certificate with Custom Root CA](https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309) |

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

#### Netcat

Netcat can be used to send and receive data over a network. It can also be used to test redis connections.

```bash
nc -v -ssl your-redis-url 6379
# or if you don't have encryption enabled
nc -v your-redis-url 6379
```

#### Nmap

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

## Test a WebSocket using cURL

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

## HTML Simple Maintenance Page

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

## Download the latest release from GitHub

```bash
curl -s https://api.github.com/repos/jgm/pandoc/releases/latest \
| grep "browser_download_url.*deb" \
| cut -d : -f 2,3 \
| tr -d \" \
| wget -qi -
```

## SMTP Settings for common providers

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

[A11Y](https://github.com/brunopulis/awesome-a11y) | [Agile](https://github.com/lorabv/awesome-agile) | [Authentication](https://github.com/casbin/awesome-auth) | [Automation Scripts](https://github.com/python-geeks/Automation-scripts) | [Argo](https://github.com/terrytangyuan/awesome-argo) | [AWS](https://github.com/donnemartin/awesome-aws) | [Azure Policy](https://github.com/globalbao/awesome-azure-policy) | [Bash](https://github.com/awesome-lists/awesome-bash) | [Books](https://github.com/hackerkid/Mind-Expanding-Books) | [Business Intelligence](https://github.com/thenaturalist/awesome-business-intelligence) | [Chaos Engineering](https://github.com/dastergon/awesome-chaos-engineering) |[Cheatsheets](https://github.com/LeCoupa/awesome-cheatsheets) | [CI](https://github.com/ligurio/awesome-ci) | [Cloud Native](https://github.com/rootsongjc/awesome-cloud-native) | [Cloud Security](https://github.com/4ndersonLin/awesome-cloud-security) | [CTO](https://github.com/kuchin/awesome-cto) | [Data Science](https://github.com/academic/awesome-datascience) | [Dataset Tools](https://github.com/jsbroks/awesome-dataset-tools) | [DevOps](https://github.com/wmariuss/awesome-devops) | [DevSecOps](https://github.com/sottlmarek/DevSecOps) | [Discord Communities](https://github.com/mhxion/awesome-discord-communities) | [Docker](https://github.com/veggiemonk/awesome-docker) | [Docker Compose](https://github.com/docker/awesome-compose) | [eBPF](https://github.com/zoidbergwill/awesome-ebpf) | [GitHub Actions](https://github.com/sdras/awesome-actions) | [Golang](https://github.com/avelino/awesome-go) | [GraphQL](https://github.com/chentsulin/awesome-graphql) | [gRPC](https://github.com/grpc-ecosystem/awesome-grpc) | [Guidelines](https://github.com/Kristories/awesome-guidelines) | [Home Kubernetes](https://github.com/k8s-at-home/awesome-home-kubernetes) | [JavaScript Mini Projects](https://github.com/thinkswell/javascript-mini-projects) | [json](https://github.com/burningtree/awesome-json) | [k8s Resources](https://github.com/tomhuang12/awesome-k8s-resources) | [Kubernetes](https://github.com/ramitsurana/awesome-kubernetes) | [Linux Containers](https://github.com/Friz-zy/awesome-linux-containers) | [Linux Software](https://github.com/luong-komorebi/Awesome-Linux-Software) | [Leading and managing](https://github.com/LappleApple/awesome-leading-and-managing) | [Naming](https://github.com/gruhn/awesome-naming) | [Network Automation](https://github.com/networktocode/awesome-network-automation) | [Newsletters](https://github.com/zudochkin/awesome-newsletters) | [No login web apps](https://github.com/aviaryan/awesome-no-login-web-apps) | [No/Low Code](https://github.com/kairichard/awesome-nocode-lowcode) | [Nodejs](https://github.com/sindresorhus/awesome-nodejs) | [Nodejs Security](https://github.com/lirantal/awesome-nodejs-security) | [Notebooks](https://github.com/jupyter-naas/awesome-notebooks) | [OSINT](https://github.com/jivoi/awesome-osint) | [PaaS](https://github.com/debarshibasak/awesome-paas) | [Pentest](https://github.com/enaqx/awesome-pentest) | [Personal Security Checklist](https://github.com/Lissy93/personal-security-checklist) | [Privacy](https://github.com/pluja/awesome-privacy) | [Product Design](https://github.com/ttt30ga/awesome-product-design) | [Productivity](https://github.com/jyguyomarch/awesome-productivity) | [Prometheus Alerts](https://github.com/samber/awesome-prometheus-alerts) | [Python](https://github.com/vinta/awesome-python) | [Python Scripts](https://github.com/prathimacode-hub/Awesome_Python_Scripts) | [Raspberry Pi](https://github.com/thibmaek/awesome-raspberry-pi) | [README Template](https://github.com/othneildrew/Best-README-Template) | [REST API](https://github.com/marmelab/awesome-rest) | [Rust](https://github.com/rust-unofficial/awesome-rust) | [SaaS Boilerplates](https://github.com/smirnov-am/awesome-saas-boilerplates) | [Scalability](https://github.com/binhnguyennus/awesome-scalability) | [Security Hardening](https://github.com/decalage2/awesome-security-hardening) | [Self Hosted](https://github.com/awesome-selfhosted/awesome-selfhosted) | [Shell](https://github.com/alebcay/awesome-shell) | [Scripts](https://github.com/codePerfectPlus/awesomeScripts) | [Software Architecture](https://github.com/mehdihadeli/awesome-software-architecture) | [Stacks](https://github.com/ethibox/awesome-stacks) | [Startpage](https://github.com/jnmcfly/awesome-startpage) | [Startup](https://github.com/KrishMunot/awesome-startup) | [Storage](https://github.com/okhosting/awesome-storage) | [Tech Blogs](https://github.com/markodenic/awesome-tech-blogs) | [Technical Writing](https://github.com/BolajiAyodeji/awesome-technical-writing) | [Threat Detection](https://github.com/0x4D31/awesome-threat-detection) | [Tips](https://github.com/jbhuang0604/awesome-tips) | [UUID](https://github.com/grantcarthew/awesome-unique-id) | [VSCode](https://github.com/viatsko/awesome-vscode) | [WP Speed Up](https://github.com/lukecav/awesome-wp-speed-up)

### Awesome Cheatsheets

[express.js](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/backend/express.js) | [node.js](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/backend/node.js) | [redis](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/databases/redis.sh) | [mysql](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/databases/mysql.sh) | [mongodb](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/databases/mongodb.sh) | [angular](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/frontend/angular.js) | [css](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/frontend/css3.css) | [html](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/frontend/html5.html) | [react](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/frontend/react.js) | [tailwind](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/frontend/tailwind.css) | [vue](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/frontend/vue.js) | [xml](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/languages/XML.md) | [bash](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/languages/bash.sh) | [golang](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/languages/golang.md) | [javascript](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/languages/javascript.js) | [php](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/languages/php.php) | [python](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/languages/python.md) | [aws](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/aws.sh) | [curl](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/curl.sh) | [docker](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/docker.sh) | [elasticsearch](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/elasticsearch.js) | [gcp](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/gcp.md) | [git](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/git.sh) | [heroku](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/heroku.sh) | [kubernetes](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/kubernetes.md) | [nginx](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/nginx.sh) | [pm2](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/pm2.sh) | [puppeteer](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/puppeteer.js) [ubuntu](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/ubuntu.sh) | [vim](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/vim.txt) | [vscode](https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/vscode.md)

## Google dork cheatsheet

### Search filters

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

### Examples

```bash
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

### Operators

#### Search Term

This operator searches for the exact phrase within speech marks only. This is ideal when the phrase you are using to search is ambiguous and could be easily confused with something else, or when you’re not quite getting relevant enough results back. For example:

```bash
"Tinned Sandwiches"
```

#### OR

This self explanatory operator searches for a given search term OR an equivalent term.

```bash
site:facebook.com | site:twitter.com
```

#### AND

```bash
site:facebook.com & site:twitter.com
```

#### Operators combinaison

```bash
(site:facebook.com | site:twitter.com) & intext:"login"
(site:facebook.com | site:twitter.com) (intext:"login")
```

#### Include results

This will order results by the number of occurrence of the keyword.

```bash
-site:facebook.com +site:facebook.*
```

#### Exclude results

```bash
site:facebook.* -site:facebook.com
```

#### Synonyms

Adding a tilde to a search word tells Google that you want it to bring backsynonyms for the term as well. For example, entering “~set” will bring back results that include words like “configure”, “collection” and “change” which are all synonyms of “set”. Fun fact: “set” has the most definitions of any word in the dictionary.

```bash
~set
```

#### Glob pattern (\*)

Putting an asterisk in a search tells Google ‘I don’t know what goes here’. Basically, it’s really good for finding half remembered song lyrics or names of things.

```bash
site:*.com
```

## Dump ENV variables to JSON using Python

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

## Kill a process and make it look like an accident

Script to inject an exit(0) syscall into a running process. NB: only x86_64 for now!

```bash
gdb -p "$1" -batch -ex 'set {short}$rip = 0x050f' -ex 'set $rax=231' -ex 'set $rdi=0' -ex 'cont'
```

## Run an ad-hoc web server

Each of these commands will run an ad hoc http static server in your current (or specified) directory, available at [http://localhost:8000](http://localhost:8000) Use this power wisely.

Python

```bash
python -m http.server 8000
```

Ruby

```bash
ruby -run -ehttpd . -p8000
```

Node.js

```bash
npm install -g http-server 
http-server -p 8000
```

Or

```bash
npm install -g node-static
static -p 8000
```

PHP

```bash
php -S 127.0.0.1:8000
```

Busybox

```bash
busybox httpd -f -p 8000
```

## Latency comparison

Latency Comparison Numbers (~2012)

```bash
L1 cache reference                           0.5 ns
Branch mispredict                            5   ns
L2 cache reference                           7   ns                      14x L1 cache
Mutex lock/unlock                           25   ns
Main memory reference                      100   ns                      20x L2 cache, 200x L1 cache
Compress 1K bytes with Zippy             3,000   ns        3 us
Send 1K bytes over 1 Gbps network       10,000   ns       10 us
Read 4K randomly from SSD*             150,000   ns      150 us          ~1GB/sec SSD
Read 1 MB sequentially from memory     250,000   ns      250 us
Round trip within same datacenter      500,000   ns      500 us
Read 1 MB sequentially from SSD*     1,000,000   ns    1,000 us    1 ms  ~1GB/sec SSD, 4X memory
Disk seek                           10,000,000   ns   10,000 us   10 ms  20x datacenter roundtrip
Read 1 MB sequentially from disk    20,000,000   ns   20,000 us   20 ms  80x memory, 20X SSD
Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms
```

Notes

```bash
1 ns = 10^-9 seconds
1 us = 10^-6 seconds = 1,000 ns
1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns
```

## Example async function JavaScript

I keep getting stuck with this kind of thing in JavaScript and I got this example to help.

```js
async function geticons() {
  const response = await fetch(
    "https://raw.githubusercontent.com/EddieHubCommunity/LinkFree/main/src/config/links.json",
    {
      method: "GET",
      mode: "cors",
    }
  );
  const data = await response.json();
  console.log(data);
}

geticons();
```
