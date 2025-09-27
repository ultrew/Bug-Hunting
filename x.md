# Task 7 — Static Malware Analysis
## Internship: Broskieshub.com 
## Overview
I performed a static analysis of the provided sample ( which I created using ai ) without executing it. All work was done on my Kali host using `strings`, `hexdump`, Ghidra, and VirusTotal (hash lookup only). This README explains what I checked, what I found, and how to reproduce my steps.
 
---

## Files I included
- `Evidence/malware` — the ELF sample I analyzed
- `Evidence/malware.c` — source used to build the sample
- `Evidence/hashes.txt` — recorded SHA256 / SHA1 / MD5
- `Evidence/strings_output.txt` — `strings` output
- `Evidence/hexdump_head.txt` — hexdump excerpt showing embedded marker
- `Evidence/file_type.txt`, `Evidence/readelf_header.txt`, `Evidence/objdump_full.txt` — file metadata
- `Screenshots/` — minimal screenshots: hashes, key strings, hexdump MZ offset, Ghidra strings, Ghidra decompile, VirusTotal lookup

---

## Summary of findings (short)
- **File type:** ELF 64-bit LSB PIE executable, x86-64, stripped.
- **Hashes:**  
  - SHA256: `70daf232c4baa313714d241fb5e8adc2458d34f94a895757379a915ab6c497e3`  
  - SHA1: `ae6c836e8a68f77eac712f08d81d0fced006cbc9`  
  - MD5: `79463907f988db78e7f383669cc422e7`
- **Key indicators:**  
  - I found an ASCII `MZ` sequence inside the ELF (suggests an embedded PE-like blob).  
  - Found suspicious literal strings `ThisLooksLikePE|cmd.exe /c whoami /all|CONNECT_BACK` and a base64-like fragment `UEsDBBQAAAAI`.  
  - Ghidra shows these strings in the Defined Strings pane and they are referenced by `main()`; the ELF binary itself contains no obvious socket or process-launching imports.

---

## How I reproduced my work (commands I ran)
> Run these in a controlled environment.

```bash
# go to the folder containing the extracted Task7 files
cd ./Task7/Evidence

# compute hashes
sha256sum malware | tee hashes.txt
sha1sum malware >> hashes.txt
md5sum malware  >> hashes.txt
cat hashes.txt

# extract readable strings
strings -a -n 4 malware | tee strings_output.txt

# hexdump to inspect bytes and find embedded MZ
hexdump -C malware | less
# quick locate MZ occurrences (prints byte offsets in decimal)
grep -b -a -o $'\x4d\x5a' malware

# file metadata / headers
file malware > file_type.txt
readelf -h malware > readelf_header.txt
objdump -x malware > objdump_full.txt

# manually inspect in Ghidra:
# - ghidraRun
# - File -> New Project -> Import ./malware -> run auto-analysis
# - Window -> Defined Strings (look for MZ/cmd/CONNECT_BACK)
# - Open main() and press F for decompiled pseudocode
````
---

## Screenshots I included

I included only the essential screenshots in `Screenshots/`:
0. `00_listing_after_compile.png` - Compilation of binary
1. `01_hashes.png` — hash outputs
2. `02_strings.png` — `strings` output with the suspicious literals
3. `03_hexdump.png` — hexdump showing `MZ` offset `0x2008`
4. `04_file_info.png` - file info
5. `05_defined_strings.png` — Ghidra defined strings pane
6. `06_function_and_imports.png` — Ghidra decompiled `main()` and imports
7. `07_virus_total.png` — VirusTotal hash lookup result


## Safety note (I follow this)

I did **not** execute the sample. I only performed static inspection. I recommend doing any dynamic or hostile-sample work inside isolated VMs or dedicated sandbox infrastructure.

---

End of report.
