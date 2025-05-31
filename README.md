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

---

## Benefits

- **Secure encrypted proxy** (VLESS + TLS)  
- **Stable hostname** with dynamic IP updates (No-IP)  
- **Cloud-based, scalable** and accessible globally  
- **Free tier friendly** (AWS free tier + No-IP free tier)  
- Open-source software stack for full control  

---

## Limitations

- Requires some Linux command line familiarity  
- Self-signed TLS certificates cause warnings unless replaced with Let’s Encrypt  
- Dynamic DNS propagation may take a few minutes  
- AWS free tier limits resources and usage  

---

# Project File Structure


