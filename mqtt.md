# Mosquitto MQTT Broker Setup

This guide will help you set up the Mosquitto MQTT broker on Ubuntu.

## Installation

Install Mosquitto using apt package manager:

```bash
sudo apt update
sudo apt install mosquitto
```

## Create Password File

Create a password file with username and password for authentication:

```bash
sudo mosquitto_passwd -c /etc/mosquitto/passwd shubham
```

# Enter and confirm the password when prompted

## Set Ownership

Give ownership of the password file to Mosquitto user:

```bash
sudo chown mosquitto:mosquitto /etc/mosquitto/passwd
```

## Configure Mosquitto

Create a configuration file named default.conf:

```bash
sudo nano /etc/mosquitto/conf.d/default.conf
```

Paste the following content into default.conf:

```conf
listener 1883
listener 9001
protocol mqtt
protocol websockets
allow_anonymous false
password_file /etc/mosquitto/passwd
```
## Test Configuration

Test the configuration file for any syntax errors:
```bash
mosquitto -c /etc/mosquitto/conf.d/default.conf
```

## Restart Mosquitto

Restart the Mosquitto service to apply the changes:
```bash
sudo systemctl restart mosquitto
```

You have successfully set up Mosquitto MQTT broker with authentication on your Ubuntu system.
