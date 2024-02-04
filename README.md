# Appwrite self Hosting Setup in Ubuntu 22 for Raspberry Pi 4
---

## Ubuntu 22 Setup for Raspberry Pi 4

### Flash Ubuntu 22 Server 64-bit to SD Card

Follow these steps to flash Ubuntu 22 Server 64-bit to an SD card for Raspberry Pi 4:

1. Download Ubuntu 22 Server 64-bit image.
2. Use Etcher or Raspberry Pi Imager to flash the image to the SD card.

### SSH Setup with Different Port and Key File

Generate an SSH key pair and configure SSH with a different port and key file:

1. Generate SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 2048 -f my_ssh_key
   ```

2. Convert private key to PEM format:
   ```bash
   ssh-keygen -p -m PEM -f my_ssh_key
   ```

3. Set permissions for private key:
   ```bash
   chmod 600 my_ssh_key
   ```

4. Copy public key to server:
   ```bash
   ssh-copy-id -i my_ssh_key.pub user@hostname
   ```

5. Update SSH server configuration:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
   Set `Port` to desired port number and `PasswordAuthentication` to `no`.

6. Restart SSH service:
   ```bash
   sudo systemctl restart ssh
   ```

### Firewall Configuration

Set up firewall rules using `ufw`:

1. Allow incoming connections on the new SSH port (e.g., 321):
   ```bash
   sudo ufw allow 321/tcp
   ```

2. Deny incoming connections on the default SSH port (22):
   ```bash
   sudo ufw deny 22/tcp
   ```

3. Enable firewall:
   ```bash
   sudo ufw enable
   ```

4. Check firewall status:
   ```bash
   sudo ufw status
   ```

### Docker Installation Setup

Install Docker and Docker Compose:

```bash
sudo apt install docker.io
sudo apt install docker-compose
```

### Appwrite Setup

Install and configure Appwrite using Docker:

```bash
sudo docker run -it --rm \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    --volume "$(pwd)"/appwrite:/usr/src/code/appwrite:rw \
    --entrypoint="install" \
    appwrite/appwrite:1.4.13
```

### Nginx Reverse Proxy Setup

Install Nginx and configure it as a reverse proxy:

```bash
sudo apt update
sudo apt install nginx
sudo ufw allow 'Nginx HTTP'
systemctl status nginx
```

Follow the instructions in [this guide](https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-as-a-reverse-proxy-on-ubuntu-22-04) to configure Nginx as a reverse proxy.

### DNS ddclient Setup

Install and configure ddclient for dynamic DNS updates:

```bash
sudo apt update && sudo apt install ddclient
```

Follow the instructions in the following references to set up ddclient for dynamic DNS updates:
1. [Google Domains Dynamic DNS with Google Domains](https://cloud-jake.medium.com/google-domains-dynamic-dns-with-google-domains-1dd0ea45c219)
2. [Dynu Dynamic DNS with DDClient](https://www.dynu.com/DynamicDNS/IPUpdateClient/DDClient)

### Appwrite .env File Configuration for Gmail SMTP Setup

Configure Appwrite's `.env` file for Gmail SMTP setup:

1. Enable 2-factor authentication and create an app password in Gmail account settings.
2. Edit the `.env` file:
   ```bash
   sudo nano /path/to/appwrite/.env
   ```
3. Update the following parameters in the `.env` file:
   ```yaml
   _APP_SMTP_HOST=smtp.gmail.com
   _APP_SMTP_PORT=465
   _APP_SMTP_SECURE=ssl
   _APP_SMTP_USERNAME=Your_full_Gmail_address@gmail.com
   _APP_SMTP_PASSWORD=your_email_app_password
   ```

### Restart Appwrite

After saving the `.env` file, restart Appwrite:

```bash
sudo docker-compose up -d
```

Reference documents for SMTP email setup: [Appwrite Self-Hosting - Email](https://appwrite.io/docs/advanced/self-hosting/email)

---
