# Firewall - UFW on Ubuntu

- [Firewall - UFW on Ubuntu](#firewall---ufw-on-ubuntu)
  - [Install / Uninstall UFW](#install--uninstall-ufw)
  - [Enable / Disable UFW](#enable--disable-ufw)
  - [Policy](#policy)
    - [Reset all policy](#reset-all-policy)
    - [Checking the Status and Rules](#checking-the-status-and-rules)
    - [View setup rules](#view-setup-rules)
    - [Setup default rules](#setup-default-rules)
    - [Managing UFW Application](#managing-ufw-application)
    - [Allow Connections](#allow-connections)
    - [Denying Connections](#denying-connections)
    - [Limiting SSH connections](#limiting-ssh-connections)
  - [Deleting Rules](#deleting-rules)

---

## Install / Uninstall UFW

```sh
sudo apt update && sudo apt upgrade -y
sudo apt-get install ufw -y
sudo apt autoremove ufw --purge -y
```

---

## Enable / Disable UFW

```sh
sudo ufw enable
sudo ufw status
sudo ufw disable
```

---

## Policy

### Reset all policy

```sh
sudo ufw reset
```

### Checking the Status and Rules

```sh
# To check the firewall status and ufw rules
sudo ufw status verbose

# To view the ufw rules with their sequence numbers
sudo ufw status numbered
```

---

### View setup rules


```sh
# verify which rules were added
sudo ufw show added
```

### Setup default rules

```sh
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

---

### Managing UFW Application

```sh
# To list all application profiles available on your server
sudo ufw app list

# see profiles for all known applications
sudo ufw app info all

# To view details of the firewall profile for a specific application
sudo ufw app info '<name>'

# to update the firewall with the most recent profile information
sudo ufw app update '<name>'
```

---

### Allow Connections

- syntax:
  - `ufw allow [proto PROTOCOL] [from ADDRESS [port PORT | app APPNAME ]] [to ADDRESS [port PORT | app APPNAME ]] [comment COMMENT]`

```sh
sudo ufw allow port_number
sudo ufw allow port_number/protocol
```

- e.g.:

```sh
# allow ssh
sudo ufw allow ssh
sudo ufw allow 22
# allow ftp
sudo ufw allow 21/tcp
sudo ufw allow 20/tcp
# allow mysql
sudo ufw allow mysql
sudo ufw allow 3306/tcp
# allow HTTP
sudo ufw allow 80/tcp
sudo ufw allow http
# allow HTTPS
sudo ufw allow https
sudo ufw allow 443/tcp
# alloww DNS 53
sudo ufw allow 53 comment 'DNS server'
sudo ufw allow dns comment 'DNS server'
# Allowing Port Ranges
sudo ufw allow 55100:55200/tcp
sudo ufw allow 22,80,443/tcp
# allow Apache with both HTTP and HTTPS
sudo ufw allow 'Apache Full'
# Nginx with both HTTP and HTTPS
sudo ufw allow 'Nginx Full'


# Allow Connections From an Only Trusted IP Address
sudo ufw allow from 10.10.10.100
# Allow Connections From a Trusted IP Address on Specific port
sudo ufw allow from 10.10.10.10 to any port 3306
# Allow Connections From Trusted Subnets
sudo ufw allow from 10.10.0.0/24 to any port 20:21 proto tcp
# Allow Connections From a Specific Interface
sudo ufw allow in on ens18 to any port 80 proto tcp
ip addr
sudo ufw allow in on eth0 to any port 80
sudo ufw allow in on eth1 to any port 3306
```

---

### Denying Connections

```sh
sudo ufw deny port/protocol
```

- e.g.:

```sh
sudo ufw deny http
sudo ufw deny out 25

sudo ufw deny from 122.133.144.155
sudo ufw deny from 122.133.144.155 to any port 80,443 proto tcp
sudo ufw deny ssh/tcp
```

---

### Limiting SSH connections

```sh
sudo ufw limit ssh/tcp
```

---

## Deleting Rules

```sh
# delete by numbered
sudo ufw status numbered
sudo ufw delete 2

# delete by rule
sudo ufw delete allow 8080
sudo ufw delete allow "Apache Full"
sudo ufw delete allow http
sudo ufw delete allow 80
```
