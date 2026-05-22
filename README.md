# iOS IPA Decrypter for Windows

A set of automated Windows utility scripts to easily list, decrypt, and dump iOS apps (IPAs) from a jailbroken device over USB. 

By utilizing **Clutch** on the device and **libimobiledevice** on Windows, this tool creates a local USB tunnel, triggers decryption, downloads the raw binary, and automatically renames the final IPA file with its true version number.

## ✨ Features
* **100% USB-Based:** No Wi-Fi required. Uses `iproxy` to securely tunnel SSH traffic over USB.
* **Alphabetical App List:** Automatically fetches and sorts all installed encrypted apps on your device.
* **Bulk Processing:** Decrypt multiple apps at once by entering a list of numbers (e.g., `1 5 12`) or typing `ALL`.
* **Live Terminal Output:** Watch Clutch's decryption process live from your Windows terminal.
* **Smart File Renaming:** Automatically unpacks the downloaded IPA, reads the `Info.plist`, and tags the final filename with the app's actual version number.

## ⚠️ Prerequisites
Before using these scripts, ensure you have the following:

1. **A Windows PC** (Windows 10/11 recommended).
2. **A Jailbroken iOS Device** connected via USB and unlocked.
3. **OpenSSH** installed on your iOS device (default port `22`, default password `alpine`).
4. **[Clutch](https://github.com/KJCracks/Clutch)** installed on your iOS device. *(Ensure the executable is in your device's PATH, typically `/usr/bin/clutch`)*.

## 📂 Repository Structure
For these scripts to work, you must have a folder named `libimobile-suite-w64` in the same directory as the scripts containing the required Windows binaries:
* `iproxy.exe`
* `sshpass.exe`
* `pscp.exe`

## 🚀 How to Use

### Step 1: List Installed Apps
1. Connect your unlocked jailbroken device to your PC via USB.
2. Double-click **`01-Get-ClutchApps.bat`**.
3. The script will boot up a background USB tunnel, log into your device, and display an alphabetical list of all apps eligible for decryption, each with an assigned ID number.
4. Keep the window open or note down the numbers of the apps you want to dump.

### Step 2: Decrypt and Dump IPAs
1. Right-click **`02-Dump-App.ps1`** and select **"Run with PowerShell"**.
2. When prompted, enter the app numbers you wish to decrypt:
   * **Single app:** `5`
   * **Multiple apps:** `5 12 18` or `5-12-18`
   * **Every app:** Type `ALL`
3. Sit back and watch. The script will command Clutch to decrypt the app, use SFTP to transfer the file to your PC, parse the app's version, and rename the file.

## ⚙️ Configuration
You can customize the output behavior by right-clicking `02-Dump-App.ps1` and selecting **Edit**. Look for the configuration block at the top:

```powershell
# ====================================================================
# ==================== CONFIGURATION (EDIT HERE) =====================
# ====================================================================

# 1. Folder containing your tools (iproxy, pscp, sshpass)
$SuitePath = Join-Path $ScriptDir "libimobile-suite-w64"

# 2. Where to save the final extracted IPAs
$DestinationFolder = "E:\Decrypted IPA Files"

# 3. Custom tag added to the final filename
$Signature = "ninja_archive"
