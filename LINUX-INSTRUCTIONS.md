# DinoServer v1.0.19 — Linux computer installation

This guide runs the **Python server directly on a Linux computer**, serving the Android JPB game on a phone/emulator. It does not use Termux, Wine, or `DinoServer.exe`. The Windows desktop launcher and Windows automatic updater are not included in the Linux workflow. This is a source-based installation; a native Linux end-to-end game test has not been performed for this release.

Commands below target **Ubuntu 24.04 / Debian 12 or newer** with Python **3.11+**. Other distributions need equivalent packages. Enter each command block in Terminal, then press Enter and wait for it to finish. Do not type the surrounding Markdown backticks. A `sudo` password prompt normally shows no characters while you type.

**Related installation video:** [User-supplied video, starting at 1:18](https://www.youtube.com/watch?v=RTyVlTEICQQ&t=78s). This is supplementary; the Linux commands below are the instructions for this source package.

## 1. Install the required tools

```bash
sudo apt update
sudo apt install python3 unzip curl ca-certificates authbind
python3 --version
```

Confirm Python reports 3.11 or newer. These server/cache commands use the Python standard library; you do not need to install the Windows GUI requirements. Allow several GB of free space for downloaded and extracted cache plus backups. Keep the computer awake while downloading and playing.

## 2. Download and extract the Linux source package

Open [the download release](https://github.com/discordsussy12345-cpu/JPB-DinoServer-Downloads/releases/latest). Under **Assets**, download **DinoServer-Linux-Source-v1.0.19.zip** and its matching **.zip.sha256** into your Downloads folder. Do not choose GitHub's automatically generated Source code ZIP or the Windows update ZIP.

For a fresh installation, run:

```bash
cd "$HOME/Downloads"
sha256sum -c DinoServer-Linux-Source-v1.0.19.zip.sha256
mkdir -p "$HOME/Games/JPB-1.0.19"
unzip DinoServer-Linux-Source-v1.0.19.zip -d "$HOME/Games/JPB-1.0.19"
cd "$HOME/Games/JPB-1.0.19/DinoServer/server"
pwd
```

The checksum must say **OK**. Stop if it fails. The final directory should end in `/DinoServer/server` and contain `jpb_server`, `tools`, and `config`. If the destination already contains an installation, choose a new empty directory instead of accepting overwrite prompts. Commands later in this guide assume this exact location; adjust it if you chose another.

## 3. Download, verify, and install the Android cache

Do this while DinoServer is stopped:

```bash
cd "$HOME/Games/JPB-1.0.19/DinoServer/server"
python3 -c 'from launcher.cache_delivery import install_cache; print(install_cache(".", progress=print))'
```

This calls the same verified cache installer used by Windows: it downloads the published Android cache ZIP from [JPB-Android-Cache](https://github.com/discordsussy12345-cpu/JPB-Android-Cache), checks its SHA-256, verifies each supported file, and installs into `server/cache_android`. It does not launch the server. Wait for the final success message, not merely the download percentage. The success text mentions Windows buttons; on Linux continue with step 4 below.

If interrupted during downloading, run the same command again to resume. For a checksum error, preserve the error text and replace the damaged archive under `server/downloads` before retrying. Do not run two cache imports at once.

If you already have the published ZIP, use its full path instead (replace YOURNAME):

```bash
python3 -c 'from launcher.cache_delivery import install_cache; print(install_cache(".", archive="/home/YOURNAME/Downloads/JPB-Android-Cache.zip", progress=print))'
```

The cache folder must contain the actual assets directly, not another nested `cache_android` folder. Android and iOS caches are different; this guide configures Android only.

## 4. Find your Linux computer's LAN address and build the manifest

```bash
hostname -I
```

Choose the IPv4 address belonging to the Wi-Fi/Ethernet network shared with the game device. It may resemble `192.168.1.21`. Ignore unrelated VPN/container addresses. You can also find it in the Linux network settings. **Replace the example IP below with your own:**

```bash
cd "$HOME/Games/JPB-1.0.19/DinoServer/server"
python3 tools/fix_manifest.py --host 192.168.1.21 --port 9943 --source config/fixed_manifest.json --cache-dir cache_android --build-from-cache --output config/fixed_manifest_android.json
```

Wait for the summary. Investigate missing assets before playing. This manifest tells the game where to fetch files; using an old PC address or `127.0.0.1` here will break downloads from another device. Repeat this step whenever the server IP changes, with DinoServer stopped.

## 5. Allow this user to bind HTTP port 80

Linux normally restricts ports below 1024. `authbind` lets your user bind port 80 without running the entire game server as root. On a personally managed computer where `/etc/authbind/byport/80` is not already configured:

```bash
sudo touch /etc/authbind/byport/80
sudo chown "$(id -un):$(id -gn)" /etc/authbind/byport/80
sudo chmod 500 /etc/authbind/byport/80
```

On a shared/administered computer, have the administrator grant access instead of replacing an existing authorization file. These commands grant your account permission for port 80; they do not open the firewall or start DinoServer. See the installed manual with `man authbind` for the permission model.

## 6. Permit the game device through an existing firewall

The Android server listens on TCP **80**, **9943**, and **9933**. If UFW is installed, inspect its current state:

```bash
sudo ufw status
```

If UFW is active, allow your game device's LAN IP. Replace `192.168.1.50` below with the **phone/emulator's reachable source IP**, not the server's IP:

```bash
sudo ufw allow from 192.168.1.50 to any port 80 proto tcp
sudo ufw allow from 192.168.1.50 to any port 9943 proto tcp
sudo ufw allow from 192.168.1.50 to any port 9933 proto tcp
```

If UFW is inactive, do not enable it just to follow this guide; if another firewall is active, configure equivalent LAN access there. These are local-network instructions, not instructions to expose the server on the public internet. [Ubuntu firewall documentation](https://ubuntu.com/server/docs/how-to/security/firewalls/) explains UFW rule syntax.

## 7. Start the server deliberately

If migrating an existing park, copy it first using step 10. Otherwise run:

```bash
cd "$HOME/Games/JPB-1.0.19/DinoServer/server"
JPB_MANIFEST_FILE=config/fixed_manifest_android.json authbind --deep python3 -m jpb_server.current_server_android --host 0.0.0.0 --http-ports 80,9943 --sfs-port 9933
```

This command starts the server now. Keep that Terminal open. It runs a single server process with connection threads; it is not the Windows launcher's worker-management setup. Do not start multiple copies on the same ports. No boot-time service or automatic restart is installed by this guide.

If it reports **Address already in use**, find and stop the other instance normally. If it reports **Permission denied** for port 80, check step 5 and that you used `authbind`. Do not solve this by running all of Python as root.

## 8. Test and connect JPB

1. Open a second Terminal and run `curl http://127.0.0.1:9943/status/2.0/`. Expect a status response.
2. On the phone/emulator browser open `http://YOUR-LINUX-IP:9943/status/2.0/`. Replace the placeholder with the same IP you used in step 4.
3. If the second test fails, check the firewall, IP, shared Wi-Fi/LAN, and router client isolation. The first test alone does not prove the phone can reach the server.
4. In **JPB HOSTS EDITOR** on Android, enter the **Linux computer's LAN IP**, enable VPN routing, and accept Android's VPN prompt. Use an original-Ludia-domain JPB APK. A game patched directly to another IP needs a matching build instead.
5. Do not use `127.0.0.1` on the phone to reach a separate Linux computer; it points back to the phone. This guide does not change the Linux computer's own hosts file to route the Android game.
6. Open JPB and choose **Play as Guest**. Leave the server Terminal and computer running.

## 9. Stop, restart, and inspect logs

Close JPB after it finishes saving. In the server Terminal press **Ctrl+C** and wait for the shell prompt. To start again, repeat step 7. Logs are under `DinoServer/server/logs`; guest saves are under `DinoServer/server/guest_saves`.

For a failed login, keep the exact `[SAVE-LOCK]` or `[GUEST-RECOVERY]` line and surrounding error. Close duplicate game/server sessions before retrying. Do not erase a live lock database or disable recovery checks.

## 10. Bring over an existing guest save

1. Stop the old and new servers and close the game. Make a dated copy of the entire old installation first.
2. Locate the old `guest_saves` folder (some old versions use `guest/_saves`). Copy the actual `D-….json` files into `DinoServer/server/guest_saves`, creating that folder if needed. Preserve names and letter case. Do not paste another `guest_saves` folder inside it.
3. Preserve your own `config/device_links.json`, `config/guest_recovery.sqlite3`, and `config/whitelist.json` if present. Copy the recovery database with any matching `-wal`/`-shm` companions only after both servers stop. Do not replace the whole new config folder. Do not copy the old `run` folder or session databases.
4. Ensure copied files are owned/writable by the Linux account running DinoServer. Extract and copy as that user, not with `sudo`.
5. If the game retains the same ID, it should select its existing save. If its ID changed, a recovery request is expected. Do not rename the save or choose a new park as a workaround.
6. To inspect pending device requests after one guest login attempt, stop the game/server and run from `server`:

```bash
python3 -c 'import json; from jpb_server import guest_recovery; print(json.dumps(guest_recovery.snapshot("."), indent=2))'
```

7. Identify the **device** value belonging to your device and the **id** of its original save from that output. Then replace BOTH placeholders in this command before running it:

```bash
python3 -c 'from jpb_server import guest_recovery; print(guest_recovery.approve(".", "CURRENT_DEVICE_ID", "ORIGINAL_SAVE_ID", False))'
```

This is an explicit ownership link, not a save merge. Only approve a device you recognize for the correct player's park. Preserve `guest_recovery.sqlite3` with future backups.

8. Start again, log in, verify level/resources and all unlocked parks, make a small change, close and reopen the game, and confirm persistence. Keep the original backup.

## 11. Future Linux updates

The Windows automatic updater does not apply to Linux. Download the next Linux source package from the downloads repository into a new folder, stop both installations, and migrate your saves/config/cache as above. Rebuild the manifest for your LAN IP. Do not extract a Windows update ZIP over Linux or overwrite your only working copy.
