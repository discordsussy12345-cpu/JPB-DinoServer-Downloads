# DinoServer v1.1.19 — Windows instructions

This guide is for running the server on a **Windows computer** and playing JPB on an Android phone or emulator. The Windows download includes the Python runtime; you do not install Python or Termux for this package. Cache files and the game are separate downloads.

**Installation video:** [Watch the supplied installation video, starting at 1:18](https://www.youtube.com/watch?v=RTyVlTEICQQ&t=78s). Use the written steps below for this release's folder layout and update source; an older video may show different buttons or folders.

## 1. How to install DinoServer 1.1.19

### 1.1 Choose the correct download

1. Open [DinoServer downloads](https://github.com/discordsussy12345-cpu/JPB-DinoServer-Downloads/releases/latest).
2. Expand **Assets** if the files are hidden.
3. Download **DinoServer-Windows-v1.1.19.zip**. This is the complete portable Windows application.
4. Do not choose **Source code (zip)**, **Source code (tar.gz)**, the Linux source ZIP, or **DinoServer-Update-v1.1.19.zip** for a first installation. The update ZIP is for the updater, not a standalone installation.
5. Wait until the download finishes. A browser file ending in `.crdownload` or `.part` is incomplete.
6. Allow several GB of free disk space for the cache download, extracted cache, and backups. You need internet for downloading these files; local play still needs a working connection between the game device and server computer.

### 1.2 Extract the whole application

1. If replacing an older server, close JPB, press **Stop server** in the old launcher, and close the launcher. Closing its window alone does not stop a running server. Keep the old folder intact until your saves work in the new one.
2. In File Explorer, open **Downloads** and find the completed Windows ZIP.
3. Right-click it and choose **Extract All…**. Choose a new folder you can find again, for example `C:\Games\JPB-1.1.19`. Click **Extract**.
4. Open the extracted **DinoServer** folder. You should see **DinoServer.exe**, **server**, and **docs** beside each other.
5. Run the EXE from that extracted folder. Do not run inside the ZIP, move only the EXE onto your desktop, or merge this download into the old folder.
6. For a desktop shortcut, right-click the EXE and create a shortcut. Move the shortcut, not the EXE.
7. If Windows flags the download, check that it came from the release linked above. Do not disable antivirus globally. An incomplete or quarantined file must be resolved before continuing.
8. Open **DinoServer.exe**. The server should remain stopped until you press **Start server**. Opening Diagnostics or Cache Delivery does not intentionally start it.

**Already have a park?** Back up the old folder first, then follow section 2. The guided importer requires one guest login on the new server to establish its current device/save ID. Do not uninstall JPB or clear its data to install the server update.

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
- Existing 1.0.19 installations already using our update feed can update to 1.1.19 in the launcher. If your old package checks a different repository, install this full Windows package once and migrate using section 2.
- The updater preserves personal saves and configuration. Still keep a separate backup before applying any update. An update ZIP must never contain somebody else's saves, recovery database, device links, or running-session locks.

### 1.6 If something does not work

| What you see | What to check |
|---|---|
| EXE will not open | Extract the complete Windows ZIP; ensure `server` remains beside the EXE. Check for an incomplete download or quarantined file. |
| Cache download fails | Check internet and free space; keep the PC awake and retry. Wait for verification as well as download. |
| Status page cannot be reached from Android | Start the server deliberately, verify the LAN IP, same-network access, firewall, and whether another program owns the ports. Use **Diagnostics** and **Logs**. |
| Status works but JPB fails | Check original-domain versus IP-patched APK, the Android routing app, Android / Emulator profile, and server log messages. |
| `[GUEST-RECOVERY]` at login | Follow section 2.4; a changed device ID needs linking to its existing park. |
| `[SAVE-LOCK]` at login | Close all copies of the game using that park, stop duplicate server instances, then stop/start this server normally and retry after the old session expires. Keep the exact log message if it persists. Do not delete a live SQLite database or disable save protection. |
| A new empty park appears | Stop playing immediately. Keep both folders/saves and follow section 2.4. Do not overwrite your original park with the empty one. |

## 2. Import an old guest save in 1.1.19

> **MUST PLAY GAME ONCE FIRST TO GET DEVICE/SAVE ID.**

Open **Guest Saves** in the left sidebar, directly below **Cache Delivery**. It contains **Import old guest save**, **Restore existing park / changed device ID**, and **Repair guest save**. Settings > Project files also has an Open Guest Saves shortcut.

### 2.1 Back up your old park

1. Allow the game to finish saving, close JPB, stop the old server and close its launcher.
2. Copy the entire old server folder to a separate dated backup. Keep your original folder intact.
3. Set up the new extracted 1.1.19 server, cache and connection using section 1. Keep the new EXE and server scripts supplied with this release.

A guest save is normally a D-….json file in the old guest_saves folder. Older layouts may keep that folder at the installation root; the compact layout uses server/guest_saves. Cache files and the game APK do not contain your server park.

**Already updated in place and your existing park loads correctly?** You do not need to import it again. The updater preserves your saves and configuration. Import is for bringing progress from another folder or save file.

### 2.2 Play once on the new server

1. Start the new server and connect JPB to it.
2. Choose **Play as Guest** and actually enter the park. This allows the server to create and record the current device/save identity; simply opening the launcher is not enough.
3. Let it save, close JPB completely, and press **Stop server**.
4. Open **Guest Saves > Import old guest save**. Your current save should appear. If it does not, check the connection and complete the first guest login, then click **Refresh current saves**.

If login is waiting for device approval, see section 2.4. Keep the existing game installation and its app data.

### 2.3 Select and import your old park

1. Click **Select old folder** and select the old release folder (for example 1.0.18), or its **guest_saves** folder. Alternatively, click **Select guest save file** and choose the old JSON directly.
2. If several old parks are found, choose the correct one using its name, level and filename. The importer does not guess between multiple parks.
3. Review **CURRENT DEVICE / SAVE TO RESTORE**. If multiple current saves exist, select the device/save you just played. Importing replaces this selected park's progress.
4. Click **Import old guest save**. Review the old park, destination and device IDs in the confirmation, then confirm.
5. Wait for the success message. Start the server yourself, fully reopen JPB, and choose **Play as Guest**.

The imported copy automatically takes the selected current save's filename. **Leave your old filename and the new server's device/recovery/lock databases intact.** The current device binding remains in place. The original old file is unchanged, and copies of the replaced save and imported data are stored in **server/save_backups/imports**.

The selected old save supplies the progress, wallet and claim history, including aquatic/glacier data actually present in it. The importer cannot reconstruct missing data and does not combine unrelated parks. A live save lock blocks import instead of being forcibly cleared.

### 2.4 If the device is awaiting approval

Open **Guest Saves > Restore existing park / changed device ID** after trying guest login once.

- **The correct old park is already in this server:** select the current device and that existing park, then use **Restore selected park**. Retry guest login. No import or new park is needed.
- **You are deliberately importing a park from another folder:** select the waiting device and approve **Create new park instead**, enter that new park once, close JPB and stop the server, then use the guided import in sections 2.2–2.3. The selected new park is the destination that will be replaced.

Check whose device request you are approving on a multi-player server. Keep session databases and ownership protections intact. If the save is still in use, close other copies of the game and stop duplicate server instances before retrying.

### 2.5 Verify the imported park

1. Check the expected level, Bucks, resources, dinosaurs and buildings.
2. Visit each previously unlocked park: Surface, Aquatic and Glacier.
3. Make a small normal change, allow it to save, close JPB and reconnect. Confirm the change persists.
4. Keep the old folder and backups. If the wrong park was selected or progress is missing, stop and retain both copies before attempting recovery.

## 3. Version numbering and known investigation

Maintenance releases follow **1.1.19 → 1.2.19 → 1.3.19** and onward. **1.20.0** is reserved for future popup/promotional work.

Reports of XP bars dropping to zero and players stalling at levels such as 19 or 60 are still under investigation. **1.1.19 does not claim to fix those reports.**
