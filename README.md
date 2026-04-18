# Simple Chess App

A basic web-based chess game with drag-select moves, validation for major pieces, turn-based play.

## Local Run
1. Open `chess.html` (recommended, table-based, guaranteed clicks).
2. Click piece → green valid moves → click target. index.html canvas version fallback.
3. Reset button.

## Docker Run (Local)
```bash
docker build -t chess-app .
docker run -p 8080:80 chess-app
```
Access: http://localhost:8080

## Deploy to EC2
1. Launch EC2 instance (Amazon Linux 2023, t2.micro, enable HTTP 80 in Security Group).
2. SSH: `ssh -i your-key.pem ec2-user@ec2-public-dns`
3. Install Docker:
   ```
   sudo yum update -y
   sudo yum install docker -y
   sudo service docker start
   sudo usermod -a -G docker ec2-user
   ```
   Logout/login or `newgrp docker`.
4. Copy files:
   ```
   # From local: scp -i your-key.pem -r ./* ec2-user@ec2-public-dns:~/
   cd chess
   docker build -t chess-app .
   docker run -d -p 80:80 --restart always --name chess chess-app
   ```
5. Access: http://ec2-public-ip

## Features
- 8x8 board with Unicode pieces.
- Basic move validation (pawn, knight, rook, bishop, queen, king).
- Turn switching.
- No castling/en passant/checkmate (simple version).

Enjoy!

