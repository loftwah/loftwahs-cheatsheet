# Loftwah's Cheatsheet

![LOFTWAH'S](https://user-images.githubusercontent.com/19922556/150899356-b3930a05-6b65-43c4-a492-f5b7e5f94b39.png)

This is a repo with a bunch of stuff I use regularly.

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

## Download the latest release from GitHub

```bash
curl -s https://api.github.com/repos/jgm/pandoc/releases/latest \
| grep "browser_download_url.*deb" \
| cut -d : -f 2,3 \
| tr -d \" \
| wget -qi -
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
