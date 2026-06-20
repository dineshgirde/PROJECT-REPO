# 🏍️ Bike Race Game - AWS Docker Deployment Guide

## Project Structure
```
bike-race-game/
├── index.html          # Game file
├── Dockerfile          # Docker image config
├── docker-compose.yml  # Easy run config
├── nginx.conf          # Web server config
└── DEPLOY.md           # Yeh file
```

---

## Step 1: AWS EC2 Instance Launch Karo

1. AWS Console → EC2 → "Launch Instance"
2. **AMI**: Amazon Linux 2023 (free tier eligible)
3. **Instance Type**: t2.micro (free tier)
4. **Security Group Rules** (important!):
   - SSH: Port 22 (your IP se)
   - HTTP: Port 80 (0.0.0.0/0 - sabke liye)
5. Key pair download karo (.pem file)

---

## Step 2: EC2 pe Connect Karo

```bash
chmod 400 your-key.pem
ssh -i your-key.pem ec2-user@YOUR_EC2_PUBLIC_IP
```

---

## Step 3: Docker Install Karo (EC2 pe)

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

# Docker Compose install
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Logout karke wapas login karo (group apply hone ke liye)
exit
```

---

## Step 4: Project Upload Karo

**Option A - SCP se (local machine se):**
```bash
scp -i your-key.pem -r ./bike-race-game ec2-user@YOUR_EC2_PUBLIC_IP:~/
```

**Option B - Git se (agar GitHub pe hai):**
```bash
git clone https://github.com/YOUR_USERNAME/bike-race-game.git
```

---

## Step 5: Game Run Karo!

```bash
cd bike-race-game

# Docker Compose se run karo (recommended)
docker-compose up -d

# Ya directly Docker se:
docker build -t bike-race .
docker run -d -p 80:80 --name bike-race-game bike-race
```

---

## Step 6: Game Open Karo 🎮

Browser mein open karo:
```
http://YOUR_EC2_PUBLIC_IP
```

---

## Useful Commands

```bash
# Container status check karo
docker ps

# Logs dekhna ho toh
docker logs bike-race-game

# Game stop karo
docker-compose down

# Game restart karo
docker-compose restart

# New changes ke baad rebuild karo
docker-compose up -d --build
```

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Site nahi khul raha | Security Group mein Port 80 check karo |
| Container nahi chala | `docker logs bike-race-game` se error dekho |
| Permission denied | `sudo usermod -aG docker ec2-user` kar ke logout/login karo |

---

## 💡 Pro Tips

- **Free Tier**: t2.micro pe bilkul free chalega (750 hrs/month)
- **Domain**: Route 53 se custom domain laga sakte ho
- **HTTPS**: AWS Certificate Manager + ALB se SSL free milta hai
- **ECR**: Image ko AWS Elastic Container Registry pe push kar sakte ho

---

Happy Racing! 🏍️💨
