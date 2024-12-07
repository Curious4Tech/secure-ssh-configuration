# secure-ssh-configuration
A step-by-step guide to securing SSH servers, including best practices like key-based authentication, disabling root login, changing default ports, enabling 2FA, and more. Perfect for system administrators and developers looking to harden their SSH setup.

---

# Secure SSH Configuration Guide

This guide walks you through securing SSH on your server. Follow these steps to protect your server from unauthorized access and attacks.

---

## **Step 1: Install and Enable SSH**
### Installing SSH
1. Update and upgrade your package manager:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
![image](https://github.com/user-attachments/assets/62aeddb9-bb42-487d-b012-1426358330de)

2. Install the OpenSSH server:
   ```bash
   sudo apt install openssh-server -y
   ```
![image](https://github.com/user-attachments/assets/f304c9c0-40cf-4937-9c97-0f3ce538bf96)

### Enabling and Starting SSH
1. Enable the SSH service to start on boot:
   ```bash
   sudo systemctl enable ssh
   ```
2. Start the SSH service:
   ```bash
   sudo systemctl start ssh
   ```

![image](https://github.com/user-attachments/assets/0c0bb7bd-6c81-4689-bc61-56ee13ba89cc)

3. Check the status of the SSH service:
   ```bash
   sudo systemctl status ssh
   ```

![image](https://github.com/user-attachments/assets/5b09b722-8bb2-46c8-baf5-f5575e076065)

---

## **Step 2: Use SSH Keys**
1. Generate an SSH key pair on your local machine:
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
2. Copy the public key to the server:
   ```bash
   ssh-copy-id username@server_ip
   ```
3. Disable password authentication for enhanced security (explained below).

---

## **Step 3: Secure SSH Configuration**

### Edit the SSH Configuration File
1. Open the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

### Key Security Steps:
- **Disable Root Login**:
  ```plaintext
  PermitRootLogin no
  ```
- **Disable Password Authentication**:
  ```plaintext
  PasswordAuthentication no
  ```
- **Enable Public Key Authentication**:
  ```plaintext
  PubkeyAuthentication yes
  ```
- **Change the Default SSH Port** (e.g., `2222`):
  ```plaintext
  Port 2222
  ```
  
### Restart SSH Service
```bash
sudo systemctl restart ssh
```

---

## **Step 4: Configure Firewall**
Allow the new SSH port:
```bash
sudo ufw allow 2222
sudo ufw reload
```

---

## **Step 5: Enable Fail2Ban**
1. Install Fail2Ban:
   ```bash
   sudo apt install fail2ban -y
   ```
2. Configure Fail2Ban:
   ```bash
   sudo nano /etc/fail2ban/jail.local
   ```
3. Add the following under `[sshd]`:
   ```plaintext
   [sshd]
   enabled = true
   port = 2222
   logpath = /var/log/auth.log
   maxretry = 5
   ```
4. Restart Fail2Ban:
   ```bash
   sudo systemctl restart fail2ban
   ```

---

## **Step 6: Allow Specific Users or Groups**
1. Edit the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Add the following to allow specific users:
   ```plaintext
   AllowUsers your_username
   ```
   Or, allow specific groups:
   ```plaintext
   AllowGroups sshusers
   ```
3. Restart SSH:
   ```bash
   sudo systemctl restart ssh
   ```

---

## **Step 7: Enable Two-Factor Authentication (Optional)**
1. Install the Google Authenticator PAM module:
   ```bash
   sudo apt install libpam-google-authenticator -y
   ```
2. Set up the authenticator for your user:
   ```bash
   google-authenticator
   ```
3. Enable 2FA in the SSH configuration file:
   - Edit:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Add:
     ```plaintext
     ChallengeResponseAuthentication yes
     ```
4. Configure PAM:
   ```bash
   sudo nano /etc/pam.d/sshd
   ```
   Add:
   ```plaintext
   auth required pam_google_authenticator.so
   ```
5. Restart SSH:
   ```bash
   sudo systemctl restart ssh
   ```

---

## **Step 8: Monitor and Update**
1. Check SSH logs:
   ```bash
   sudo tail -f /var/log/auth.log
   ```
2. Regularly update your server:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

---

## **Conclusion**
By following these steps, your SSH server will be significantly more secure, reducing the risk of unauthorized access.

For any issues or improvements, feel free to open an issue or submit a pull request. 😊
