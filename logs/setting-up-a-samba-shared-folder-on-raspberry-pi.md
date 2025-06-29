# Samba Shared Folder on Raspberry Pi

**Description:**  
Documentation covering the setup and troubleshooting of a Samba shared folder on a Raspberry Pi 5 to be accessed from a macOS device. Intended for users who want a simple local file sharing setup over Wi-Fi.

---

## Setup Overview

- **Device:** Raspberry Pi 5 (4GB RAM)
- **Client Device:** Mac
- **Network:** Home Wi-Fi (5GHz)
- **Access Method:** Samba (SMB) shared folder from macOS Finder

---

## Initial Setup: Install and Configure Samba

1. SSH into the Pi or use terminal directly.

2. Install Samba:

   ```bash
   sudo apt update
   sudo apt install samba samba-common-bin
   ```

3. Enable and start the Samba service:

   ```bash
   sudo systemctl enable smbd
   sudo systemctl start smbd
   ```

---

## Step 2: Create a Shared Directory

Create the directory you want to share. Replace `yourusername` with your actual Raspberry Pi username:

```bash
mkdir -p /home/yourusername/shared
chmod 775 /home/yourusername/shared
```

---

## Step 3: Configure Samba

1. Edit the Samba config file:

```bash
sudo nano /etc/samba/smb.conf
```

2. Add this to the bottom:

```ini
[Shared]
path = /home/yourusername/shared
writeable = yes
create mask = 0775
directory mask = 0775
browseable = yes
public = yes
guest ok = yes
```

3. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`)

---

## Step 4: Set a Samba User

Set a Samba password for your Pi user:

```bash
sudo smbpasswd -a yourusername
```

- You’ll be prompted to enter a password (can match your Pi login or be separate).
- Make sure the user exists on the Pi.

---

## Step 5: Restart Samba

```bash
sudo systemctl restart smbd
```

---

## Accessing from macOS

1. Open Finder → Go → Connect to Server…

2. Enter:

```bash
smb://<raspberry-pi-ip-address>
```

3. Choose “Registered User” and enter:

- **Username:** your Raspberry Pi username
- **Password:** the one set with `smbpasswd`

---

## Key Learnings

- Samba uses its own **authentication layer**, separate from system login.
- macOS “Registered User” login requires **exact match** with the `smbpasswd` user.
- Finder often caches bad attempts — use `Cmd + K` for a fresh mount.
- Permission errors are often due to missing `chmod` or misconfigured `smb.conf`.
- Samba restarts are essential after making any config changes.
