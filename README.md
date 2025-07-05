# 🛠️ Ubuntu Dual Boot: Simple NTFS Partition Auto-Mount Fix

This guide fixes common issues where NTFS partitions from Windows don't show up or mount automatically in Ubuntu dual-boot systems.

---

## 🔍 Problem Summary

NTFS (Windows) partitions don't auto-mount or appear in your file manager after booting into Ubuntu.

---

## ✅ Solution Overview

We will:

- Fix any filesystem issues.
- Set up the partition to mount automatically when you log in.
- Ensure it's visible in your File Manager under "Devices".

---

## 🧰 Simple Step-by-Step Setup

### 🔹 Step 1: Fix and Identify Your NTFS Partition

1.  **List partitions to find yours:**

    ```bash
    lsblk -f
    ```
    *Look for a partition with `ntfs` under `FSTYPE`, e.g., `/dev/nvme0n1p4`.*

2.  **Fix the filesystem:**

    ```bash
    sudo ntfsfix /dev/nvme0n1p4
    ```
    *(Replace `/dev/nvme0n1p4` with your actual partition name).*

### 🔹 Step 2: Set Up Auto-Mount

1.  **Create a folder for your drive:**

    ```bash
    sudo mkdir -p /media/$USER/SharedDrive
    ```

2.  **Create a script to mount the drive:**

    ```bash
    mkdir -p ~/.config/scripts
    nano ~/.config/scripts/mount-ntfs.sh
    ```
    *Paste this exactly into the file:*
    ```bash
    #!/bin/bash
    sudo mount -t ntfs-3g /dev/nvme0n1p4 /media/$USER/SharedDrive
    ```
    *(Remember to replace `/dev/nvme0n1p4` with your partition. Save and exit nano by pressing `Ctrl+O`, then `Enter`, then `Ctrl+X`.)*

3.  **Make the script executable:**

    ```bash
    chmod +x ~/.config/scripts/mount-ntfs.sh
    ```

### 🔹 Step 3: Allow Auto-Mount Without Password

1.  **Edit sudoers file:**

    ```bash
    sudo visudo
    ```
    *Add this line at the very bottom (replace `your_username` with your actual Ubuntu username):*
    ```
    your_username ALL=(ALL) NOPASSWD: /bin/mount
    ```
    *Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).*

### 🔹 Step 4: Run Script Automatically on Login

1.  **Open "Startup Applications":**
    *Search for "Startup Applications" in your applications menu and open it.*

2.  **Add a new entry:**
    *Click "Add", and fill in the details:*
    -   **Name:** `Auto-Mount NTFS`
    -   **Command:** `/home/your_username/.config/scripts/mount-ntfs.sh`
        *(Replace `your_username` with your actual Ubuntu username).*
    -   **Comment:** `Mount NTFS volume automatically after login`

---

### ✅ You're Done!

Restart your computer or log out and back in. Your NTFS partition should now be automatically mounted and visible under "Devices" in your file manager.

---

### ⚠️ Quick Troubleshooting Tips

* **If Windows was shut down improperly (e.g., without disabling Fast Startup/Hibernation):**
    Open Windows, disable "Fast Startup" and "Hibernation" (search for "power options" or run `powercfg /h off` in Windows Command Prompt as admin).