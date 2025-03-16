# Firewall Configuration Report
## Firewall Rules
### Allow Necessary Services
# Allow SSH connections (port 22) - Required for remote access
```
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
# Allow HTTP (port 80) and HTTPS (port 443) traffic
```
sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 80 -m conntrack --ctstate ESTABLISHED -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 443 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
### Logging Blocked Connections
```
# Log all dropped packets
sudo iptables -A INPUT -j LOG --log-prefix "[IPTables-Dropped] " --log-level 4
```







