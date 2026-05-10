# Hospital Ransomware — Incident Response

A simulated cybersecurity incident response scenario. A hospital's network was breached, medical records encrypted for ransom, and patient data stolen. This project investigates the attack in two parts: recovering the encrypted data and performing forensic log analysis to identify the attacker and the malware used.

---

## Scenario

- 5 encrypted files found on a compromised hospital server
- A `keys` file discovered that may contain the encryption keys used
- Patient data at risk; hospital systems held for ransom
- Log files available for forensic analysis

---

## Part 1 — File Decryption & Data Recovery (`milestone2_7.py`)

Analyzed the encryption scheme and identified it as a **Vigenère cipher**. Used the `keys` file to reverse the encryption and recover the plaintext.

**Results:**
- Successfully decrypted 3 of 5 files
- Extracted **129 unique patient email addresses** from the recovered data
- Validated emails against known domains (gmail, yahoo, hotmail) and deduplicated

---

## Part 2 — Log Forensics & Malware Identification (`process_analysis_1.py`)

Analyzed Windows process log files to detect malicious activity. The script cross-references running processes against a baseline of known-good processes and flags any process consuming **>50 KB more memory than its normal baseline** — a common indicator of process injection.

**Suspicious processes identified:**
```
services.exe  |  explorer.exe  |  iexplore.exe  |  rundll32.exe
```

These are legitimate Windows processes that had been **hijacked by the Flame malware**, which injects itself into trusted processes to evade detection.

**Malware identified: Flame**

Flame is a sophisticated espionage toolkit that:
- Injects code fragments across multiple threads, hiding behind legitimate processes
- Steals files, captures screenshots, logs keystrokes
- Spreads via Wi-Fi, Bluetooth, and USB
- Can activate microphones to record and transmit audio

**Suspect identified:** Cross-referencing log timestamps with personnel schedules narrowed the attack window to a specific user active in early morning hours in November.

---

## Security Recommendations

1. Deploy up-to-date antivirus and anti-malware software
2. Regularly audit process logs and flag anomalous memory usage
3. Segment hospital networks to limit lateral movement
4. Enforce least-privilege access on all systems

---

## Tech stack

Python · Vigenère cipher · Windows process forensics · Log analysis
