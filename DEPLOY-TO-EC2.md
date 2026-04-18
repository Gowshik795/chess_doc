# Deploy Chess App to AWS EC2

## Prerequisites
- AWS account (free tier eligible)
- SSH key pair (.pem file)
- Docker Hub image OR local files (using Dockerfile)

## Step-by-Step Deployment

### 1. Launch EC2 Instance
1. AWS Console → EC2 → **Launch Instance**
2. **Name**: `chess-app`
3. **AMI**: Amazon Linux 2023 (free tier)
4. **Instance Type**: `t2.micro` (free tier)
5. **Key Pair**: Create/download `.pem` or use existing
6. **Network Settings**:
   - Allow **HTTP (port 80)** & **SSH (port 22)** in Security Group
   - Source: Anywhere (0.0.0.0/0) **⚠️ For testing only**
7. **Launch** → Note **Public IPv4** (e.g., 3.123.45.67)

### 2. SSH to Instance
```bash
chmod 400 your-key.pem
ssh -i your-key.pem ec2-user@ec2-xx-xx-xx-xx.compute-1.amazonaws.com
```

### 3. Install Docker
```bash
sudo yum update -y
sudo yum install docker -y
sudo service docker start
sudo usermod -aG docker ec2-user
# Logout & SSH back in
newgrp docker  # OR relogin
```

### 4. Deploy Chess App (2 Options)

**Option A: From Docker Hub (Recommended - FAST)**
```bash
docker pull yourusername/chess-app
docker run -d -p 80:80 --restart always --name chess-app yourusername/chess-app
```

**Option B: Build from source** (if files copied)
```bash
# SCP files from local: scp -i your-key.pem -r ./* ec2-user@ec2-public-ip:~/
docker build -t chess-app .
docker run -d -p 80:80 --restart always --name chess-app chess-app
```

### 5. Verify
- **URL**: http://ec2-public-ip (e.g., http://3.123.45.67)
- Auto-opens chess-final.html (complete game: checkmate!)
- Check logs: `docker logs chess-app`

### 6. Management Commands
```bash
docker stop chess-app && docker rm chess-app  # Stop
docker start chess-app  # Start
docker restart chess-app  # Restart
```

## Make Permanent (Optional)
- **Elastic IP**: EC2 → Elastic IPs → Allocate → Associate (prevents IP change)
- **Domain**: Route53 → Hosted Zone → A record → Elastic IP

## Clean Up
```
docker stop chess-app && docker rm chess-app
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
```

**🎉 Chess app live on EC2!** Public URL: http://your-ec2-public-ip/chess-final.html

**Cost**: ~$0/month (t2.micro free tier 750hrs)
