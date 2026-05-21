# 02 — Key Pair & Security Group

> Configuring authentication credentials and firewall rules before launching the instance.

---

## Part 1 — Key Pair (SSH Authentication)

A Key Pair is a set of cryptographic credentials used to authenticate SSH access to the server — no password required.

- **Public key:** stored by AWS on the server
- **Private key:** downloaded as a `.pem` file to your local machine

### Creating the Key Pair

1. In the instance configuration screen, locate the **Key pair (login)** section
2. Click **"Create new key pair"** — a modal opens **on top of the current screen** (your configuration is preserved)
3. Fill in the fields:

| Field | Value |
|---|---|
| Name | `minecraft-key` |
| Key pair type | RSA |
| Private key file format | `.pem` (for PowerShell / Mac / Linux) |

4. Click **"Create key pair"**
5. The file `minecraft-key.pem` downloads automatically — **save it in a secure location**

> 📸 `assets/screenshots/02-keypair-modal.png`

> ⚠️ **Critical:** The `.pem` file is provided **only once**. If lost, you must create a new Key Pair and re-associate it with the instance. Never share this file.

---

## Part 2 — Security Group (Firewall)

A Security Group acts as a virtual firewall, controlling which ports accept inbound connections to the instance.

### Network Settings

In the **Network settings** section:

- **VPC:** default (leave as is)
- **Subnet:** No preference
- **Auto-assign public IP:** Enable
- **Firewall:** Create security group

> 📸 `assets/screenshots/02-network-settings.png`

### Inbound Rules

Configure **two rules**:

#### Rule 1 — SSH Access (terminal connection)

| Field | Value |
|---|---|
| Type | SSH |
| Protocol | TCP |
| Port | 22 |
| Source type | **My IP** |

> Restricting port 22 to your IP is a security best practice — it prevents unauthorized SSH access from external IPs.

#### Rule 2 — Minecraft (game connection)

Click **"Add security group rule"** and fill in:

| Field | Value |
|---|---|
| Type | Custom TCP |
| Protocol | TCP |
| Port | 25565 |
| Source type | **Anywhere — 0.0.0.0/0** |

> Port 25565 must be open to all IPs so any player can connect to the server.

> 📸 `assets/screenshots/02-security-group-rules.png`

### Final Security Group Configuration

```
Inbound Rules:
┌─────────────┬──────────┬───────────────────────┐
│ Type        │ Port     │ Source                │
├─────────────┼──────────┼───────────────────────┤
│ SSH         │ 22       │ My IP (x.x.x.x/32)   │
│ Custom TCP  │ 25565    │ 0.0.0.0/0             │
└─────────────┴──────────┴───────────────────────┘
```

---

## Launching the Instance

After configuring Key Pair and Security Group:

1. Review the configuration summary on the right panel
2. Click **"Launch instance"**
3. Wait ~30 seconds for the status to change to **"Running"**
4. Click on the instance ID and note the **Public IPv4 address**

> 📸 `assets/screenshots/02-instance-running.png`

> ⚠️ **Important:** The public IP **changes** every time the instance is stopped and restarted. For a permanent IP, use **Elastic IP** (free while the instance is running).

---

## Next Step

➡️ [03 — SSH Connection](03-ssh-connection.md)
