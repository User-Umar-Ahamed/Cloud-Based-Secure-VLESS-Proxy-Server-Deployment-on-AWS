# Cloud-Based Secure VLESS Proxy Server on AWS with Dynamic DNS

## Overview  
This project guides you through deploying a secure VLESS proxy server on an AWS EC2 Ubuntu instance, configuring No-IP Dynamic DNS for hostname resolution, and connecting clients securely via TLS.

You will learn how to:

- Sign up for AWS and launch an Ubuntu EC2 server  
- Set up a free dynamic DNS hostname using No-IP  
- Install and configure the Xray-core VLESS proxy server  
- Secure the connection with TLS encryption  
- Connect clients using the No-IP hostname  

---

## Why This Project?

Many users need a reliable, secure proxy server to bypass network restrictions or enhance privacy. AWS provides flexible cloud infrastructure, but its public IP can change if you stop/start the instance. Using No-IP Dynamic DNS solves this by providing a stable hostname.

VLESS protocol with TLS ensures encrypted, fast, and modern proxy connections. This setup combines the best of cloud scalability, dynamic hostname, and secure proxy protocols.

Purpose: To set up a secure VLESS (VMess-less) proxy server using Xray-core with a GUI (3X UI), hosted on AWS EC2 and dynamically linked to a custom domain using No-IP DDNS.

🔧 Step-by-Step Breakdown
🔹 Step 1: Create AWS EC2 Instance
🧠 What Happens:
You sign up for an Amazon Web Services (AWS) account and launch a virtual server (EC2 instance) running Ubuntu Linux.

You choose t2.micro, which is free under AWS Free Tier.

You open required ports via Security Groups:

22 (SSH) for secure terminal access

443 (HTTPS) and 2004 (Custom TCP for panel access)

✅ Outcome:
You get a remote Ubuntu server with open access to install your services and manage your proxy panel.

🔹 Step 2: Connect to EC2 and Install Xray-core
🧠 What Happens:
You SSH into your instance using the .pem key.

You install essential packages and Xray-core using the official script:

bash
Copy
Edit
bash <(curl -L https://raw.githubusercontent.com/XTLS/Xray-install/main/install-release.sh)
This sets up the Xray-core binary and environment on your EC2 server.

✅ Outcome:
Your server is now capable of handling encrypted VLESS protocol connections for proxy use.

🔹 Step 3: Set Up No-IP Dynamic DNS
🧠 What Happens:
AWS public IPs are not static — they change after restarts.

No-IP gives you a free dynamic hostname (e.g., teanex.zapto.org) that automatically maps to your current public IP.

✅ Outcome:
You get a stable domain name (like a website address) to connect to your proxy, even when IP changes.

🔹 Step 4: Secure with SSL Certificate (Using Certbot)
🧠 What Happens:
You install Certbot, which fetches an SSL certificate from Let's Encrypt using your hostname.

You run:

bash
Copy
Edit
certbot certonly --standalone --agree-tos --register-unsafely-without-email -d yourdomain.com
Certbot verifies your domain, then generates a public and private key for TLS encryption.

✅ Outcome:
This lets your panel and proxy run on secure HTTPS/TLS (vless + tls) instead of unencrypted HTTP.

🔹 Step 5: Configure 3X UI Panel
🧠 What Happens:
You run x-ui to launch the 3X UI setup.

You access it via browser:

arduino
Copy
Edit
https://yourdomain.com:2004/
You login using default or reset credentials and paste your SSL cert and key into Panel Settings → Certificates.

Then, you create a new inbound rule for the VLESS proxy on port 443, enable TLS, set SNI, and other parameters.

✅ Outcome:
You now have a web-based GUI to manage users, generate VLESS links, and monitor traffic stats.

🔹 Step 6: Connect Using Client App (e.g., NekoRay / Netmod)
🧠 What Happens:
You generate VLESS client config via 3X UI or copy the generated link.

You import the config into a mobile/desktop app like:

Netmod (Android)

NekoRay or v2rayNG

You connect using TLS-secured VLESS to your AWS instance.

✅ Outcome:
You have a working, encrypted, personal proxy service for bypassing censorship, enhancing privacy, or testing secure tunneling.

📦 What This Project Does
Feature	Description
✅ Secure Proxy (VLESS + TLS)	Uses the modern VLESS protocol with TLS for secure traffic tunneling.
✅ Remote GUI Panel (3X UI)	Manage configs, users, and monitor stats from a browser-based dashboard.
✅ Dynamic DNS Support	No-IP keeps your custom domain synced to changing AWS IPs.
✅ Certificate-Based Encryption	SSL certificates ensure encrypted HTTPS connections.

⚡ Benefits
🌐 Bypass network restrictions (e.g., blocked content or countries with firewall)

🔐 Encrypted traffic protects user identity and data.

💻 Portable – Works on PC, Android, and other devices.

🧪 Educational – Learn cloud deployment, DNS, SSL, Xray-core config, and proxy protocols.

⚠️ Limitations
❌ Not production-ready (self-signed or non-renewing SSL may break)

💰 AWS IP may change unless you use Elastic IPs (not free long-term)

🔓 Misconfigured security groups can expose your server

📉 Not optimized for heavy load (free-tier EC2 has limited resources)

🔒 Disclaimer
This project is for educational and personal learning purposes only.
Please do not use it for illegal activities, or in commercial or production environments.
The tools and methods shown are intended to help you understand proxy technology, encryption, and cloud configuration in a secure and responsible way.


