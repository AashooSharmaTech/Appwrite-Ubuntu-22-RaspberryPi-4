# Appwrite self Hosting Setup in Ubuntu 22 for Raspberry Pi 4

---

## Table of Contents

- [Flash Ubuntu 22 Server 64-bit to SD Card](#flash-ubuntu-22-server-64-bit-to-sd-card)
- [Static IP address configuration setup](#static-ip-address-configuration-setup)
- [Port forwarding](#port-forwarding)
- [SSH Setup with Different Port and Key File](#ssh-setup-with-different-port-and-key-file)
- [Nginx Reverse Proxy Setup](#nginx-reverse-proxy-setup)
- [ddclient Google domains DNS setup](#ddclient-google-domains-dns-setup)
- [Firewall Security Setup](#firewall-security-setup)
  - [Firewall Configuration](#firewall-configuration)
- [Docker Installation Setup](#docker-installation-setup)


---


## Flash Ubuntu 22 Server 64-bit to SD Card

Follow these steps to flash Ubuntu 22 Server 64-bit to an SD card for Raspberry Pi 4:

1. Download Ubuntu 22 Server 64-bit image.
2. Use Etcher or Raspberry Pi Imager to flash the image to the SD card.

## Static IP address configuration setup

Configure a static IP address by editing the network configuration file located in `/etc/netplan/`. Refer to the following document for detailed instructions: [Ubuntu 20.04 Static IP Address](https://pimylifeup.com/ubuntu-20-04-static-ip-address/).

## Port forwarding

Navigate to your WiFi admin panel, access the Port Forwarding section, and add the device IP and port to enable port forwarding.

## SSH Setup with Different Port and Key File

Generate an SSH key pair, configure SSH with a different port, and use a custom key file:

```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 2048 -f my_ssh_key

# Convert private key to PEM format
ssh-keygen -p -m PEM -f my_ssh_key

# Set permissions for private key
chmod 600 my_ssh_key

# Copy public key to server
ssh-copy-id -i my_ssh_key.pub user@hostname

# Update SSH server configuration
sudo nano /etc/ssh/sshd_config
```

Set `Port` to desired port number and `PasswordAuthentication` to `no`. Restart SSH service:

```bash
sudo systemctl restart ssh
```

## Nginx Reverse Proxy Setup

Edit the Nginx configuration file:

```bash
sudo nano /etc/nginx/sites-available/nema.tenguriya.com
```

Add the following code:

```nginx
server {
    listen 80;
    server_name nema.tenguriya.com;

    location / {
        proxy_pass http://localhost:21012/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Refer to [How To Configure Nginx as a Reverse Proxy on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-as-a-reverse-proxy-on-ubuntu-22-04) for detailed instructions.

## ddclient Google domains DNS setup

Configure `ddclient` for dynamic DNS updates by editing the configuration file:

```bash
sudo nano /etc/ddclient.conf
```

Add the following configuration:

```conf
# ddclient configuration for Google Domains
daemon=60
syslog=yes
mail=root
mail-failure=root
pid=/var/run/ddclient.pid
use=web, web=checkip.dynu.com/, web-skip='IP Address'
server=domains.google.com
protocol=dyndns2
login=username
password='my password'
nema.tenguriya.com
```

Follow the instructions in the following references to set up `ddclient` for dynamic DNS updates:
- [Google Domains Dynamic DNS with Google Domains](https://cloud-jake.medium.com/google-domains-dynamic-dns-with-google-domains-1dd0ea45c219)
- [Dynu Dynamic DNS with DDClient](https://www.dynu.com/DynamicDNS/IPUpdateClient/DDClient)

## Firewall Security Setup

### Firewall Configuration

Set up firewall rules using `ufw`:

```bash
# Allow incoming connections on the new SSH port (e.g., 321)
sudo ufw allow 321/tcp

# Deny incoming connections on the default SSH port (22)
sudo ufw deny 22/tcp

# Enable firewall
sudo ufw enable

# Check firewall status
sudo ufw status
```

## Docker Installation Setup

Install Docker and Docker Compose:

```bash
sudo apt install docker.io
sudo apt install docker-compose
```

---

