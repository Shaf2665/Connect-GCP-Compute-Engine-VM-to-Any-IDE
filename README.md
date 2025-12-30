# Connect GCP Compute Engine VM to Any IDE

[![Status](https://img.shields.io/badge/Project-Active-success)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-blue)]()
[![Cloud](https://img.shields.io/badge/Cloud-Google%20Cloud-important)]()
[![IDE Support](https://img.shields.io/badge/IDE-Cursor%20%7C%20VS%20Code%20%7C%20JetBrains-informational)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Stars](https://img.shields.io/github/stars/Shaf2665/Connect-GCP-Compute-Engine-VM-to-Any-IDE?style=social)]()
[![Downloads](https://img.shields.io/github/downloads/Shaf2665/Connect-GCP-Compute-Engine-VM-to-Any-IDE/total?label=Downloads)]()

A complete guide to help developers connect their Google Cloud Compute Engine VM to an IDE such as Cursor, VS Code, JetBrains IDEs, or any SSH-enabled development environment.

---

## Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Step 1 — Install Google Cloud CLI](#step-1--install-google-cloud-cli)
- [Step 2 — Generate SSH Key](#step-2--generate-ssh-key)
- [Step 3 — Add Key Using OS Login](#step-3--add-key-using-os-login)
- [Step 4 — Find Your OS Login SSH Username](#step-4--find-your-os-login-ssh-username)
- [Step 5 — Connect to VM from Terminal](#step-5--connect-to-vm-from-terminal)
- [Connect Using IDEs](#connect-using-ides)
  - [Cursor IDE](#cursor-ide)
  - [Visual Studio Code](#visual-studio-code)
  - [JetBrains IDEs](#jetbrains-ides)
- [Troubleshooting](#troubleshooting)
- [License](#license)
- [Contributing](#contributing)

---

## Overview

This guide uses:

- **OS Login (recommended by Google)** for security
- **SSH key authentication**
- Works on **Windows**, **macOS**, and **Linux**

---

## Prerequisites

- Active Google Cloud account
- Compute Engine VM running Linux (Ubuntu recommended)
- External IP or IAP access

---

## Step 1 — Install Google Cloud CLI

Install from the link below:

https://cloud.google.com/sdk/docs/install

Then authenticate:

```sh
gcloud init
```

---

## Step 2 — Generate SSH Key

### Windows (PowerShell)

```powershell
ssh-keygen -t rsa -f "$env:USERPROFILE\.ssh\gcp_vm" -C "local-access"
```

### macOS / Linux

```bash
ssh-keygen -t rsa -f ~/.ssh/gcp_vm -C "local-access"
```

> **Note:** Press Enter for passphrase (optional)

---

## Step 3 — Add Key Using OS Login

### Windows (PowerShell)

```powershell
gcloud compute os-login ssh-keys add --key-file "$env:USERPROFILE\.ssh\gcp_vm.pub"
```

### macOS / Linux

```bash
gcloud compute os-login ssh-keys add --key-file ~/.ssh/gcp_vm.pub
```

---

## Step 4 — Find Your OS Login SSH Username

```bash
gcloud compute os-login describe-profile --format="value(posixAccounts[0].username)"
```

**Note the output** (example):

```
mohammedshafiq_gmail_com
```

---

## Step 5 — Connect to VM from Terminal

1. Find VM external IP:
   - GCP Console → Compute Engine → VM Instances

2. Connect:

   **Windows (PowerShell):**

   ```powershell
   ssh -i "$env:USERPROFILE\.ssh\gcp_vm" <USERNAME>@<EXTERNAL_IP>
   ```

   **macOS / Linux:**

   ```bash
   ssh -i ~/.ssh/gcp_vm <USERNAME>@<EXTERNAL_IP>
   ```

   **Example:**

   ```bash
   ssh -i ~/.ssh/gcp_vm mohammedshafiq_gmail_com@34.100.245.241
   ```

---

## Connect Using IDEs

### Cursor IDE

1. Open terminal inside Cursor:

   ```bash
   ssh <USERNAME>@<EXTERNAL_IP> -i ~/.ssh/gcp_vm
   ```

2. You can now:
   - Run commands on VM
   - Use remote development workflow

### Visual Studio Code

1. Install extension:
   - **Remote Development**
   - **Remote - SSH**

2. Edit SSH config:

   ```bash
   code ~/.ssh/config
   ```

3. Add:

   ```ssh-config
   Host gcpvm
       HostName <EXTERNAL_IP>
       User <USERNAME>
       IdentityFile ~/.ssh/gcp_vm
   ```

4. Then:
   - Press `F1` → `Remote-SSH: Connect to Host` → `gcpvm`
   - Select folder inside VM

### JetBrains IDEs (IntelliJ, PyCharm, etc.)

1. Open JetBrains Gateway
2. Select **SSH Connection**
3. Configure:
   - **Host:** VM IP
   - **User:** OS Login username
   - **Identity:** `gcp_vm` private key
4. Select IDE on backend → **Connect**

---

## Troubleshooting

| Issue | Root Cause | Fix |
|-------|------------|-----|
| Permission denied (publickey) | Key not added via OS Login | Re-run Step 3 |
| gcloud not recognized | CLI not installed | Install CLI + restart terminal |
| Timeout | Firewall blocking port 22 | Enable SSH in VPC → Firewall |
| Wrong username | OS Login username required | Run Step 4 |
| Cannot find .ssh folder | Windows path differs | Use `$env:USERPROFILE\.ssh` |

---

## License

MIT License — Open for anyone to use and improve.

See the [LICENSE](LICENSE) file for details.

---

## Contributing

Contributions are welcome! Feel free to submit PRs for:

- More IDE integration examples
- Auto-sync tooling
- Dev Containers configuration
- Documentation improvements
- Bug fixes

---

## Visual Assets (Optional)

Future updates may include diagrams and screenshots.

Contributions welcome.

---

**Made with ❤️ for the developer community**
