# 🕵️‍♂️ Digital Forensics Investigation: Quantum Processor Data Leak

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![Tools](https://img.shields.io/badge/Tools-Autopsy%20%7C%20John%20The%20Ripper-red)
![Status](https://img.shields.io/badge/Case-Solved-success)

## 📌 Project Overview
This repository documents the digital forensic investigation of a data leakage incident involving the theft of intellectual property ("Quantum Processor Design"). The investigation focuses on analyzing a disk image (`johns.001`), recovering deleted artifacts, tracing financial motives, and decrypting secured files used to exfiltrate data.

**Course:** Cyber Security (CAK3CAB3)  
**Role:** Digital Forensic Investigator  

---

## 📂 Case Summary
* **Incident:** Suspected theft of "Top Secret" company documents.
* **Suspect:** John (Employee).
* **Evidence Analyzed:** Disk Image `johns.001`.
* **Key Findings:**
    * Recovered deleted source document (`desainXYZ123.pdf`).
    * Identified anti-forensic techniques (file deletion & encryption).
    * Cracked passwords for exfiltrated files using dictionary attacks.
    * Confirmed financial motive (proof of bank transfers totaling IDR 500 Million).

---

## 🛠️ Tools & Technologies
The investigation utilized the following tools:
* **[Autopsy](https://www.autopsy.com/):** For disk image ingestion, file recovery (carving), and timeline analysis.
* **[John the Ripper (Jumbo)](https://github.com/openwall/john):** For brute-forcing password hashes.
* **Python 3:** For running hash extraction scripts.
* **Libraries:** `olefile`, `pyhanko`.

---

## 🔍 Investigation Methodology

### 1. Evidence Acquisition & Integrity
* Verified the integrity of the disk image.
* Calculated MD5 hashes of extracted artifacts to maintain the Chain of Custody.

### 2. File Recovery
* Located and recovered the deleted file `desainXYZ123.pdf` from the unallocated space using Autopsy.
* Confirmed the file contained "Top Secret" watermarks.

### 3. Cryptanalysis (Password Cracking)
The suspect encrypted the stolen data before distribution. We utilized custom Python scripts to extract hashes and cracked them using a dictionary attack (`rockyou.txt`).

| File | Type | Encryption | Password Found |
| :--- | :--- | :--- | :--- |
| `URL.docx` | MS Word | Office CryptoAPI | **`open`** |
| `desainXYZ123__enc.pdf` | PDF | AES-128 | **`secret`** |

### 4. Timeline Reconstruction
* **20:26 WIB:** Document created/copied and original file deleted.
* **22:27 WIB:** Suspect received final payment after data delivery.

---

## 💻 Custom Scripts

This repository includes utility scripts used during the investigation.

### 🔓 Office2John (`office2john.py`)
This Python utility is designed to extract password hashes from encrypted Microsoft Office documents. It serves as a crucial bridge between locked files and password cracking tools.

**Features:**
* **Broad Compatibility**: Supports both legacy binary formats (Office 97-2003: `.doc`, `.xls`, `.ppt`) and modern XML-based formats (Office 2007+: `.docx`, `.xlsx`).
* **Encryption Detection**: Automatically parses OLE structures to identify the encryption type (RC4, CryptoAPI, or AES).
* **JtR Ready**: Outputs a formatted hash string compatible with John the Ripper.

**Usage:**
```bash
python3 office2john.py encrypted_file.docx > hash.txt
