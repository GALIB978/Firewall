# Linux Server Firewall Configuration Guide
## Overview
This guide details the process of configuring a firewall on a Linux server to secure it against unauthorized access, network attacks, and other threats. The configuration includes allowing essential services, logging network traffic, and applying security measures to mitigate potential attacks.

## Objectives
- Secure the server by allowing only necessary services (SSH, HTTP, HTTPS).
- Implement logging to track allowed and denied traffic.
- Apply measures to prevent SYN flood and brute-force SSH attacks.
- Ensure only authorized connections are allowed into the server.

# Firewall Configuration Steps
## 1. Set Default Firewall Policies

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
### Explanation:
The default policies are set to deny all incoming connections and allow all outgoing traffic. This secures the server by blocking any unsolicited incoming traffic while enabling unrestricted communication from the server.

## 2. Allow SSH (OpenSSH) Access
```
sudo ufw allow OpenSSH
```
#### Explanation:
This rule enables SSH access on port 22, allowing administrators to remotely manage the server. SSH is a vital service for remote access and management.

## 3. Allow HTTP and HTTPS Traffic
```
sudo ufw allow 80/tcp    # HTTP
sudo ufw allow 443/tcp   # HTTPS
```
### Explanation:
These rules open port 80 (HTTP) and port 443 (HTTPS) for incoming web traffic. This is essential for serving websites or web applications hosted on the server.

## 4. Enable Firewall Logging
```
sudo ufw logging medium
```
### Explanation:
Logging is activated to capture details of both allowed and blocked traffic. This helps monitor potential security threats and provides useful data for troubleshooting.

## 5. Mitigate SYN Flood Attacks
```
sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT
```
### Explanation:
This iptables rule helps defend against SYN flood attacks, which can overwhelm a server with a large number of incomplete TCP connection requests. The limit restricts the number of SYN packets the server will accept per second.

## 6. Use Fail2Ban to Prevent Brute-Force SSH Attacks

### Install Fail2Ban
```
sudo apt install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```
### Explanation:
Fail2Ban is installed and enabled to automatically detect and block IP addresses with excessive failed login attempts, thereby preventing brute-force attacks on SSH.

### Configure Fail2Ban SSH Protection
```
[sshd]
enabled = true
port = ssh
maxretry = 5
findtime = 10m
bantime = 1h
```
### Explanation:
This configuration limits the number of failed SSH login attempts to 5 within 10 minutes. If an IP exceeds this limit, it is banned for 1 hour. This helps protect the server from brute-force login attempts.

## 7. Enable the Firewall
```
sudo ufw enable
```
### Explanation:
Enabling UFW applies all the firewall rules and activates the firewall, effectively enforcing the security measures defined in the previous steps.

# Summary
By following these steps, the server is now secured with essential firewall rules that allow SSH for remote access, HTTP and HTTPS for web traffic, and protection against SYN flood attacks and brute-force SSH login attempts. Regular monitoring and updates are recommended to maintain optimal security.





































































