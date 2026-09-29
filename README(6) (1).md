# Cryptographic Encryption Vault

A Python-based directory encryption tool that recursively encrypts and decrypts files using **Fernet authenticated encryption**. The encryption key is derived from a master passphrase with **PBKDF2-HMAC-SHA256**.

> Use only on files you own or are authorized to protect. Never publish passwords or key-backup files on GitHub.

## Objective
Build a multi-file directory encryption tool with secure key-management practices.

## Features
- Recursive directory scanning
- Fernet authenticated encryption
- PBKDF2-HMAC-SHA256 key derivation
- Random per-vault salt
- Encrypt and decrypt modes
- Execution summary and logs
- Local recovery-key backup

## Requirements
Python 3.9+ and the `cryptography` package.

```bash
pip install cryptography
```

## Run

Encrypt:
```bash
python vault.py encrypt test_vault
```

Decrypt:
```bash
python vault.py decrypt test_vault
```

The same master passphrase must be supplied for decryption.

## Sample Execution

```text
$ python vault.py encrypt test_vault
Master passphrase: ********
Confirm passphrase: ********
ENCRYPTED: data/sample.csv -> data/sample.csv.enc
ENCRYPTED: notes.txt -> notes.txt.enc
ENCRYPTED: report.txt -> report.txt.enc

=== VAULT SUMMARY ===
Mode: encrypt
Files processed: 3
Status: SUCCESS

$ python vault.py decrypt test_vault
Master passphrase: ********
DECRYPTED: data/sample.csv.enc -> data/sample.csv
DECRYPTED: notes.txt.enc -> notes.txt
DECRYPTED: report.txt.enc -> report.txt

=== VAULT SUMMARY ===
Mode: decrypt
Files processed: 3
Status: SUCCESS
```

## Execution Screenshots

![Encryption execution](screenshots/encryption_execution.png)

![Decryption execution](screenshots/decryption_execution.png)

These are sample execution screenshots with the passphrase hidden.

## How It Works
1. A random 16-byte salt is generated for a new vault.
2. PBKDF2-HMAC-SHA256 derives a 32-byte key from the master passphrase.
3. Fernet encrypts each file independently.
4. Encrypted files receive the `.enc` extension.
5. During decryption, Fernet verifies the ciphertext before restoring the file.
6. A summary is printed after processing.

## Key Management
The master passphrase is never stored. A `.vault_salt` file stores the salt needed for key derivation. The program also creates `.vault_key_backup.txt` for authorized recovery.

**Do not upload `.vault_key_backup.txt` to a public GitHub repository.** Treat it as a secret because possession of the derived key allows decryption.

## Submission Files
- `vault.py` — source code
- `execution_log.txt` — sample execution log
- `screenshots/encryption_execution.png` — encryption proof
- `screenshots/decryption_execution.png` — decryption proof
- `README.md` — project documentation

## Author
Sai Nikitha
