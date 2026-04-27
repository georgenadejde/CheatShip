# Resources

External references, tools, and links.

---

## Essential References

| Resource | Use |
|:---:|:---:|
| [GTFOBins](https://gtfobins.github.io) | SUID/sudo/capabilities exploit DB |
| [HackTricks](https://book.hacktricks.xyz) | Comprehensive pentesting wiki |
| [ired.team](https://www.ired.team) | Red team techniques, deep dives |
| [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) | Payload repository for all attack types |
| [exploit-db.com](https://exploit-db.com) | Public exploits and PoCs |
| [shodan.io](https://www.shodan.io) | Internet device fingerprinting |
| [dnsdumpster.com](https://dnsdumpster.com) | DNS recon |
| [crt.sh](https://crt.sh) | Certificate transparency / subdomain discovery |
| [hashes.com](https://hashes.com/en/tools/hash_identifier) | Hash identification |
| [explainshell.com](https://explainshell.com) | Explain any shell command |
| [devhints.io](https://devhints.io) | Quick cheat sheets for everything |
| [tls12.xargs.org](https://tls12.xargs.org) | Visual TLS packet walkthrough |

---

## Common Tools

| Tool | Primary Use |
|:---:|:---:|
| nmap | Port scanning + service detection |
| gobuster / ffuf | Dir/subdomain bruteforce |
| burpsuite | Web proxy, intercept, intruder |
| metasploit | Exploit framework |
| msfvenom | Payload generation |
| john / hashcat | Password / hash cracking |
| hydra | Online brute force |
| sqlmap | Automated SQL injection |
| wireshark / tcpdump | Packet capture + analysis |
| ghidra / binary ninja | Static reverse engineering |
| frida | Dynamic instrumentation |
| binwalk | Firmware extraction |
| jadx | Android APK decompiler |
| objection | Frida-based mobile pentest toolkit |

---

## Wordlists

| Path | Use |
|:---:|:---:|
| `/usr/share/wordlists/rockyou.txt` | Passwords |
| `/usr/share/wordlists/dirb/common.txt` | Fast dir enum |
| `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` | Thorough dir enum |
| `SecLists/Discovery/DNS/subdomains-top1million-5000.txt` | Subdomains |
| `SecLists/Discovery/Web-Content/big.txt` | Web content discovery |
