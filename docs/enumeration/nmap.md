# Nmap

Network scanner. Port discovery, service version detection, NSE scripts, firewall evasion.

---

## Quick Reference

```bash
# fast default scan
nmap -sV -sC -oN scan.txt <IP>
```

```bash
# full port scan + version + scripts
nmap -p- -sV -sC -T4 -oA full <IP>
```

```bash
# UDP top 20
nmap -sU --top-ports 20 <IP>
```

```bash
# Decoy scan
nmap -D <DECOY_IP>,ME <MACHINE_IP>
```

```bash
# Custom TCP scan
sudo nmap --scanflags URGACKPSHRSTSYNFIN MACHINE_IP
```

```bash 
# Spoofed Source IP
sudo nmap -S <SPOOFED_IP> <MACHINE_IP>
```

```bash
# ping sweep (host discovery)
nmap -sn 192.168.0.0/24
```

---

## Key Flags

| Flag | Description |
|:---:|:---:|
| `-sS` | SYN/stealth scan (default w/ sudo) |
| `-sT` | TCP connect scan (default without sudo) |
| `-sU` | UDP scan (slow) |
| `-sV` | Service version detection |
| `-sC` | Default NSE scripts |
| `-O` | OS detection |
| `-p-` | All 65535 ports |
| `-p 80,443` | Specific ports |
| `-T0..5` | Timing (T4 = fast, T5 = insane/noisy) |
| `-oN / -oG / -oA` | Output: normal / grepable / all formats |
| `-Pn` | Skip host discovery (bypass ICMP block) |
| `-v / -vv` | Verbose output |
| `-d / -d` | Debug details |
|`-reason` |	Explains how Nmap made its conclusion|

---

## NSE Scripts

```bash
# run a category
nmap --script=vuln <IP>
```

```bash
# run a specific script
nmap --script=http-fileupload-exploiter <IP>
```

```bash
# multiple scripts
nmap --script=smb-enum-users,smb-enum-shares <IP>
```

```bash
# script with arguments
nmap -p 80 --script http-put \
  --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php' <IP>
```

```bash
# find scripts locally
grep "ftp" /usr/share/nmap/scripts/script.db
ls /usr/share/nmap/scripts/*ftp*
```

```bash
# install a new script
sudo wget -O /usr/share/nmap/scripts/<name>.nse \
  https://svn.nmap.org/nmap/scripts/<name>.nse

nmap --script-updatedb
```
??? info "Script categories"
    Most important: `safe`, `intrusive`, `vuln`, `exploit`, `auth`, `brute`, `discovery`

---

## Stealth & Firewall Evasion

```bash title="Null / FIN / XMAS — bypass SYN-filtering firewalls"
nmap -sN <IP>   # Null  — no flags set
nmap -sF <IP>   # FIN
nmap -sX <IP>   # XMAS  (PSH+URG+FIN)
```
```bash
# fragment packets (evade some IDS)
nmap -f <IP> # (1)!
```

1. `-ff` to fragment data into 16 bytes 

```bash
# control MTU (must be multiple of 8)
nmap --mtu 16 <IP>
```

```bash
# add scan delay
nmap --scan-delay 500ms <IP>
```

```bash
# detect firewall (bad checksum: legit hosts won't respond)
nmap --badsum <IP> # (1)!
```

1.  Sends packets with invalid checksums. Legit hosts drop them silently. Firewalls often respond automatically, revealing themselves.

```bash
# append random data to packets
nmap --data-length 25 <IP>
```

!!! warning
    NULL (`-sN`) / FIN (`-sF`) / XMAS (`-sX`) show ports as `open|filtered`. Windows often responds with RST for all ports regardless, making results unreliable.
