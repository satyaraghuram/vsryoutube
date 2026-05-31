<img width="1305" height="313" alt="Screenshot 2026-05-31 at 7 37 45 PM" src="https://github.com/user-attachments/assets/af4cdb96-973d-4e18-9669-ec5db8e32f8f" />


# 🧹 Mac Storage Cleanup — Complete Guide
### Free Up Gigabytes in Minutes | No Extra App Needed

> **Who is this for?** Anyone whose Mac says "Storage Almost Full" or is running slow.  
> **What you need:** Just your Mac's Terminal app. Nothing to install.  
> **How to open Terminal:** Press `Command (⌘) + Space` → type **Terminal** → press Enter.

---

## ⚠️ Before You Start — Read This

- All commands below work on **macOS Ventura, Sonoma, and Sequoia**
- These are **safe, reversible-in-spirit** cleanups — caches, logs, and backups will rebuild over time
- Commands marked **🔴 Permanent** delete things that will NOT come back (like VM files or WhatsApp data)
- Copy each command **exactly** as written — one wrong character matters

---

## STEP 1 — Find Out What's Eating Your Storage

Before deleting anything, run these two commands to **see what's taking the most space**.

---

### 🔍 Check Your Top 20 Biggest App Containers

App containers are folders where apps secretly store gigabytes of data. This command lists them from **largest to smallest**.

**Paste this into Terminal and press Enter:**

```bash
du -hxd1 ~/Library/Containers | sort -hr | head -20
```

**What you'll see:**

```
12G    /Users/yourname/Library/Containers/com.utmapp.UTM
4.2G   /Users/yourname/Library/Containers/net.whatsapp.WhatsApp
1.8G   /Users/yourname/Library/Containers/com.microsoft.teams2
...
```

The **biggest number on the left = biggest space hog**. Note the folder name — you'll use it below.

---

### 🔍 Check Your Entire Library Folder

```bash
du -hxd1 ~/Library | sort -h
```

This shows ALL Library subfolders — caches, logs, app support, etc. — sorted by size.

---

### 🔍 Find Any Single File Bigger Than 500MB (Anywhere on Mac)

```bash
sudo find / -type f -size +500M 2>/dev/null
```

**What each part means — in plain English:**

| Part | What it does |
|---|---|
| `sudo` | Runs the command as admin, so it can look inside protected folders |
| `find /` | Starts searching from the very root of your Mac (everywhere) |
| `-type f` | Only look for **files** (not folders) |
| `-size +500M` | Only show files **bigger than 500 MB** |
| `2>/dev/null` | Hides error messages like "Permission denied" so your screen stays clean |

> 💡 Think of it like: *"Search my entire Mac, as an admin, and show me every file larger than 500 MB — quietly."*

**What you'll see after running it:**

```
/Users/yourname/Movies/wedding_video_raw.mov         ← 8.2 GB — probably yours, keep it
/Users/yourname/Downloads/MacOS_Ventura.dmg          ← 6.1 GB — old installer, safe to delete
/private/var/vm/sleepimage                           ← 8 GB — system file, do NOT delete
/Users/yourname/Library/Containers/com.utmapp.UTM/...← 12 GB — VM file, delete if not needed
```

**What to delete vs. keep:**

| Location starts with… | What it is | Safe to delete? |
|---|---|---|
| `/Users/yourname/Downloads/` | Files you downloaded | ✅ Yes, if you don't need them |
| `/Users/yourname/Movies/` | Your video files | ⚠️ Only if you have a backup |
| `/Users/yourname/Library/Containers/` | App data | ✅ Yes, using the commands in Step 3–4 |
| `/private/var/` | macOS system files | 🔴 No — leave these alone |
| `/Library/` (without yourname) | System/app files | 🔴 No — leave these alone |

> ⚠️ **This command takes 1–2 minutes to finish** — the cursor will just sit there. That's normal. Wait for it.

---

### 🔍 Check Total Free vs. Used Disk Space

```bash
df -h
```

Look at the row starting with `/dev/disk` — the **"Avail"** column shows your free space.

---

## STEP 2 — Delete What's Safe (No Login Lost)

These are **100% safe to delete**. Apps will recreate these files automatically.

---

### 🗑️ Clear All User Caches

**What it is:** Temporary files apps create to load faster. They rebuild automatically.  
**Typical space saved:** 1–10 GB

```bash
rm -rf ~/Library/Caches/*
```

✅ Safe — nothing will break. Apps may open slightly slower once, then return to normal.

---

### 🗑️ Clear App Logs

**What it is:** Text files apps write to track errors. You never read them. They pile up.  
**Typical space saved:** 100 MB – 2 GB

```bash
rm -rf ~/Library/Logs/*
```

✅ Safe — no apps are affected.

---

### 🗑️ Clear Safari Browser Cache

**What it is:** Websites save images/scripts locally to load faster. Clears safely.  
**Typical space saved:** 200 MB – 3 GB

```bash
rm -rf ~/Library/Containers/com.apple.Safari/Data/Library/Caches/*
```

✅ Safe — Safari will reload pages fresh. No passwords lost.

---

### 🗑️ Delete iPhone/iPad Backups (BIG space!)

**What it is:** Every time you plug your iPhone into your Mac, it backs up — silently eating 10–100 GB.  
**Typical space saved:** 10–100 GB ← This is often the #1 culprit!

```bash
rm -rf ~/Library/Application\ Support/MobileSync/Backup/*
```

✅ Safe — your iPhone is still backed up to iCloud. This only removes the **Mac-local copy**.

> 💡 **Tip:** After running this, go to **System Settings → General → Storage → iPhone Backups** to confirm it's cleared.

