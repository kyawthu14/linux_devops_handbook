# Phase 4: Networking & Security

This file contains the commands, explanations, warnings, and verification questions we covered during Phase 4.

---

## 4.1 Networking Basics

DevOps/SRE engineers configure, diagnose, and test connections between multiple distributed systems.

### Commands
* **`ip a`** (or `ip address`): Lists all active network interfaces and their assigned IP addresses.
  * Loopback interface is named `lo` and always has the IP `127.0.0.1`.
* **`ss`** (Socket Statistics): Inspects active and listening sockets on the server.
  * **`sudo ss -tulpn`:** (DevOps Standard command): Lists all listening ports and which process owns them:
    * `-t`: TCP ports
    * `-u`: UDP ports
    * `-l`: Listening ports
    * `-p`: Show Process name and PID (requires `sudo`)
    * `-n`: Show numeric port numbers (e.g. `80` instead of `http`)
  * `sudo ss -tulpn | grep :80` (finds what program is using port 80).
* **`ping -c 4 host`:** Sends 4 packets to test basic network connectivity.
* **`traceroute host`** (or `tracepath`): Shows the routing hops packets take to reach the host.
* **`curl URL`:** Transfers data from/to a server. Fetches webpage source.
  * **`curl -I URL`:** Prints **only the HTTP headers** (helpful for checking status codes like `200 OK` or `301 Redirect` and server details).
* **`wget URL`:** Direct file downloader.

---

## 4.2 Firewalls & Securing Ports

Ubuntu uses **UFW** (Uncomplicated Firewall) to manage port permissions securely.

### Commands
* **`sudo ufw status`:** Checks if the firewall is active or inactive.
* **`sudo ufw status numbered`:** Lists firewall rules numbered for easy deletion.
* **`sudo ufw show added`:** Lists configured rules, even if the firewall is currently inactive.
* **`sudo ufw default deny incoming`:** Blocks all incoming requests by default (security baseline).
* **`sudo ufw default allow outgoing`:** Allows all outgoing requests by default.
* **`sudo ufw allow 80`:** Opens port 80 (HTTP) to everyone.
* **`sudo ufw allow 443/tcp`:** Opens port 443 (HTTPS) specifically for TCP traffic.
* **`sudo ufw deny 21`:** Blocks port 21 (FTP).
* **`sudo ufw allow from 10.0.0.15 to any port 5432`:** Opens database port 5432 *only* to the trusted IP `10.0.0.15`.
* **`sudo ufw delete allow ssh`:** Deletes the rule allowing SSH.
* **`sudo ufw enable`:** Turns the firewall on.
* **`sudo ufw disable`:** Turns the firewall off.

### ⚠️ CRITICAL WARNING: The SSH Lockout
Before enabling the firewall on a remote server, you **must** allow SSH first:
```bash
sudo ufw allow ssh
```
If you enable UFW without this, you will instantly get locked out of your server and lose connection permanently.

---

## 4.3 SSH Administration & Security

SSH (Secure Shell) is the cryptographic protocol used to securely connect to remote servers.

### Key Authentication
* **Private Key** (e.g. `id_ed25519`): Stored **only** on your local machine. Permissions must be strict (`-rw-------` or `chmod 600`). Never share or upload this file!
* **Public Key** (e.g. `id_ed25519.pub`): Shared with remote servers.
* **Remote Path:** Paste your public key inside **`~/.ssh/authorized_keys`** on the remote server to allow connection.

### Commands
* **`ssh-keygen -t ed25519`:** Generates a secure modern SSH keypair.

### Local SSH Config File (`~/.ssh/config`)
Configure connection shortcuts on your local laptop to avoid typing long commands:
```text
Host prod-web
    HostName 18.25.10.4
    User ubuntu
    Port 22
    IdentityFile ~/.ssh/prod.pem
```
Now you can log in by running:
```bash
ssh prod-web
```

---

## Verification Questions We Covered

1. **Q:** Which command lists all network interfaces and local IP addresses?
   * **A:** `ip a`
2. **Q:** If your web server fails to start because port 80 is occupied, how do you find the PID of the blocking app?
   * **A:** Run `sudo ss -tulpn | grep :80` (requires `sudo` to view processes owned by other system users).
3. **Q:** How do you view only the HTTP response headers of a website?
   * **A:** `curl -I https://google.com`
4. **Q:** If you enable the firewall without adding rules, what happens to your SSH session? How do you prevent it?
   * **A:** You get locked out because the default policy blocks all incoming traffic. Prevent this by running `sudo ufw allow ssh` before enabling UFW.
5. **Q:** How do you allow HTTP (port 80) and HTTPS (port 443) traffic in UFW?
   * **A:** Run separate commands: `sudo ufw allow 80` and `sudo ufw allow 443/tcp`.
6. **Q:** How do you secure a database on port 5432 so only IP `10.0.0.15` can access it?
   * **A:** `sudo ufw allow from 10.0.0.15 to any port 5432`
7. **Q:** Where do you save the public key on the remote server?
   * **A:** In the file `~/.ssh/authorized_keys`.
8. **Q:** **True or False:** Is it secure to copy your private key (`id_ed25519`) to remote servers to allow them to connect to other servers?
   * **A:** False. If a server is compromised, attackers get your private key. Use SSH Agent Forwarding instead.
