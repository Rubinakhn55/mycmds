# 🔐 Anonymous Browsing & Pentesting with Proxychains + Tor on Ubuntu

This guide explains how to install and configure **Proxychains** and **Tor** on Ubuntu to anonymize your terminal tools (e.g., `curl`, `sqlmap`, `nmap`, etc.).

---

## ✅ Requirements

- Ubuntu (20.04 or later)
- Internet connection
- Terminal access (sudo privileges)

---

## 🧰 Step-by-Step Installation

### 1. Install Tor and Proxychains

```bash
sudo apt update
sudo apt install tor proxychains4 -y
```

---

### 2. Configure Proxychains to Use Tor (SOCKS5)

Edit the Proxychains config:

```bash
sudo nano /etc/proxychains4.conf
```

#### In the config file:

- Enable `dynamic_chain`  
- Disable (comment out) `strict_chain` and `random_chain`  
- Set the proxy type to SOCKS5:

```ini
dynamic_chain
#strict_chain
#random_chain

# Add this at the bottom
socks5 127.0.0.1 9050
```

> ✅ Save with `Ctrl + O`, then press `Enter`, and exit with `Ctrl + X`.

---

### 3. Start the Tor Service

```bash
sudo systemctl start tor
```

(Optional: enable Tor to start on boot)

```bash
sudo systemctl enable tor
```

Verify Tor is listening on port 9050:

```bash
ss -ltnp | grep 9050
```

Expected output:

```
LISTEN 127.0.0.1:9050 ... tor
```

---

## 🧪 Testing Tor + Proxychains

### Check your current IP (should be Tor exit node)

```bash
proxychains curl ifconfig.me
```

Or verify via Tor Project:

```bash
proxychains curl https://check.torproject.org
```

---

## 🚀 Example Usage

You can now run tools through Tor like this:

```bash
proxychains curl https://example.com
proxychains sqlmap -u http://target.com --batch
proxychains gobuster dir -u http://target.com -w wordlist.txt
proxychains nmap -sT -Pn target.com
```

---

## 🔁 Getting a New Tor IP (New Circuit)

To rotate Tor's circuit and get a new IP:

```bash
sudo systemctl restart tor
```

---

## 🛡️ Notes

- Never rely on proxychains alone for anonymity — combine it with good OPSEC.
- Tor exit nodes can be slow; adjust your tools accordingly.
- Some sites may block Tor exit nodes — that's normal.

---

## 📎 Resources

- [https://www.torproject.org](https://www.torproject.org)
- [https://github.com/rofl0r/proxychains-ng](https://github.com/rofl0r/proxychains-ng)

---

## ✅ Author

Created by `your-name-here` for secure, anonymous terminal operations using open-source tools.