---

## STEP 3 — Delete App Data (Login Required Again)

These commands **completely wipe an app's stored data**. The app stays installed, but you'll need to log back in.

---

### 🔴 Reset Microsoft Teams

**What it is:** Teams stores meeting recordings, cached media, and app data locally.  
**Typical space saved:** 500 MB – 5 GB

```bash
rm -rf ~/Library/Containers/com.microsoft.teams2
```

🔴 **After this:** Open Teams → Log in again with your Microsoft account.

---

### 🔴 Reset WhatsApp

**What it is:** WhatsApp stores all your messages and media locally on your Mac.  
**Typical space saved:** 1–10 GB

```bash
rm -rf ~/Library/Containers/net.whatsapp.WhatsApp
```

🔴 **After this:** Open WhatsApp → Log in again by scanning the QR code with your iPhone.

> ⚠️ Your **phone's WhatsApp is unaffected** — only the Mac app data is deleted.

---

## STEP 4 — Delete Very Large Files (Big Savings)

---

### 🔴 Delete UTM Virtual Machines

**What it is:** UTM lets you run Windows or Linux inside your Mac. Those virtual machines are stored as massive files — often 10–50 GB each.  
**Typical space saved:** 10–100 GB

```bash
rm -rf ~/Library/Containers/com.utmapp.UTM
```

🔴 **After this:** All your virtual machines are gone. Only do this if you no longer use UTM, or you've exported your VMs.

---

### 🔴 Delete a Vlog / Video App Container

**What it is:** Some video apps (like Vlog by Front Row) cache enormous amounts of video data.  
**Typical space saved:** 1–20 GB

```bash
rm -rf ~/Library/Containers/maccatalyst.com.frontrow.vlog
```

🔴 **After this:** App data is wiped. Log back in if needed.

---

## STEP 5 — Delete ANY Container You Found in Step 1

If **Step 1** showed a container eating a lot of space that isn't listed above, here's how to delete it.

**Template command:**

```bash
rm -rf ~/Library/Containers/PASTE-CONTAINER-NAME-HERE
```

**Example:** If Step 1 showed `5.3G /Users/yourname/Library/Containers/com.apple.iMovieApp`, you'd run:

```bash
rm -rf ~/Library/Containers/com.apple.iMovieApp
```

> 💡 **How to paste the name easily:** In the Step 1 output, **double-click** the container name to select it, then **right-click → Copy**, then paste it into the command above.

---

## 📋 Quick Reference — All Commands in One Place

| What It Cleans | Command | Space Saved | Login Lost? |
|---|---|---|---|
| Find biggest containers | `du -hxd1 ~/Library/Containers \| sort -hr \| head -20` | — | No |
| Find large files | `sudo find / -type f -size +500M 2>/dev/null` | — | No |
| Check free space | `df -h` | — | No |
| User caches | `rm -rf ~/Library/Caches/*` | 1–10 GB | No |
| App logs | `rm -rf ~/Library/Logs/*` | 0.1–2 GB | No |
| Safari cache | `rm -rf ~/Library/Containers/com.apple.Safari/Data/Library/Caches/*` | 0.2–3 GB | No |
| iPhone backups | `rm -rf ~/Library/Application\ Support/MobileSync/Backup/*` | 10–100 GB | No |
| Microsoft Teams | `rm -rf ~/Library/Containers/com.microsoft.teams2` | 0.5–5 GB | ✅ Yes |
| WhatsApp | `rm -rf ~/Library/Containers/net.whatsapp.WhatsApp` | 1–10 GB | ✅ Yes |
| UTM Virtual Machines | `rm -rf ~/Library/Containers/com.utmapp.UTM` | 10–100 GB | ✅ Yes |
| Vlog app | `rm -rf ~/Library/Containers/maccatalyst.com.frontrow.vlog` | 1–20 GB | ✅ Yes |

---

## ✅ Recommended Order (Maximum Results, Minimum Risk)

Run these **in this order** for the safest, biggest cleanup:

1. `df -h` → note your current free space
2. `du -hxd1 ~/Library/Containers | sort -hr | head -20` → see what's biggest
3. `rm -rf ~/Library/Caches/*`
4. `rm -rf ~/Library/Logs/*`
5. `rm -rf ~/Library/Application\ Support/MobileSync/Backup/*`
6. `rm -rf ~/Library/Containers/com.apple.Safari/Data/Library/Caches/*`
7. `df -h` → check how much you've freed so far
8. Decide if you want to delete Teams / WhatsApp / UTM based on Step 2 results
9. `df -h` → final check

---

## ❓ FAQ

**Q: Will I lose my photos or documents?**  
No. These commands only touch caches, logs, and app containers — not your Desktop, Documents, Photos library, or iCloud Drive.

**Q: My Mac asked for a password. Is that normal?**  
Yes — only for the `sudo find` command. Type your Mac login password. It won't show as you type. Press Enter when done.

**Q: I accidentally deleted something I needed. Can I get it back?**  
Caches and logs: No need — they rebuild automatically. App containers (WhatsApp, Teams): The app still works, just log back in. VM files (UTM): Only recoverable from Time Machine backup.

**Q: How often should I do this?**  
Run the cache/logs/backup cleanup every 2–3 months. Run the "find biggest containers" check once a month to catch surprise storage hogs.

**Q: Nothing changed after running the commands?**  
macOS shows updated storage after a few minutes. Go to **Apple Menu → About This Mac → Storage** and wait 30 seconds for it to refresh.

---

*Guide version 1.0 — Works on macOS Ventura 13, Sonoma 14, Sequoia 15*
