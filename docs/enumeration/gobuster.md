# Gobuster

Brute-force directory, file, DNS subdomain and vhost enumeration.

---

## Directory & File Enumeration

```bash
# basic
gobuster dir -u "http://<IP>" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

```bash
# with file extensions
gobuster dir -u "http://<IP>" -w <wordlist> -x php,html,txt,js
```

```bash
# authenticated + follow redirects + high threads
gobuster dir -u "http://<IP>" -w <wordlist> -U admin -P password -r -t 64
```

```bash
# skip TLS validation (self-signed certs in CTFs)
gobuster dir -u "https://<IP>" -w <wordlist> -k
```

```bash
# save output
gobuster dir -u "http://<IP>" -w <wordlist> -o results.txt
```

!!! tip
    Gobuster does **not** recurse. If you find a directory, re-run explicitly on that path.

---

## Key `dir` Flags

| Flag | Description |
|:---:|:---:|
| `-u` | Target URL |
| `-w` | Wordlist path |
| `-x` | File extensions to look for |
| `-t` | Threads (default 10, use 64 for speed) |
| `-r` | Follow redirects |
| `-k` | Skip TLS certificate validation |
| `-o` | Output file |
| `-b` | Status codes to blacklist (e.g. `404,403`) |
| `-s` | Status codes to show |
| `--delay` | Delay between requests (evade rate limiting) |

---

## DNS Subdomain Enumeration

```bash
gobuster dns -d example.com \
  -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt

# show IPs
gobuster dns -d example.com -w <wordlist> -i
```

---

## Virtual Host Enumeration

=== "gobuster"

    ```bash
    # vhost enum — filter false positives by response size
    gobuster vhost -u "http://<IP>" \
          --domain example.thm \
          -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt \
          --append-domain \
          --exclude-length 250-320
    ```

=== "ffuf"

    ```bash
    ffuf -w <wordlist> -H "Host: FUZZ.example.thm" -u http://<IP> -fs <size>
    ```

??? tip
    Run without `--exclude-length` first to see the size of false positive responses, then filter by that size.

---

## Useful Wordlists

| Path | Use |
|:---:|:---:|
| `/usr/share/wordlists/dirb/common.txt` | Fast dir enum |
| `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` | Thorough dir enum |
| `/usr/share/wordlists/rockyou.txt` | Passwords |
| `SecLists/Discovery/DNS/subdomains-top1million-5000.txt` | Subdomains |
| `SecLists/Discovery/Web-Content/big.txt` | Web content |
