# secure-ssh-configuration
A step-by-step guide to securing SSH servers, including best practices like key-based authentication, disabling root login, changing default ports, enabling 2FA, and more. Perfect for system administrators and developers looking to harden their SSH setup.


# Secure SSH Configuration Guide

Securing SSH is essential to protect your servers from unauthorized access. This guide provides step-by-step instructions to enhance the security of your SSH server.

---

## 1. Use Strong Passwords or Disable Password Authentication
- Ensure all user accounts have strong passwords or disable password authentication.

### Steps:
1. Open the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Disable password authentication:
   ```plaintext
   PasswordAuthentication no
   ```
3. Enable public key authentication:
   ```plaintext
   PubkeyAuthentication yes
   ```
4. Save the file and restart SSH:
   ```bash
   sudo systemctl restart sshd
   ```

---

## 2. Use SSH Keys
### Steps:
1. Generate SSH keys on your client machine:
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
2. Copy the public key to the server:
   ```bash
   ssh-copy-id username@server_ip
   ```
3. Test the login and disable password login as mentioned above.

---

## 3. Change the Default SSH Port
### Steps:
1. Choose a custom port (e.g., `2222`) and edit the SSH configuration:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Update the port:
   ```plaintext
   Port 2222
   ```
3. Restart SSH:
   ```bash
   sudo systemctl restart sshd
   ```
4. Update firewall rules:
   ```bash
   sudo ufw allow 2222
   sudo ufw reload
   ```

---

## 4. Enable SSH Rate Limiting
Limit SSH connection attempts using UFW:
```bash
sudo ufw limit 2222/tcp
```

---

## 5. Disable Root Login
### Steps:
1. Edit the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Disable root login:
   ```plaintext
   PermitRootLogin no
   ```
3. Restart SSH:
   ```bash
   sudo systemctl restart sshd
   ```

---

## 6. Use Fail2Ban
### Steps:
1. Install Fail2Ban:
   ```bash
   sudo apt install fail2ban
   ```
2. Configure Fail2Ban for SSH:
   - Edit or create `/etc/fail2ban/jail.local`:
     ```plaintext
     [sshd]
     enabled = true
     port = 2222
     logpath = /var/log/auth.log
     maxretry = 5
     ```
   - Restart Fail2Ban:
     ```bash
     sudo systemctl restart fail2ban
     ```

---

## 7. Allow Only Specific Users or Groups
### Steps:
1. Edit the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Add allowed users or groups:
   ```plaintext
   AllowUsers your_username
   # Or
   AllowGroups sshusers
   ```
3. Restart SSH:
   ```bash
   sudo systemctl restart sshd
   ```

---

## 8. Use Two-Factor Authentication (2FA)
### Steps:
1. Install Google Authenticator:
   ```bash
   sudo apt install libpam-google-authenticator
   ```
2. Set up the authenticator:
   ```bash
   google-authenticator
   ```
3. Enable PAM in SSH:
   - Edit the SSH configuration file:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
     Add:
     ```plaintext
     ChallengeResponseAuthentication yes
     ```
   - Edit `/etc/pam.d/sshd`:
     ```plaintext
     auth required pam_google_authenticator.so
     ```
4. Restart SSH:
   ```bash
   sudo systemctl restart sshd
   ```

---

## 9. Use an SSH Bastion Host
Set up an intermediate bastion host to act as a gateway for SSH connections to other servers.

---

## 10. Regularly Update and Monitor
### Steps:
1. Keep your system updated:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Monitor SSH logs for unusual activities:
   ```bash
   sudo tail -f /var/log/auth.log
   ```

---

By following these steps, your SSH server will be significantly more secure against unauthorized access.

---

### License
This guide is open-source and free to use under the MIT License.

---
```

You can copy this content to a `README.md` file for your GitHub repository!
