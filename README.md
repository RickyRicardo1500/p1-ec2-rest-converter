# EC2 REST Converter: Pounds → Kilograms

## Overview
A minimal REST API deployed on AWS EC2 that converts pounds (lbs) to kilograms (kg).

## Setup Steps
1. Select **Amazon Linux 2023** EC2 instance (t3.micro).
2. Create a new key pair or reuse a created key from the drop down menu.
3. Allow SSH(22) traffic from my IP, allow HTTPS(443) and HTTP(80) from Anywhere IPv4, and allow custom TCP with Port 8080 from Anywhere IPv4
4. Launch the instance using the Pulbic IPv4 address
5. SSH into the instance:
   ```bash
   ssh -i MyKeyPair.pem ec2-user@<PUBLIC_IP>
   ```
6. Clone Git Repo & change working directory
   ```bash
   git clone https://github.com/RickyRicardo1500/p1.git && cd p1
   ```
7. Update & install Node.js
   ```bash
   sudo yum update -y && sudo yum install -y nodejs npm
   ```
8. Initialize Node Package Manager
   ```bash
   npm init -y
   ```
9. Install Express & Morgan
   ```bash
   npm install express morgan
   ```
10. Copy the entire command below into the bash shell
   ```bash
   cat > server.js <<'EOF'
   const express = require('express');
   const morgan = require('morgan');
   const app = express();
   app.use(morgan('combined'));
   app.get('/convert', (req, res) => {
   const lbs = Number(req.query.lbs);
   if (req.query.lbs === undefined || Number.isNaN(lbs)) {
   return res.status(400).json({ error: 'Query param lbs is required and must be a number' });
   }
   if (!Number.isFinite(lbs) || lbs < 0) {
   return res.status(422).json({ error: 'lbs must be a non-negative, finite number' });
   }
   const kg = Math.round(lbs * 0.45359237 * 1000) / 1000;
   return res.json({ lbs, kg, formula: 'kg = lbs * 0.45359237' });
   });
   const port = process.env.PORT || 8080;
   app.listen(port, () => console.log(`listening on ${port}`));
   EOF
   ```
11. Start Node.js
   ```bash
   node server.js
   ```
12. Create a systemd unit
    ```bash
    sudo bash -c 'cat > /etc/systemd/system/p1.service <<"UNIT"
   [Unit]
   Description=CS554 Project 1 service
   After=network.target
   
   [Service]
   User=ec2-user
   WorkingDirectory=/home/ec2-user/p1
   ExecStart=/usr/bin/node /home/ec2-user/p1/server.js
   Restart=always
   Environment=PORT=8080
   
   [Install]
   WantedBy=multi-user.target
   UNIT'
   ```

```bash
sudo systemctl daemon-reload
```
```bash
sudo systemctl enable --now p1
```
```bash
sudo systemctl status p1 --no-pager
```


   


