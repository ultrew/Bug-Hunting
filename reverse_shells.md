# Reverse Shell — Links & Resources

_A GitHub-ready collection of links covering all types of shells (reverse, bind, webshells, spawn/TTY tricks) and related resources._

---

## Table of Contents
1. Primary cheat sheets
2. Collections & repos
3. Language / tool-specific reverse shells
4. Web shells & uploads
5. Binaries & GTFOBins / LOLBAS
6. Generators & tooling
7. Further reading / tutorials
8. Notes & cautions

---

## 1) Primary cheat sheets
- **PentestMonkey — Reverse Shell Cheat Sheet** — A compact one‑liner focused cheat sheet covering bash, perl, python, php, ruby, etc. Useful for quick copy/paste examples.
- **PayloadsAllTheThings — Reverse Shell Cheatsheet** — A curated collection and methodology page with many variants and links to supplementary payloads.
- **HighOn.Coffee — Reverse Shell Cheat Sheet (blog)** — Beginner-friendly article that groups Windows and Linux reverse shells by language/tool.

## 2) Collections & repositories
- **SecLists (danielmiessler)** — Large repo of wordlists and a `Web-Shells` section; useful for discovery and webshell payloads.
- **PayloadsAllTheThings (swisskyrepo)** — Broad repository of payloads including command injection and reverse shell examples.
- **php-reverse-shell (pentestmonkey/php-reverse-shell)** — Single-file PHP reverse shell project; commonly used as an uploadable webshell.
- **reverse-shells (nicholasaleks)** — A GitHub repo collecting many reverse shell one‑liners across languages.
- **Gists & Cheat Gists** — various community gists that collect miscellaneous one‑liners and spawn/TTY tricks.

## 3) Language / tool-specific reverse shells (examples & resources)
- **Bash / sh / POSIX TCP** — classic `/dev/tcp` and FD redirection one-liners; widely supported on Linux.
- **Netcat / nc / ncat** — bind and reverse shells using netcat variants; note `-e` availability differs per build.
- **Socat** — flexible for SSL/TLS and reliable PTY allocation (spawning TTYs).
- **Python** — `socket` + `os.dup2` method for interactive shells.
- **Perl** — `Socket`-based one-liners with FD redirection.
- **PHP** — `fsockopen` and `proc_open` variants for web-hosted shells.
- **Ruby** — `TCPSocket` based one-liners.
- **Golang** — compiled or small stagers for constrained environments.
- **Java / JSP** — JSP-based webshells and reverse shells for JVM hosts.
- **PowerShell (Windows)** — robust interactive reverse shells, including encrypted/obfuscated variants.
- **CMD / native Windows** — `cmd.exe` one-liners, Metasploit stagers, and `CertUtil`/`bitsadmin` helpers for payload download.

## 4) Web shells & uploads
- **Web‑Shells lists (SecLists)** — curated filenames/paths for webshell detection and common uploadable shells (PHP, ASP, JSP, etc.).
- **Kali Webshells package** — distro package containing many uploadable webshells for testing.
- **php-reverse-shell** — the canonical PHP reverse shell file referenced often in writeups.

## 5) Binaries & GTFOBins / LOLBAS
- **GTFOBins** — catalog of Unix binaries that can spawn shells, escalate privileges, do reverse shells when available on a host.
- **LOLBAS** — similar catalog for Windows binaries and living-off-the-land techniques that can be leveraged to spawn shells.

## 6) Generators & tooling
- **reverse-shell-generator (online & CLI)** — online generators and small CLI tools that generate one‑liners for you with chosen LHOST/LPORT.
- **revshellgen / revshell scripts** — community CLI tools that print variants per language.
- **Metasploit / msfvenom** — advanced payload generation and multi-platform stagers.

## 7) Further reading / tutorials
- **0xffsec / 0xffsec handbook** — conceptual explanations on reverse vs bind shells, TTY spawning, and post‑exploitation tips.
- Blog posts and video walkthroughs for spawning TTYs and stabilizing shells using `python -c 'import pty; pty.spawn("/bin/bash")'`, `stty raw -echo`, and `Ctrl-Z` job control techniques.

## 8) Notes & cautions
- **Legal / ethical**: Use these resources only on systems you own or have explicit permission to test. Unauthorized access is illegal and unethical.
- **AV / detection**: Some repo contents may trigger AV alerts — treat artifacts carefully and avoid running on production systems.
- **Environment differences**: Not every one-liner works on all systems (e.g., `/dev/tcp` not available, `nc -e` missing). Prefer multiple variants.

---

## Quick license & contribution note
This document is intended to be a neutral index of public resources. When rehosting or quoting content, respect original licenses (MIT, GPL, etc.) and attribution where required.

---

*Generated: a GitHub-ready markdown index of reverse-shell resources.*

