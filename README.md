# Loftwah's Cheatsheet 2025

![LOFTWAH'S](https://user-images.githubusercontent.com/19922556/150899356-b3930a05-6b65-43c4-a492-f5b7e5f94b39.png)

This repository consolidates essential tools, commands, and workflows tailored for my current tech stack, focusing on efficiency and modern best practices. Each section includes practical examples with expected outputs to help understand the tools better.

---

## Google Workspace

Google Workspace remains central to my workflow.

- [Admin Console](https://admin.google.com/ac/home?hl=en)
- [Cloud Platform](https://console.cloud.google.com/home/dashboard)
- [Drive](https://drive.google.com/drive/u/0/)
- [Gmail](https://mail.google.com/)
- [Sheets](https://sheets.google.com/)
- [How Search Works](https://www.google.com/search/howsearchworks/?fg=1)

---

## Docker

Docker simplifies containerisation and deployment. Docker Compose is now bundled with the `get.docker.com` installation script.

### Installing Docker on Ubuntu

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ${USER}
```

### Common Commands

```bash
# Start Docker daemon
sudo service docker start
# Launch a container with an interactive shell
sudo docker run -it <image-name> /bin/bash
# Access a running container shell
sudo docker exec -it <container-name> bash
# Inspect container details
sudo docker inspect <container-name>
# List running containers
sudo docker ps
# Remove all stopped containers
sudo docker rm $(sudo docker ps -aq)
# Remove unused volumes
sudo docker volume prune
```

### Practical Example: Web Application Stack

```bash
# Create project structure
mkdir my-webapp && cd my-webapp
mkdir src

# Create a test page
cat > src/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>Docker Test</title>
</head>
<body>
    <h1>Hello from Docker!</h1>
</body>
</html>
EOF

# Create compose file
cat > compose.yaml << 'EOF'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./src:/usr/share/nginx/html
  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: example
      POSTGRES_DB: myapp
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
EOF

# Start the stack
docker compose up -d

# Expected Output:
# [+] Running 2/2
# ⠿ Container my-webapp-db-1   Started
# ⠿ Container my-webapp-web-1  Started

# Check status
docker compose ps
# Expected Output:
# NAME                COMMAND                  SERVICE             STATUS              PORTS
# my-webapp-db-1      "docker-entrypoint.s…"   db                 running             5432/tcp
# my-webapp-web-1     "/docker-entrypoint.…"   web                running             0.0.0.0:8080->80/tcp

# View logs
docker compose logs
```

You can now access the web server at http://localhost:8080

---

## Terraform

Terraform allows Infrastructure as Code (IaC).

### Installation via tfenv

```bash
# Install tfenv
git clone https://github.com/tfutils/tfenv.git ~/.tfenv
sudo ln -s ~/.tfenv/bin/* /usr/local/bin

# Install Terraform using tfenv
sudo tfenv install latest
sudo tfenv use latest
```

### Practical Example: AWS S3 Static Website

```bash
# Create new project
mkdir terraform-demo && cd terraform-demo

# Create main configuration
cat > main.tf << 'EOF'
provider "aws" {
  region = "ap-southeast-2"
}

resource "aws_s3_bucket" "website" {
  bucket = "my-unique-website-2025"
}

resource "aws_s3_bucket_public_access_block" "website" {
  bucket = aws_s3_bucket.website.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_website_configuration" "website" {
  bucket = aws_s3_bucket.website.id

  index_document {
    suffix = "index.html"
  }

  error_document {
    key = "error.html"
  }
}

resource "aws_s3_bucket_policy" "website" {
  bucket = aws_s3_bucket.website.id
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid       = "PublicReadGetObject"
        Effect    = "Allow"
        Principal = "*"
        Action    = "s3:GetObject"
        Resource  = "${aws_s3_bucket.website.arn}/*"
      },
    ]
  })
}
EOF

# Initialize working directory
terraform init

# Preview changes
terraform plan

# Apply changes
terraform apply
# For automation without prompts:
terraform apply --auto-approve

# Destroy when done
terraform destroy --auto-approve
```

---

## Language Installation with mise

Streamline the installation of common languages (Ruby, Node.js, Python, etc.) using `mise`.

### Install mise

```bash
curl -fsSL https://get.mise.sh | sh
```

### Practical Example: Python Project Setup

```bash
# Create project directory
mkdir python-project && cd python-project

# Create mise configuration
cat > .mise.toml << 'EOF'
[tools]
python = "3.12"
nodejs = "20"
EOF

# Install specified versions
mise install
# Expected Output:
# Installing python@3.12.0
# Installing nodejs@20.0.0

# Activate the environment
mise use

# Verify installations
python --version
# Expected Output: Python 3.12.0

node --version
# Expected Output: v20.0.0
```

---

## Python Package Management with uv

### Install uv

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Practical Example: Managing Dependencies

```bash
# Create virtual environment
uv create myenv
# Expected Output: Created virtual environment at /path/to/myenv

# Activate environment
uv activate myenv

# Create requirements file
cat > requirements.txt << 'EOF'
fastapi==0.109.0
uvicorn==0.27.0
pydantic==2.5.3
python-dotenv==1.0.0
EOF

# Install dependencies
uv pip install -r requirements.txt
# Expected Output: Successfully installed fastapi-0.109.0 uvicorn-0.27.0 ...

# Create test application
cat > app.py << 'EOF'
from fastapi import FastAPI
from pydantic import BaseModel

# Create FastAPI instance
app = FastAPI(title="My API", version="1.0.0")

# Define a data model
class Item(BaseModel):
    name: str
    description: str | None = None
    price: float

# Create some endpoints
@app.get("/")
async def root():
    return {"message": "Hello from FastAPI!"}

@app.post("/items/")
async def create_item(item: Item):
    return {"item": item, "status": "created"}

EOF

# Run application
uvicorn app:app --reload
# Access API docs at http://127.0.0.1:8000/docs
```

---

## Bun

Bun replaces `npm` and `npx` for a faster and modern JavaScript runtime and package manager.

### Install Bun

```bash
# Via mise
mise install bun

# Or via curl
curl -fsSL https://bun.sh/install | bash
```

### Practical Example: Next.js Project

```bash
# Create new Astro project
bunx create astro@latest my-app
cd my-app

# Install dependencies
bun install
# Expected Output: XX packages installed [XXX.XXms]

# Add integrations (example with Tailwind)
bunx astro add tailwind
# Answer the prompt with 'y' to proceed

# Development commands
bun dev        # Start development server
bun preview    # Preview production build
bun build      # Build for production
bun astro sync # Sync content types

# Common integrations
bun astro add react    # Add React support
bun astro add vercel   # Add Vercel adapter
bun astro add mdx      # Add MDX support

# Create new component
cat > src/components/Counter.astro << 'EOF'
---
let count = 0;
---
<button id="counter">
  Count: <span>{count}</span>
</button>

<script>
  const button = document.getElementById('counter');
  let count = 0;

  button?.addEventListener('click', () => {
    count++;
    if (button.querySelector('span')) {
      button.querySelector('span')!.textContent = count.toString();
    }
  });
</script>
EOF
```

---

## TailwindCSS

Simplifying frontend design.

### Practical Example: Setting up Tailwind

```bash
# Create new project
mkdir tailwind-project && cd tailwind-project

# Initialize package.json
bun init -y

# Install Tailwind and dependencies
bun add -d tailwindcss postcss autoprefixer
bunx tailwindcss init

# Create configuration
cat > tailwind.config.mts << 'EOF'
import { defineConfig } from 'tailwindcss';

export default defineConfig({
  content: ['./src/**/*.{html,js,ts,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: '#3B82F6',
      },
    },
  },
  plugins: [],
});
EOF

# Create source directory
mkdir -p src/styles

# Create CSS file
cat > src/styles/tailwind.css << 'EOF'
@tailwind base;
@tailwind components;
@tailwind utilities;
EOF

# Create sample HTML
cat > src/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>Tailwind Project</title>
    <link href="./styles/output.css" rel="stylesheet">
</head>
<body class="bg-gray-100">
    <div class="container mx-auto px-4 py-8">
        <h1 class="text-4xl font-bold text-primary">
            Hello Tailwind!
        </h1>
    </div>
</body>
</html>
EOF

# Add build script to package.json
cat > package.json << 'EOF'
{
  "scripts": {
    "build": "tailwindcss -i ./src/styles/tailwind.css -o ./src/styles/output.css",
    "watch": "tailwindcss -i ./src/styles/tailwind.css -o ./src/styles/output.css --watch"
  }
}
EOF

# Build CSS
bun run build
```

---

## Nginx

Reliable reverse proxy with support for SSL via Let's Encrypt.

### Configure Reverse Proxy for localhost

```bash
# Create Nginx configuration
sudo tee /etc/nginx/sites-available/myapp << 'EOF'
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
EOF

# Enable configuration
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
sudo nginx -t
sudo nginx -s reload
```

### Add SSL with Let's Encrypt and Certbot

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d example.com -d www.example.com

# Test SSL renewal
sudo certbot renew --dry-run

# Reload Nginx after config changes
sudo nginx -s reload
```

### SSL Configuration Example

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name example.com www.example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

---

## AWS CLI

Automating infrastructure tasks.

### Common S3 Commands

```bash
# Create new bucket in Sydney region
aws s3 mb s3://my-sydney-bucket-2025 --region ap-southeast-2
# Expected Output: make_bucket: my-sydney-bucket-2025

# Create a test file
echo "This is a test file for S3 upload" > test.txt

# Upload file with metadata and explicit region
aws s3 cp test.txt s3://my-sydney-bucket-2025/uploads/test.txt \
    --region ap-southeast-2 \
    --metadata "project=demo,environment=test" \
    --content-type "text/plain"
# Expected Output: upload: ./test.txt to s3://my-sydney-bucket-2025/uploads/test.txt

# Verify metadata
aws s3api head-object \
    --bucket my-sydney-bucket-2025 \
    --key uploads/test.txt \
    --region ap-southeast-2
# Expected Output: Shows metadata including content-type and custom metadata

# Download file from specific region
aws s3 cp s3://my-sydney-bucket-2025/uploads/test.txt \
    downloaded-test.txt \
    --region ap-southeast-2
# Expected Output: download: s3://my-sydney-bucket-2025/uploads/test.txt to ./downloaded-test.txt

# Verify contents
cat downloaded-test.txt
# Expected Output: This is a test file for S3 upload

# List bucket contents with sizes and dates
aws s3 ls s3://my-sydney-bucket-2025 --recursive --human-readable
# Expected Output: 2024-12-22 10:30:15   29 B  uploads/test.txt

# Remove file
aws s3 rm s3://my-sydney-bucket-2025/uploads/test.txt \
    --region ap-southeast-2

# Remove bucket and all contents
aws s3 rb s3://my-sydney-bucket-2025 --force \
    --region ap-southeast-2
```

### Find Ubuntu AMIs

Find Ubuntu AMIs using the [Ubuntu Cloud Image Locator](https://cloud-images.ubuntu.com/locator/ec2/).

For Sydney region (ap-southeast-2), you can filter the results accordingly.

---

## Summary

This cheatsheet combines tools and workflows aligned with my 2025 tech stack, ensuring compatibility, efficiency, and scalability in daily operations. Each section includes practical examples to demonstrate real-world usage.
