# 🚀 My Budget-Friendly Home Lab: ARM64 Infrastructure & Self-Hosting

Welcome to my Home Lab repository! This project documents my journey of repurposing an unused Android TV Box into a fully functional, low-power Linux mini-server. 

My primary goal is to build an efficient, cost-effective infrastructure to learn system administration, containerization, and secure remote networking.

---

## 🖥️ Hardware Specifications

Instead of buying expensive enterprise equipment, I optimized what I had. This setup proves that you can run a capable server environment on highly constrained resources.

* **Device Model:** STB HG680p (Repurposed Android TV Box)
* **Architecture:** ARM64
* **RAM:** 2 GB
* **ROM:** 8 GB eMMC
* **External Storage:** 32 GB USB Flash Drive (Used for boot & main storage)

---

## 🛠️ Base OS & Provisioning

To turn the Android TV box into a server, I replaced the stock Android OS with a lightweight Linux distribution.

1. **OS Selection:** I am running **Armbian OS (v25.05.00)**, sourced from a local Mini Server community.
2. **Flashing Process:** I used **BalenaEtcher** to flash the Armbian image onto the 32GB USB Drive.
3. **Troubleshooting the Boot Sequence:** Initially, the STB refused to boot from the USB drive and booted straight into Android. After consulting with the community, I discovered that triggering a forced reboot while the USB was plugged in would interrupt the bootloader and successfully boot into Armbian.

---

## 📦 Containerization & Management

To keep the system clean and make application deployments easier, I utilize Docker. To simplify container management, I use **CasaOS** as my primary dashboard.

**Installing Docker Engine:**
```bash
sudo apt-get update
sudo apt-get install docker-ce

```

**Installing CasaOS:**

```bash
curl -fsSL get.casaos.io/install.sh | sudo bash

```

---

## 🌐 Networking & Secure Remote Access

I wanted to access my CasaOS dashboard and other self-hosted services from anywhere without compromising my home network's security. Since I am behind a standard ISP router, traditional Port Forwarding was not ideal.

**Solution: Cloudflare Tunnel (Zero Trust)**
I purchased a personal domain (`wanzz.my.id`) and implemented a Cloudflare Tunnel. This creates a secure, outbound-only connection to Cloudflare's edge, allowing me to expose my local services safely.

**1. Installing `cloudflared` on Armbian:**

```bash
sudo mkdir -p --mode=0755 /usr/share/keyrings
curl -fsSL [https://pkg.cloudflare.com/cloudflare-main.gpg](https://pkg.cloudflare.com/cloudflare-main.gpg) | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null

echo 'deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] [https://pkg.cloudflare.com/cloudflared](https://pkg.cloudflare.com/cloudflared) any main' | sudo tee /etc/apt/sources.list.d/cloudflared.list

sudo apt-get update && sudo apt-get install cloudflared

```

**2. Authenticating & Running the Service:**

```bash
sudo cloudflared service install <YOUR_CLOUDFLARE_TOKEN_HERE>
sudo systemctl start cloudflared

```

**Result:** I successfully routed `casa.wanzz.my.id` through the tunnel to my local CasaOS port. I can now manage my server securely from anywhere in the world!

---

## 📈 Future Projects (Roadmap)

Since this is an ongoing project, here are a few things I plan to implement next:

* [x] Repurpose Android TV Box into a Linux Server.
* [x] Implement secure remote access via Cloudflare Tunnel.
* [ ] Deploy an ad-blocker (Wireguard / AdGuard Home) for the local network
* [ ] Set up a personal cloud storage solution (Nextcloud).

---

*If you have any questions or suggestions regarding this setup, feel free to open an Issue or reach out!*
