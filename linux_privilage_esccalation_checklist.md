# TryHackMe — Linux Privilege Escalation (linprivesc) — Checklist & Tips

> **Purpose:** consolidated, practical checklist and tips derived from the TryHackMe "Linux Privilege Escalation" room. Use this as a step-by-step reference during labs or real-world assessments.

---

## Table of contents

1. [Overview & goals](#overview--goals)
2. [Preparation & quick environment checks](#preparation--quick-environment-checks)
3. [Enumeration checklist (high priority)](#enumeration-checklist-high-priority)
4. [Automated enumeration tools](#automated-enumeration-tools)
5. [Privilege escalation vectors & step-by-step checklists]
   - Kernel exploits
   - Sudo abuses
   - SUID binaries
   - File capabilities
   - Cron jobs
   - PATH hijacking
   - NFS / shares
   - Weak file permissions (/etc/shadow, backups, configs)
6. [Common commands & one-liners (copyable)](#common-commands--one-liners-copyable)
7. [Playbook / decision flow (quick)
8. [Hardening / defensive notes (short)](#hardening--defensive-notes-short)
9. [Further reading & references](#further-reading--references)

---

## Overview & goals

- Learn to turn a low-privilege user into root by systematically enumerating and exploiting common Linux misconfigurations and vulnerabilities.
- Focus on **enumeration first** — privileges are typically escalated with information found during early enumeration.

---

## Preparation & quick environment checks

- Confirm connectivity and how you access the target (SSH / web console / direct shell).
- Ensure you have basic tooling available locally: `gcc`, `make`, `python3`, `perl`, `nc`, `socat`, `ssh`, `scp`.
- Create workspace on attacker machine: `~/linprivesc/targets/<targetname>` and store any downloaded exploits there.

**Quick checks on target (first 30s)**
- `id` — confirm current user and groups
- `hostname && uname -a` — OS and kernel version
- `cat /etc/os-release` — distro and version
- `w` / `whoami` / `groups` — context of current session

---

## Enumeration checklist (high priority)

> Run these early and record results. Any 'yes' or unusual output is a lead.

- **System info**
  - `uname -a`, `cat /proc/version`, `cat /etc/os-release`
  - Note kernel version & distro (useful for known kernel exploits)

- **Users & home directories**
  - `ls -la /home` — look for interesting files (ssh keys, backups)
  - `cat /etc/passwd` — usernames and shells
  - `sudo -l` — what can the current user run with sudo? (high priority)

- **SUID/SGID binaries**
  - `find / -perm -4000 -type f 2>/dev/null | sort` — SUID
  - `find / -perm -2000 -type f 2>/dev/null | sort` — SGID

- **File capabilities**
  - `getcap -r / 2>/dev/null | grep -v "^$"` — capable binaries

- **Cron jobs & scheduled tasks**
  - `ls -la /etc/cron* /var/spool/cron/crontabs 2>/dev/null`
  - `crontab -l` (for current user and try other known users if you have access)

- **World or group writable files/directories**
  - `find / -writable -type d 2>/dev/null` (careful noisy/slow)
  - Check writable dirs in `$PATH`: iterate over `echo $PATH | tr ':' '\n'` and `ls -ld` each

- **Network shares / NFS**
  - `mount` and `cat /proc/mounts` — find mounted shares
  - `showmount -e <target>` from attacker (if network access)

- **Weak config files**
  - `ls -la /etc/` and look for readable backups like `shadow`, config files (e.g., `/etc/shadow`, `/etc/rsyncd.conf`, `/etc/apache2/sites-available/*`)
  - `grep -R "password" /etc 2>/dev/null` (careful, noisy)

- **Running services & listening ports**
  - `ss -tulpn` or `netstat -tulpn` — note privileged binaries binding to ports (can be exploited)

- **History & scripts**
  - `ls -la /root /home/*` for backup scripts, `*.bak`, `*.old`, `*.save`
  - `history` — check for passwords/commands used earlier

- **Installed languages & package managers**
  - `python --version`, `python3 -V`, `perl -v`, `ruby -v`, `gcc --version` — makes exploitation easier if compilers / interpreters are present

**Record everything** — copy outputs to your notes or to a single file on the target (if allowed) for later review.

---

## Automated enumeration tools

- **linpeas.sh** — highly recommended for initial deep enumeration (look for sudo, SUID, cron, interesting files). Copy to target and run: `wget -qO- https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/linPEAS/linpeas.sh | bash`

- **LinEnum.sh** — another automated enumeration script.

- **unix-privesc-check** and **GTFOBins scanner scripts** — can help identify potential misconfigurations.

> **Tip:** automated tools are good to gather leads, but **always validate manually** before exploiting. Automated output can be noisy and produce false positives.

---

## Privilege escalation vectors & step-by-step checklists

### 1) Kernel exploits

- **Goal:** check kernel version and map to known Local Privilege Escalation (LPE) CVEs.
- Steps:
  1. `uname -r` / `cat /proc/version` — save exact kernel string.
  2. Search exploit-db / GitHub for `CVE-<year>-<id>` matching the kernel. Verify patch dates and PoC availability.
  3. If a PoC exists and target has a compiler, download PoC `.c` and compile: `gcc -o exploit exploit.c -pthread` (follow PoC instructions).
  4. Run PoC carefully in non-production environment. If successful, `id` should become `root`.
  5. If kernel exploit fails or machine lacks a compiler, try cross-compiling or transferring a compiled binary.

**Safety:** kernel exploits can crash the system. Use caution and snapshot the VM if possible.

---

### 2) Sudo abuses

- **Goal:** find binaries the user can run as root (or other users) and abuse them to spawn shells or read files.
- Steps:
  1. `sudo -l` — note allowed commands and whether a password is required.
  2. For each allowed binary, check GTFOBins for abuse patterns (e.g., `nmap`, `vi`, `less`, `find`, `awk`, `python`).
  3. Prefer using **absolute path** to the binary shown by `sudo -l` (e.g., `sudo /usr/bin/nmap --script=...`).
  4. Examples: `sudo /usr/bin/nmap --script=/tmp/shell.nse` or `sudo /usr/bin/find / -exec /bin/sh -p \;` depending on GTFOBins entry.

**Tip:** If `sudo -l` allows running binaries with arbitrary arguments, you can often spawn a root shell. Always search GTFOBins first.

---

### 3) SUID binaries

- **Goal:** abuse mis-set SUID binaries to escalate privileges.
- Steps:
  1. `find / -perm -4000 -type f 2>/dev/null` — list SUID files.
  2. For each binary, consult GTFOBins for known abuse techniques.
  3. Test in a safe way: for some SUID binaries, only specific use-cases escalate; avoid destructive operations.

**Common SUID leads:** `python`, `less`, `vim`, `nmap`, `find`, `awk`, `perl`, `gcc` (rarely SUID), custom scripts.

---

### 4) File capabilities (capabilities)

- **Goal:** identify binaries with Linux capabilities that confer elevated privileges (e.g., `cap_net_bind_service` etc.).
- Steps:
  1. `getcap -r / 2>/dev/null` — list capabilities.
  2. If a binary is capable (e.g., `cap_setuid`), research how to abuse it or replace it (if writable) to escalate.

---

### 5) Cron jobs

- **Goal:** find scheduled tasks that run as root and can be abused (writable scripts, PATH injection).
- Steps:
  1. Inspect `/etc/crontab`, `/etc/cron.*/*`, and `/var/spool/cron/crontabs`.
  2. If a cron job executes a script stored in a world-writable directory, modify that script or plant a malicious script.
  3. If a cron job runs a script as root but reads files from a writeable location, use that to escalate.

---

### 6) PATH hijacking

- **Goal:** find directories in root’s or privileged user's `$PATH` that are writable by attacker.
- Steps:
  1. `echo $PATH | tr ':' '\n'` and check `ls -ld` for each path.
  2. If an earlier-in-path directory is writable, plant a binary/script with the name of an expected command; wait for privileged process to execute it.

**Tip:** many labs place an odd writable folder on PATH for this exact scenario.

---

### 7) NFS shares & `no_root_squash`

- **Goal:** identify export options that allow root mapping from client side.
- Steps:
  1. On attacker machine: `showmount -e <target>` to list exports.
  2. Mount shares and inspect `/etc/exports` options if accessible. Look for `no_root_squash`.
  3. If `no_root_squash` is present and you can mount the share as root on your client, you can create a root-owned SSH key in `/root/.ssh/authorized_keys` or replace files.

---

### 8) Weak file permissions (e.g., `/etc/shadow`, backups)

- **Goal:** locate readable sensitive files and use them to crack or retrieve passwords.
- Steps:
  1. Check `/etc/shadow` readability; if readable, copy and crack the hash offline with `hashcat` or `john`.
  2. Search for backup files in web directories or home directories: `*.bak`, `*.old`, `*.save`, `*~`.
  3. Look for `.ssh` private keys (`id_rsa`) with weak passphrases or unsecured permissions.

---

## Common commands & one-liners (copyable)

```
# Basic system info
id; uname -a; cat /etc/os-release

# Sudo check
sudo -l

# SUID / SGID
find / -perm -4000 -type f 2>/dev/null | sort
find / -perm -2000 -type f 2>/dev/null | sort

# Capabilities
getcap -r / 2>/dev/null

# Cron locations
ls -la /etc/cron* /var/spool/cron/crontabs 2>/dev/null

# World writable dirs
find / -xdev -type d -writable 2>/dev/null

# Search for interesting files
find /home -maxdepth 3 -type f \( -name "*.bak" -o -name "*.old" -o -name "*.save" \) 2>/dev/null

# Quick find readable shadow (dangerous - only for lab)
ls -l /etc/shadow 2>/dev/null && cat /etc/shadow 2>/dev/null

# Show PATH entries and permissions
echo $PATH | tr ':' '\n' | xargs -I{} bash -c "ls -ld {}"
```

---

## Playbook / decision flow (quick)

1. **Enumeration** — gather everything with basic commands + linpeas.
2. **Sudo** — `sudo -l` (high priority). If any usable binary, check GTFOBins and attempt safe exploit.
3. **SUID & Capabilities** — look for SUID/capable binaries and research.
4. **Cron & PATH** — check for writeable scripts or PATH entries.
5. **NFS & shares** — check mounts and exports for `no_root_squash`.
6. **Kernel exploit** — only after confirming kernel & PoC availability.
7. **Persistence / cleanup** — once root, collect flags and then document steps taken (don’t leave destructive changes in lab environments).

---

## Hardening / defensive notes (short)

- Limit `sudo` to specific commands and avoid allowing editors or network tools to run as root.
- Ensure no SUID binaries with unnecessary privileges exist; monitor changes.
- Remove world-writable directories from privileged PATHs; use full paths in cron.
- Configure `no_root_squash` carefully and avoid exposing NFS to untrusted networks.

---

## Further reading & references

- TryHackMe — Linux Privilege Escalation (lab content & tasks)
- GTFOBins — unix binaries used for privilege escalation
- PEASS-ng / linPEAS — automated enumeration
- Exploit-DB / public PoC repositories for kernel LPEs


---

> **Notes:** This checklist is written for lab/learning purposes (TryHackMe). Always follow legal and ethical rules when testing real networks or systems.



