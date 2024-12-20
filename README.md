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

![image](https://github.com/user-attachments/assets/689372b7-72d2-472b-b598-694607d4fdbb)

2. Copy the public key to the server:
   ```bash
   ssh-copy-id username@server_ip
   ```

![image](https://github.com/user-attachments/assets/bdde71df-6b44-4068-84a7-dff95c3f2f9c)

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
![image](https://github.com/user-attachments/assets/3de39ac8-8782-40a4-989a-86275b4dae78)

- **Disable Password Authentication**:
  ```plaintext
  PasswordAuthentication no
  ```
![image](https://github.com/user-attachments/assets/062535a8-5d7a-49ca-83eb-1a757487efe9)

- **Enable Public Key Authentication**:
  ```plaintext
  PubkeyAuthentication yes
  ```
![image](https://github.com/user-attachments/assets/3cf08288-f939-401e-996e-9db5730a7a12)

- **Change the Default SSH Port** (e.g., `2265`):
  ```plaintext
  Port 2265
  ```
![image](https://github.com/user-attachments/assets/05f2bdd8-1783-4573-9897-549a3f5f78d6)

### Restart SSH Service
```bash
sudo systemctl restart ssh
```

---

## **Step 4: Configure Firewall**
Allow the new SSH port:
```bash
sudo ufw allow 2265
sudo ufw reload
```
![image](https://github.com/user-attachments/assets/4b4a1fe1-dd2f-4b38-9cb4-9a89256f469d)

---

## **Step 5: Enable Fail2Ban**
1. Install Fail2Ban:
   ```bash
   sudo apt install fail2ban -y
   ```
![image](https://github.com/user-attachments/assets/ed6549c0-6381-45aa-8f97-79f8a82d3834)

2. Configure Fail2Ban:
   ```bash
   sudo nano /etc/fail2ban/jail.conf
   ```
3. Add the following under `[sshd]`:
   ```plaintext
   [sshd]
   enabled = true
   port = ssh
   logpath = /var/log/auth.log
   maxretry = 5
   ```
![image](https://github.com/user-attachments/assets/1a95dedf-ea25-43a3-9a65-4f17a8e9c2e4)

4. Restart Fail2Ban:
   ```bash
   sudo systemctl restart fail2ban
   sudo systemctl status fail2ban
   ```
![image](https://github.com/user-attachments/assets/2dad24de-187c-409f-a808-ae0989d5501c)

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
![image](https://github.com/user-attachments/assets/af88b08a-0920-4d48-9630-8081151e0563)

   Or, allow specific groups:
   ```plaintext
   AllowGroups your_groupname
   ```
3. Restart SSH:
   ```bash
   sudo systemctl restart ssh
   sudo systemctl status ssh
   ```
![image](https://github.com/user-attachments/assets/ebbede0f-d331-4534-983a-d64eb1f4a33d)

---

## **Step 7: Enable Two-Factor Authentication (Optional)**
1. Install the Google Authenticator PAM module:
   ```bash
   sudo apt install libpam-google-authenticator -y
   ```
![image](https://github.com/user-attachments/assets/5e4a11b2-62ef-42bc-b13c-cfb78194166e)

2. Set up the authenticator for your user:
   ```bash
   google-authenticator
   ```
![image](https://github.com/user-attachments/assets/09b1db4e-a070-4195-8675-61d8f4ae968f)

 - Follow and read carefully and then respond according to your desired configuratiom.

![image](https://github.com/user-attachments/assets/a0257644-abd5-4459-ba31-e4e03d51ec28)


3. Enable 2FA in the SSH configuration file:
   - Edit:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Add:
     ```plaintext
     AuthenticationMethods publickey,keyboard-interactive:pam
     ChallengeResponseAuthentication yes

     ```
![image](https://github.com/user-attachments/assets/0a7a66b3-167f-4f6d-b524-82d4fb9ca35f)


   - Retart the service:
       ```
       sudo systemctl restart sshd
       ```

4. Configure PAM:
   ```bash
   sudo nano /etc/pam.d/sshd
   ```
   Add:
   ```plaintext
   auth required pam_google_authenticator.so
   auth sufficient pam_unix.so
   ```
![image](https://github.com/user-attachments/assets/061042fd-c008-44f7-affc-65a19d014672)

5. Restart SSH:
   ```bash
   sudo systemctl restart ssh
   ```
6. Verify the configuration

![image](https://github.com/user-attachments/assets/90ea3113-6714-4a26-82ba-7945730d3204)

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
