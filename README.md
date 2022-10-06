# Why this FORK?

For now I'm just test stuff out here. I don't know what I'm doing and hope I have to do as little as possible.

## Why did I fork in the first place?

- Namechaep's api wants a client ip argument but according to [Lexicon](https://github.com/AnalogJ/lexicon/) and my testing you can just send any ip which is than updated on namecheaps side. #1
- pip module
  - Not really the reason for the fork and I'll try to contact the original maintainer before I do something in that regard.

# General

This plugin automates the process of completing a ``dns-01`` challenge by creating, and subsequently removing, TXT records using the (XML-RPC-based) namecheap.com API.

------------------

## Presequence

### Getting API access

Namecheap has certain requirements for activation to prevent system abuse. In order to have API enabled for your account, you should meet one of the following requirements:

- have at least 20 domains under your account;
- have at least $50 on your account balance;
- have at least $50 spent within the last 2 years.

## Credentials

Use of this plugin requires a configuration file containing Namecheap API credentials, obtained from your Namecheap account's [API Managenment page](https://ap.www.namecheap.com/settings/tools/apiaccess/).

```ini
# Namecheap API credentials used by Certbot
dns_namecheap_username=my-username
dns_namecheap_api_key=my-api-key
```
The file should only be readable by root. If other users or programms get access to your api credentials they can takeover your namecheap account aswell as domains!
```bash
sudo chown root:root /path/to/namecheap.ini
sudo chmod 0600 /path/to/namecheap.ini
```

The path to this file can be provided by using the `--dns-namecheap-credentials` command-line argument.

## Usage

### Docker

- **Recommended usage**. Create the credentials file and 2 folders for the certificates and logs and run:

```sh
git clone https://github.com/knoxell/certbot-dns-namecheap.git
cd certbot-dns-namecheap
docker build . -t certbot-dns-namecheap
docker run -it --rm \
  -v $(pwd)/certs:/etc/letsencrypt \
  -v $(pwd)/logs:/var/log/letsencrypt \
  -v $(pwd)/namecheap.ini:/namecheap.ini \t
  certbot-dns-namecheap certonly \
  -a dns-namecheap \
  --dns-namecheap-credentials=/namecheap.ini \
  --agree-tos \
  --no-eff-email \
  -email "your@mail.com" \
  -d example.com \
  --dry-run
```

- After a successful run, remove the last parameter `--dry-run` which enabled [staging server](https://letsencrypt.org/docs/staging-environment/) and run again.

## Python

- If you know what you're doing install the plugin into the same python environment like `certbot`. In any other case follow the `Docker` approach above:

```sh
git clone https://github.com/knoxell/certbot-dns-namecheap.git
pip install certbot-dns-namecheap/
```

- Check that `certbot` discovers the plugin:

```sh
certbot plugins
```

- Now run the command:

```sh
certbot certonly \
  -a dns-namecheap \
  --dns-namecheap-credentials=/namecheap.ini \
  --agree-tos \
  --no-eff-email \
  -email "your@mail.com" \
  -d example.com \
  --dry-run
  ```
