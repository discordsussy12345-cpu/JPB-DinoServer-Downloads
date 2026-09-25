# DinoServer v1.0.19 — Windows instructions

This guide is for running the server on a **Windows computer** and playing JPB on an Android phone or emulator. The Windows download includes the Python runtime; you do not install Python or Termux for this package. Cache files and the game are separate downloads.

**Installation video:** [Watch the supplied installation video, starting at 1:18](https://www.youtube.com/watch?v=RTyVlTEICQQ&t=78s). Use the written steps below for this release's folder layout and update source; an older video may show different buttons or folders.

## 1. How to install DinoServer 1.0.19

### 1.1 Choose the correct download

1. Open [DinoServer downloads](https://github.com/discordsussy12345-cpu/JPB-DinoServer-Downloads/releases/latest).
2. Expand **Assets** if the files are hidden.
3. Download **DinoServer-Windows-v1.0.19.zip**. This is the complete portable Windows application.
4. Do not choose **Source code (zip)**, **Source code (tar.gz)**, the Linux source ZIP, or **DinoServer-Update-v1.0.19.zip** for a first installation. The update ZIP is for the updater, not a standalone installation.
5. Wait until the download finishes. A browser file ending in `.crdownload` or `.part` is incomplete.
6. Allow several GB of free disk space for the cache download, extracted cache, and backups. You need internet for downloading these files; local play still needs a working connection between the game device and server computer.

### 1.2 Extract the whole application

1. If replacing an older server, close JPB, press **Stop server** in the old launcher, and close the launcher. Closing its window alone does not stop a running server. Keep the old folder intact until your saves work in the new one.
2. In File Explorer, open **Downloads** and find the completed Windows ZIP.
3. Right-click it and choose **Extract All…**. Choose a new folder you can find again, for example `C:\Games\JPB-1.0.19`. Click **Extract**.
4. Open the extracted **DinoServer** folder. You should see **DinoServer.exe**, **server**, and **docs** beside each other.
5. Run the EXE from that extracted folder. Do not run inside the ZIP, move only the EXE onto your desktop, or merge this download into the old folder.
6. For a desktop shortcut, right-click the EXE and create a shortcut. Move the shortcut, not the EXE.
7. If Windows flags the download, check that it came from the release linked above. Do not disable antivirus globally. An incomplete or quarantined file must be resolved before continuing.
8. Open **DinoServer.exe**. The server should remain stopped until you press **Start server**. Opening Diagnostics or Cache Delivery does not intentionally start it.

**Already have a park?** Complete section 2 before your first game login. Do not uninstall JPB or clear its data to install the server update.

### 1.3 Install the Android cache

1. Close the game and leave the server stopped.
2. In DinoServer, open **Cache Delivery**.
3. Click **Download & Install Android Cache**. It downloads from [JPB-Android-Cache](https://github.com/discordsussy12345-cpu/JPB-Android-Cache), checks the archive and individual files, and installs them into `DinoServer\server\cache_android`.
4. Keep the application open and the computer awake. Downloading is only the first stage; wait for verification and installation to finish too.
5. If the download is interrupted, use the download button again to resume. A checksum error means the cache has not been accepted; replace the damaged download rather than starting the game with it.
6. If you already downloaded the cache ZIP, use the manual ZIP option on this page and select that archive. Do not select the Windows application ZIP.
7. If copying an already extracted cache manually, put the actual cache files directly inside `server\cache_android`. Avoid `cache_android\cache_android\...` and extra ZIP wrapper folders. The ZIP import is easier because it validates the files.
8. Return to **Overview** and select **Android / Emulator**. The included Android cache is not an iOS cache. iOS requires its own assets and setup.

### 1.4 Set the computer address and connect the game

1. Connect the computer and phone/emulator to the same trusted local network. A guest Wi-Fi network may block communication between devices.
2. On **Overview**, note the **Computer IP**. It normally resembles `192.168.1.21`; that is an example, not an address everyone should use.
3. If needed, open **Settings**, enable **Detect IP automatically**, or enter your computer's current LAN IPv4 address under **IP address**. Leave **Manifest port** at **9943**, then click **Save settings**.
4. Use **Open LAN ports** in Settings if Windows Firewall blocks the device. Approve the Windows administrator prompt for this operation. Keep Windows Firewall enabled. DinoServer uses TCP **80**, **9943**, and **9933** for the normal Android connection.
5. On the Android device/emulator, open your installed **JPB HOSTS EDITOR**. Enter the **computer's LAN IP** and enable its VPN routing. Accept Android's VPN connection prompt if requested. Only one Android VPN can normally be active at a time.
6. Use a JPB APK with its original Ludia domains for this routing setup. An APK hard-coded to a different IP can ignore domain-based routing; use a matching build or have it restored to original domains. Updating DinoServer does not replace your game APK.
7. **Do not enter `127.0.0.1` when the server runs on Windows.** Inside a phone or emulator it means that Android device itself. It is appropriate only when the server also runs inside that same Android environment.
8. If using another hosts tool instead, follow the launcher's Android connection guide. Its game hosts are `jp-4-9-0-pag.ludia.net` and `jp-4-9-0-pap.ludia.net`. Do not add unrelated CDN redirects from old instructions.
9. Press **Start server** in Overview. Wait for the status to become **Online**. If it reports an error, read it before launching the game.
10. In the phone/emulator browser, open `http://COMPUTER-IP:9943/status/2.0/`, replacing `COMPUTER-IP` with the address from Overview. Example: `http://192.168.1.21:9943/status/2.0/`. A JSON/text status response proves basic reachability. It does not, by itself, prove the game's routing or save identity is correct.
11. Open JPB and choose **Play as Guest**. Leave DinoServer running and the computer awake while you play. Initial game asset downloads can take longer than later logins.

### 1.5 Everyday use, stopping, and updates

- To play again: open DinoServer, check Android / Emulator and the IP, press **Start server**, enable the Android routing app, then open JPB.
- To stop: allow the game to finish saving, close JPB, then press **Stop server**. Keep **Restart** for an intentional restart; do not use it while a save or cache import is underway.
- If your computer's IP changes, update the Android routing app and DinoServer's IP setting. Restart the server so its cache manifest uses the current address.
- This download checks [JPB-DinoServer-Updates](https://github.com/discordsussy12345-cpu/JPB-DinoServer-Updates/releases/latest). Future releases must have a higher version to be offered.
- An old 1.0.19 package still checking somebody else's repository will not discover this same-version replacement automatically. Install this full package once and migrate your saves using section 2. After that, use the launcher's update prompt for future versions.
- The updater preserves personal saves and configuration. Still keep a separate backup before applying any update. An update ZIP must never contain somebody else's saves, recovery database, device links, or running-session locks.

### 1.6 If something does not work

| What you see | What to check |
|---|---|
| EXE will not open | Extract the complete Windows ZIP; ensure `server` remains beside the EXE. Check for an incomplete download or quarantined file. |
| Cache download fails | Check internet and free space; keep the PC awake and retry. Wait for verification as well as download. |
| Status page cannot be reached from Android | Start the server deliberately, verify the LAN IP, same-network access, firewall, and whether another program owns the ports. Use **Diagnostics** and **Logs**. |
| Status works but JPB fails | Check original-domain versus IP-patched APK, the Android routing app, Android / Emulator profile, and server log messages. |
| `[GUEST-RECOVERY]` at login | Follow section 2.5; a changed device ID needs linking to its existing park. |
| `[SAVE-LOCK]` at login | Close all copies of the game using that park, stop duplicate server instances, then stop/start this server normally and retry after the old session expires. Keep the exact log message if it persists. Do not delete a live SQLite database or disable save protection. |
| A new empty park appears | Stop playing immediately. Keep both folders/saves and follow section 2.5. Do not overwrite your original park with the empty one. |

## 2. How to move your guest saves

### 2.1 Understand what you are moving

A guest save is normally a file named **D-…json**, for example `D-example.json`. The actual filename contains the player's identifier; keep it unchanged. The save contains your park progress. The cache contains shared game assets and is not a substitute for your save. Copying the game APK also does not copy the server save.

Older releases can use `DinoServer\guest_saves` or `DinoServer\guest\_saves`. This release uses **`DinoServer\server\guest_saves`**. Use the actual folder containing your existing JSON saves, not an empty similarly named folder.

### 2.2 Stop everything and make a backup first

1. Close JPB on every phone/emulator using this server. Wait for any visible saving operation to finish first.
2. In the old launcher press **Stop server**, wait for stopped status, then close it. Stop the new server too if you opened it.
3. In File Explorer enable **View → Show → File name extensions** (Windows 11) or **View → File name extensions** (Windows 10). This makes `.json` and `.zip` visible.
4. Copy the **entire old DinoServer folder** to a separate backup location, such as another drive. Name the backup with today's date. Wait until copying finishes; do not merely create a shortcut.
5. Keep the original folder and backup until you have tested the new installation. Copy files in the next steps; do not cut/move your only originals.

### 2.3 Copy the saves into the new folder

1. Open the old save folder described in 2.1.
2. Select the guest `.json` files you need. If migrating the whole server, copy all player save files. Preserve filenames exactly.
3. Open the new extracted **DinoServer** folder, then **server**.
4. Open **guest_saves**. If it does not exist yet, create a folder with that exact name.
5. Paste the files directly inside it. Correct: `DinoServer\server\guest_saves\D-….json`. Incorrect: `DinoServer\server\guest_saves\guest_saves\D-….json`.
6. If Windows asks to replace an existing file, cancel until you know which copy has the wanted progress. Back up the destination too. Do not choose Replace All blindly.
7. Copy the old `save_backups` folder into the new `server\save_backups` if you want the existing recovery history.
8. Your old Android cache can be copied into `server\cache_android`, or downloaded through Cache Delivery. Do not put it inside `guest_saves`.

### 2.4 Preserve your own device links and recovery records

1. Find the old **config** folder: either `DinoServer\config` or `DinoServer\server\config`.
2. With both servers fully stopped, copy these **if present** into the new `DinoServer\server\config`: `device_links.json`, `guest_recovery.sqlite3`, `whitelist.json`, and `local_settings.json`.
3. If `guest_recovery.sqlite3-wal` or `guest_recovery.sqlite3-shm` exists alongside that database, preserve it with the same database while stopped. Do not mix database files from different installations. The whole-folder backup in 2.2 preserves the original set.
4. A version such as 1.0.14 may not have a recovery database. That is normal; do not download someone else's or invent one.
5. Copy only your own personal configuration. Do not replace the entire new config folder: its supplied manifests/options belong to the new release. Recheck the IP after copying local settings, especially on a different computer.
6. Do **not** copy the old `run` folder, session/lock databases, old EXE, old server scripts, or logs over the new installation. Runtime coordination is recreated locally. Existing saves and device mappings are preserved separately.

### 2.5 Link a changed device ID to the correct park

Changing the game APK/signature, reinstalling it, clearing app data, or using a different Android device/profile can change the ID presented to the server. Installing this Windows release cannot force every game APK to keep its old ID.

1. After copying the saves, start the new server and try **Play as Guest** once from the affected device. This records the device request. If recovery is required, the failed login is a protection against silently assigning the wrong park.
2. Return to DinoServer's save/import controls and click **Restore existing park / changed device ID**.
3. Select the current device ID from the first list. Waiting requests appear first. If multiple people are connecting, identify whose request it is before approving it.
4. In the second list, choose that player's original save. It shows the save ID, name, and level. Verify against your old save/backup.
5. Click **Restore selected park** and confirm the displayed device-to-save link. Do not click **Create new park instead** when trying to recover progress.
6. Retry guest login. This links the device to the existing file; it does not merge parks or delete duplicates.
7. If no request appears, check that JPB is reaching this server. If no complete saves appear, recheck the destination path and that you copied the full JSON file.

### 2.6 Verify the migration before deleting anything

1. Confirm the expected player level, Bucks, resources, dinosaurs and buildings.
2. Visit each already-unlocked park: Surface, Aquatic, and Glacier. Locked areas should stay locked.
3. Make a small normal change, let it save, close JPB, and log in again. Confirm it persists.
4. Check that you are using the new server folder by viewing its logs and save file modification time.
5. Keep the dated backup even after success. If something is missing, stop and restore from that backup rather than editing currency or account IDs by hand.
