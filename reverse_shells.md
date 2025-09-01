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
- **PentestMonkey — Reverse Shell Cheat Sheet** — [pentestmonkey.net](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
- **PayloadsAllTheThings — Reverse Shell Cheatsheet** — [github.com/swisskyrepo/PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet)
- **HighOn.Coffee — Reverse Shell Cheat Sheet (blog)** — [highon.coffee/blog/reverse-shell-cheat-sheet](https://highon.coffee/blog/reverse-shell-cheat-sheet/)

## 2) Collections & repositories
- **SecLists (danielmiessler)** — [github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)
- **PayloadsAllTheThings (swisskyrepo)** — [github.com/swisskyrepo/PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- **php-reverse-shell (pentestmonkey/php-reverse-shell)** — [github.com/pentestmonkey/php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell)
- **reverse-shells (nicholasaleks)** — [github.com/nicholasaleks/reverse-shells](https://github.com/nicholasaleks/reverse-shells)
- **Gists & Cheat Gists** — [gist.github.com](https://gist.github.com/search?q=reverse+shell)

## 3) Language / tool-specific reverse shells (examples & resources)
- **Bash / sh / POSIX TCP** — [GTFOBins bash](https://gtfobins.github.io/gtfobins/bash/)
- **Netcat / nc / ncat** — [GTFOBins nc](https://gtfobins.github.io/gtfobins/nc/)
- **Socat** — [GTFOBins socat](https://gtfobins.github.io/gtfobins/socat/)
- **Python** — [GTFOBins python](https://gtfobins.github.io/gtfobins/python/)
- **Perl** — [GTFOBins perl](https://gtfobins.github.io/gtfobins/perl/)
- **PHP** — [php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell)
- **Ruby** — [GTFOBins ruby](https://gtfobins.github.io/gtfobins/ruby/)
- **Golang** — [revshellgen Golang examples](https://github.com/0dayCTF/reverse-shell-generator)
- **Java / JSP** — [PayloadsAllTheThings JSP shells](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Web%20Shells)
- **PowerShell (Windows)** — [PayloadsAllTheThings PowerShell](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Methodology%20and%20Resources/Windows%20-%20PowerShell)
- **CMD / native Windows** — [LOLBAS cmd.exe](https://lolbas-project.github.io/lolbas/Binaries/Cmd/)

## 4) Web shells & uploads
- **Web‑Shells lists (SecLists)** — [SecLists/Web-Shells](https://github.com/danielmiessler/SecLists/tree/master/Web-Shells)
- **Kali Webshells package** — [Kali Linux Webshells](https://gitlab.com/kalilinux/packages/webshells)
- **php-reverse-shell** — [pentestmonkey/php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell)

## 5) Binaries & GTFOBins / LOLBAS
- **GTFOBins** — [gtfobins.github.io](https://gtfobins.github.io/)
- **LOLBAS** — [lolbas-project.github.io](https://lolbas-project.github.io/)

## 6) Generators & tooling
- **reverse-shell-generator (online & CLI)** — [revshells.com](https://www.revshells.com/)
- **revshellgen / revshell scripts** — [github.com/0dayCTF/reverse-shell-generator](https://github.com/0dayCTF/reverse-shell-generator)
- **Metasploit / msfvenom** — [Metasploit Framework](https://github.com/rapid7/metasploit-framework)

## 7) Further reading / tutorials
- **0xffsec / 0xffsec handbook** — [0xffsec Handbook](https://0xffsec.com/handbook/shells/reverse-shell/)
- **Spawning TTYs & stabilizing shells** — [HighOn.Coffee TTY tricks](https://highon.coffee/blog/reverse-shell-cheat-sheet/#tty-shells)

## 8) Notes & cautions
- **Legal / ethical**: Use these resources only on systems you own or have explicit permission to test. Unauthorized access is illegal and unethical.
- **AV / detection**: Some repo contents may trigger AV alerts — treat artifacts carefully and avoid running on production systems.
- **Environment differences**: Not every one-liner works on all systems (e.g., `/dev/tcp` not available, `nc -e` missing). Prefer multiple variants.

---

## Quick license & contribution note
This document is intended to be a neutral index of public resources. When rehosting or quoting content, respect original licenses (MIT, GPL, etc.) and attribution where required.

---

*Generated: a GitHub-ready markdown index of reverse-shell resources.*

