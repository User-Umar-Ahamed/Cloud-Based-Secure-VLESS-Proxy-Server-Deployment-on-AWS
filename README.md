# Cloud-Based Secure VLESS Proxy Server on AWS with Dynamic DNS

# 🛡️ Secure VLESS Proxy Server with AWS EC2 + 3X UI + No-IP + TLS

This project sets up a **secure VLESS proxy server** using open-source tools like **Xray-core**, **3X UI**, **AWS EC2**, **No-IP**, and **Certbot**. It's designed for **personal learning** and **privacy protection**, not commercial use.

It helps beginners understand:

- How to deploy cloud infrastructure with AWS EC2  
- How to run a proxy server using the modern **VLESS** protocol  
- How to use a free domain with **Dynamic DNS (No-IP)**  
- How to enable TLS encryption using **Let's Encrypt certificates**  
- How to configure everything easily using **a graphical admin panel (3X UI)**  

Whether you're a student, developer, or privacy-conscious user, this project gives you hands-on experience with real-world networking and security practices.

---

> **Note:** This project is meant for educational purposes only and should not be used for illegal activity or commercial deployment.
---

## 📌 Table of Contents

- [Project Purpose](#project-purpose)
- [What You'll Learn](#what-youll-learn)
- [Tools & Technologies Used](#tools--technologies-used)
- [Step-by-Step Setup Guide](#step-by-step-setup-guide)
  - [Step 1: Create AWS EC2 Ubuntu Instance](#step-1-create-aws-ec2-ubuntu-instance)
  - [Step 2: Install Xray-core and 3X UI](#step-2-install-xray-core-and-3x-ui)
  - [Step 3: Set Up Dynamic DNS with No-IP](#step-3-set-up-dynamic-dns-with-no-ip)
  - [Step 4: Install SSL Certificates using Certbot](#step-4-install-ssl-certificates-using-certbot)
  - [Step 5: Configure 3X UI Panel and Add Inbound](#step-5-configure-3x-ui-panel-and-add-inbound)
  - [Step 6: Connect from VPN Client](#step-6-connect-from-vpn-client)
- [Benefits](#benefits)
- [Limitations](#limitations)
- [Disclaimer](#disclaimer)

---

## 🎯 Project Purpose

The purpose of this project is to:
- Deploy a personal proxy server using the **VLESS** protocol (a lightweight, modern version of VMess).
- Learn how to use **cloud infrastructure**, **dynamic DNS**, **TLS certificates**, and **GUI-based management** (3X UI).
- Enhance **online privacy and secure communication** through encryption and tunneling.
- Explore how **Xray-core** powers decentralized, proxy-based internet access.

---

## 📘 What You'll Learn

- Launching a Ubuntu server on AWS EC2
- Securing it with SSH, firewall rules, and SSL certificates
- Installing and configuring **Xray-core** proxy software
- Managing users and configs through the **3X UI panel**
- Using **No-IP** to map your changing IP to a static domain
- Connecting securely using a VLESS VPN client

---

## 🔧 Tools & Technologies Used

| Tool / Service     | Purpose                                     |
|--------------------|---------------------------------------------|
| **AWS EC2**         | Cloud server hosting (Ubuntu 24.04 LTS)     |
| **Xray-core**       | Core proxy engine supporting VLESS          |
| **3X UI**           | Graphical management panel for Xray         |
| **No-IP**           | Dynamic DNS service                         |
| **Certbot**         | Free SSL certificate tool (Let's Encrypt)   |
| **Netmod / Nekoray**| Client apps for VLESS proxy connection      |

---

## 🛠️ Step-by-Step Setup Guide

---

### ✅ Step 1: Create AWS EC2 Ubuntu Instance

1. Go to [AWS Free Tier](https://aws.amazon.com/free/) and sign up.
2. Launch an EC2 instance with:
   - **Ubuntu 24.04 LTS**
   - **t2.micro** (Free Tier)
3. Configure security group to open ports:
   - `22` (SSH)
   - `80` (optional, for HTTP)
   - `443` (HTTPS/VLESS TLS)
   - `2004` (3X UI panel)
4. Download your **.pem key** and store it securely.

#### 🔍 What Happens:
You're provisioning a virtual server in the cloud with firewall settings that allow SSH access, TLS connections, and panel access.

---

### ✅ Step 2: Install Xray-core and 3X UI
  
After Creating Instance in AWS you can Connect it on Web 

### ✅ Step 2: Install Xray-core and 3X UI

Switch to root and update:

<pre><code class="language-bash">
sudo -i
apt update && apt upgrade -y
</code></pre>

Install 3X UI and Xray-core:

<pre><code class="language-bash">
bash &lt;(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh)
</code></pre>

#### 🔍 What Happens:
This script installs:

- **Xray-core** (proxy backend)  
- **3X UI** (GUI for managing VLESS, VMess, Trojan, etc.)  
- Required systemd services and firewall rules  

---

### ✅ Step 3: Set Up Dynamic DNS with No-IP

1. Go to [No-IP](https://www.noip.com/) and register.
2. On your EC2 instance you can get your public IPv4 Address
3. After login in No-IP, go to **DDNS and Remote Access** → No-IP Hostname**.    
4. Create A Hostname 
5. Enter a hostname (e.g., `myvlessproxy.ddns.net` or `myproxy.zapto.org`).
6. in IPv4 Address add your EC2 Public IP Address   
7. Set hostname type: DNS Host (A). Leave other defaults.
8. Click **Create Hostname with DDNS Key **

---

### ✅ Step 4: Install SSL Certificates using Certbot

Install Certbot:

<pre><code class="language-bash">
apt install certbot -y
</code></pre>

Run Certbot (replace `yourdomain.com` with your actual No-IP hostname):

<pre><code class="language-bash">
certbot certonly --standalone --agree-tos --register-unsafely-without-email -d yourdomain.com
</code></pre>

Locate the generated certs:

- **Private Key**: `/etc/letsencrypt/live/yourdomain.com/privkey.pem`
- **Public Key**: `/etc/letsencrypt/live/yourdomain.com/fullchain.pem`

#### 🔍 What Happens:
Certbot proves ownership of your domain and installs certificates so your proxy server can use HTTPS/TLS encryption for secure connections.

---

### ✅ Step 5: Configure 3X UI Panel and Add Inbound

1. Run `x-ui` in terminal and reset your login if needed.
2. Open `https://yourdomain.com:2025` in your browser.
3. Log in → go to **Settings** → paste the certs from Step 4 into the Certificate tab.
4. Save and restart the panel.
5. Go to **Inbounds** → click “Add” → create a new VLESS inbound with the following settings:

   - **Port**: `443`  
   - **Protocol**: `vless`  
   - **Security**: `TLS`  
   - **Domain**: your No-IP hostname  
   - **Enable TCP** (optionally with obfuscation)  
   - Add a Client → auto-generate UUID  

#### 🔍 What Happens:
This configures the server to listen for encrypted VLESS traffic on port 443, allowing clients to securely connect via TLS.

---

### ✅ Step 6: Connect from VPN Client

Use a client such as **Netmod**, **v2rayNG**, or **NekoRay**.

#### 🔍 What Happens:
Your device now tunnels internet traffic securely through your EC2 proxy using encrypted VLESS connections, bypassing local network restrictions.

---

## 🌟 Benefits

- 🔒 **Strong Encryption**: TLS + VLESS provides modern, secure tunneling.
- 🌍 **Bypass Network Restrictions**: Useful for accessing geo-blocked or censored content.
- 🧪 **Hands-On Learning**: Gain skills in cloud infrastructure, Linux, SSL, and networking.
- 🧠 **GUI Panel**: Easily manage users and protocols using the 3X UI dashboard.
- 🆓 **Free Tier Friendly**: Cost-effective when run within AWS Free Tier limits.

---

## ⚠️ Limitations

- 🧊 **Not Production-Ready**: Meant for personal use and learning only.
- 🔁 **IP May Change**: EC2 public IPs can change unless you assign a static Elastic IP.
- 🧱 **Firewall Dependency**: Misconfigured security groups may block access.
- 📈 **Resource-Constrained**: t2.micro is limited in bandwidth, CPU, and memory.

---

## ❗ Disclaimer

> This project is for **educational and personal use only**.
>
> ❌ Do **not** use it for illegal activities or content distribution.  
> ❌ Do **not** deploy it in a production or commercial environment.  
>
> The author is not responsible for **any misuse, service interruptions, or legal issues** arising from the use of this project.

---


