---
title: How I Set Up Syncthing Between My Windows PC and Mac
date: 2026-07-19
tags: [syncthing, sync, workflow, windows, macos]
---

# How I Set Up Syncthing Between My Windows PC and Mac

I use two machines. My Windows desktop at home does the heavy stuff VMs, cert labs, dissertation work. My MacBook comes with me when I'm out.

For code, git is fine. But everything else including notes, PDFs, half-done writing, random folders kept getting out of sync. I'd edit something on one machine and forget it existed on the other.

I finally sorted it this weekend using Syncthing.

## Why Syncthing?

Cloud stuff like Dropbox and OneDrive works, but you're paying monthly and giving your files to someone else. Syncthing is different. It's peer-to-peer. Your files go directly from one machine to the other. No cloud in the middle. Free. Open source.

That's the pitch. Here's how it actually went.

## Installing It

**Windows:**

```
winget install Syncthing.Syncthing
```

**Mac:**

```
brew install syncthing
```

To run it on Mac, you've got two options:

- `syncthing --no-browser` in Terminal — runs in the foreground and stops when you close the terminal.
- `brew services start syncthing` — runs in the background all the time, even after reboot.

On Windows, just launch it from the Start menu.

Both machines then have a web UI at `http://127.0.0.1:8384`. Everything happens there.

## Pairing the Machines

Every device has a unique ID. Grab it from **Actions → Show ID** in the top right.

Then:

1. On Mac, click **Add Remote Device** and paste the Windows ID. Give it a name.
2. On Windows, a notification pops up asking to accept. Accept it.
3. Do the same the other way round.

Once both accept each other, you'll see "Connected" in green.

## Sharing a Folder

On the source machine (Windows for me), click **Add Folder**:

- Give it a label.
- Set the path (`Path_to_Folder`).
- Go to the **Sharing** tab and tick the Mac.
- Save.

On the Mac, a yellow bar shows up saying the Windows machine wants to share a folder. Click **Add**.

This is where I messed up. More on that below.

## Ignore Patterns Matter

Syncthing lets you ignore files and folders. Don't skip this bit.

For code:

```
.git
node_modules
.venv
__pycache__
*.log
.DS_Store
```

**Why `.git`?** Git has thousands of tiny files in there that change every time you touch anything. If Syncthing tries to sync them in real time while you're doing a `git commit`, you can end up with a broken repo. Just let git do git stuff (push and pull between machines). Let Syncthing handle everything else.

**Why `node_modules` and `.venv`?** They're huge and you can rebuild them anytime with `npm install` or `python -m venv`. Also, a Python venv from Windows won't even work on Mac.

**Why `.DS_Store`?** Mac creates these hidden files everywhere. No point copying them to Windows.

## What About BitLocker?

My Windows drive has BitLocker on it. Does that stop Syncthing?

Nope. BitLocker only protects your files when the machine is off. Once Windows boots and the drive unlocks (TPM does this automatically), files are readable normally. Syncthing sees them fine.

One thing worth knowing that BitLocker doesn't come with the files. Once they land on the Mac, they're not encrypted anymore. If you want the Mac side protected too, turn on FileVault (Settings → Privacy & Security → FileVault).

## Where I Messed Up

When the "accept share" popup came up on my Mac, the folder path was already filled in. I didn't check it. I just hit Save.

Turns out it defaulted to my Mac's home folder (`~`). So Syncthing thought: cool, let's sync the ENTIRE home folder back to Windows.

Everything. `.ssh`, `.config`, Documents, Downloads, Library, Applications. All of it. 8.34 GiB. 81,000+ files.

I only noticed when the Windows folder started filling up with weird stuff like `.dropbox`, `.bash_sessions`, and `.Trash`. That's not what my code folder looks like.

**How I fixed it:**

1. Hit **Pause All** on the Mac straight away. Stops the sync.
2. Edit the folder → **Remove**. This stops Syncthing tracking it but doesn't delete the actual files.
3. Same on Windows.
4. Delete the mess that ended up on Windows manually.
5. Start again — but this time, make an empty target folder on the Mac first: `mkdir -p ~/CODE`. Then when the popup comes up, check that the path says `/Users/yourname/CODE`, not just `/Users/yourname`.

Nothing was lost on the Mac. Syncthing's "Remove" doesn't touch files. I just had to clean up Windows and redo the last step properly.

**Lesson:** always check the destination path. Always use an empty folder. Never accept into your home folder.

## Then I Added Coursework and VMs Were a Problem

The next folder I added was my University of Westminster stuff. Turned out it had lab VMs in it. Multi-GB `.vmdk` files, `.vmem` runtime state, WinRAR archives of Windows Server images.

You really don't want VMs syncing:

- They're massive.
- Runtime state files change constantly while the VM is running, so Syncthing never stops re-syncing them.
- A Windows VMware VM won't run on Mac anyway.

So I added a bigger ignore list:

```
.DS_Store
*.log
*.vmdk
*.vmx
*.vmxf
*.vmem
*.vmsd
*.vmss
*.nvram
*.vswp
*.scoreboard
*.vbox
*.vbox-prev
*.vdi
*.vhd
*.vhdx
*.ova
*.ovf
*.rar
*.zip
*.7z
*.iso
VMs
```

The bare `VMs` name matches any folder called that anywhere in the tree. Easier than listing every path.

## What I've Got Now

- `D:\LRN\CODE` (Windows) ↔ `~/CODE` (Mac) — code and learning stuff.
- `D:\UOW` (Windows) ↔ `~/UOW` (Mac) — coursework, no VMs.
- Git still handles actual repo history.
- No cloud. Files move directly when both machines are online. If they're on different networks, Syncthing uses relay servers (a bit slower).

## What I Learned

1. **Always check the destination path** when accepting a share. Don't just click Save.
2. **Ignore patterns are not optional.** Especially `.git`, `node_modules`, and VMs.
3. **Syncthing doesn't replace git.** Use both — Syncthing for working files, git for version history.
4. **Both machines need to be on.** No cloud middleman means no sync while one is off.

If you're bouncing between two of your own machines and you don't want to pay Dropbox or hand your files to someone, Syncthing is worth an hour.

##### A supporter is worth a thousand followers! [Buy Me a Coffee](https://www.buymeacoffee.com/dx73r). If you like this blog, follow me on [GitHub](https://github.com/dx7er) and [LinkedIn](https://www.linkedin.com/in/dx73r/). 