# Remote Access Raspberry Pi 5

**Description:**  
Documentation of troubleshooting the SSH remote access setup from a Mac terminal to a Raspberry Pi 5. Covers common issues such as permission errors, host key conflicts, static IP configuration, and username mismatches encountered during setup. Useful for anyone facing similar remote SSH connection challenges with Raspberry Pi devices.

---

## Setup Overview

- **Device:** Raspberry Pi 5 (4GB RAM)
- **Kit:** Starter kit with pre-installed Raspberry Pi OS on SD card
- **Client Device:** Mac
- **Network:** Home Wi-Fi (5GHz)
- **Access Method:** SSH from macOS Terminal

---

## Initial Configuration Steps

1. Unboxed and booted Raspberry Pi using the pre-installed microSD card.
2. Attempted to SSH into the Pi from the Mac terminal:

   ```bash
   ssh pi@<ip-address>
   ```

   **Result:**

   ```
   Permission denied (publickey, password)
   ```

3. Opened the Pi terminal locally and ran:

   ```bash
   sudo raspi-config
   ```

   - Enabled SSH
   - Rebooted the Pi
   - Still got "Permission denied" when entering password

4. Checked:

   - Keyboard settings were set to US — confirmed OK
   - Suspected incorrect password entry — not the issue

---

## Network Troubleshooting

5. Suspected a connectivity issue:

   - Investigated if 2.4GHz vs 5GHz Wi-Fi was the cause
   - Confirmed Raspberry Pi 5 supports 5GHz — not the issue

---

## Fresh Start: Factory Reset

6. Re-imaged the Raspberry Pi using Raspberry Pi Imager with the following options enabled:

   - SSH access
   - Password authentication
   - Custom username and password

7. Booted up and retried SSH connection.

   **Still failed with:**

   ```
   Permission denied
   ```

8. On one attempt to SSH, received the following warning in Terminal:

   ```
   WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
   @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
   IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
   Someone could be eavesdropping on you right now (man-in-the-middle attack)!
   It is also possible that a host key has just been changed.
   ```

   - Deleted the entry in the `known_hosts` file to reset SSH trust:

     ```bash
     ssh-keygen -R <ip-address>
     ```

   - Attempted SSH again

---

## Static IP Assignment

9. Logged into router admin panel (Sagemcom):

   - Navigated to DHCP settings
   - Reserved a static IP address for the Pi
   - Entered the MAC address of the Raspberry Pi
   - Saved and applied settings

10. Tried SSH again from Mac:

    ```
    Permission denied
    ```

---

## Resolution: Username Error

11. Identified the issue: wrong username used in the SSH command.

    Incorrect:

    ```bash
    ssh pi@<ip-address>
    ```

    Correct:

    ```bash
    ssh <custom-username>@<ip-address>
    ```

    **SSH connection succeeded.**

---

## Key Learnings

- SSH login requires the correct username — `pi` won’t work if a custom user was set during imaging.
- Static IP simplifies future access, but won’t fix credential issues.
- Factory re-imaging with SSH enabled is a reliable fallback.
- Don't overlook user error — always double-check credentials.
- Removing entries from `~/.ssh/known_hosts` resolves conflicts after reimaging.
