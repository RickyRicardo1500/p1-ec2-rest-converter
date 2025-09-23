# EC2 REST Converter: Pounds → Kilograms

## Overview
A minimal REST API deployed on AWS EC2 that converts pounds (lbs) to kilograms (kg).

## Setup Steps
1. Select **Amazon Linux 2023** EC2 instance (t3.micro).
2. Create a new key pair or reuse a created key from the drop down menu.
3. Allow SSH(22) traffic from Anywhere, also allow HTTPS(443) and HTTP(80) traffic from the internet
4. Launch the instance using the Pulbic IPv4 address
5. SSH into the instance:
   ```bash
   ssh -i MyKeyPair.pem ec2-user@<PUBLIC_IP>
   ```
6. Clone Git Repo
   ```bash
   git clone https://github.com/RickyRicardo1500/p1.git && cd p1
   ```
7. Update & install Node.js
   ```bash
   sudo yum update -y && sudo yum install -y nodejs npm
   ```
8. Make then change working directory
   ```bash
   mkdir -p ~/p1 && cd ~/p1
   ```
9. Initialize Node Package Manager
   ```bash
   npm init -y
   ```
10. Install Express & Morgan
   ```bash
   npm install express morgan
   ```
12. Copy the entire command below into the bash shell
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
13. Start Node.js
   ```bash
   node server.js
   ```


   


