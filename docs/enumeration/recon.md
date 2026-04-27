# Passive Recon

OSINT, subdomain discovery, service fingerprinting — without touching the target directly.

---

## OSINT Tools

| Tool / Site | Use |
|---|---|
| [crt.sh](https://crt.sh) | SSL cert transparency logs — find subdomains |
| [dnsdumpster.com](https://dnsdumpster.com) | DNS records, subdomains, visual map |
| [shodan.io](https://www.shodan.io) | Internet-connected device fingerprinting |
| [who.is](https://who.is) | WHOIS registration data |
| [archive.org](https://archive.org) | Wayback Machine — old site versions |
| [browserleaks.com](https://browserleaks.com) | Browser/IP fingerprinting |

---

## Google Dorks

```
# find subdomains
site:*.example.com -site:www.example.com
```
```
# exposed files
site:example.com filetype:pdf
site:example.com filetype:env
```

```
# login pages
site:example.com inurl:login
```

```
# exposed git repos
site:example.com inurl:.git
```

---

## DNS Lookup

```bash
nslookup example.com
```

```bash
dig example.com
```

```bash
dig example.com ANY        # all record types
```

```bash
dig @8.8.8.8 example.com  # use specific DNS server
```

---

## Active Recon Tools

```bash
# subdomain brute force
sublist3r -d example.com
```

=== "Linux"
    ```bash
    # traceroute
    traceroute <IP>
    ```

=== "Windows"
    ```cmd
    tracert <IP>
    ```