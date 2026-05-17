# PaySure Cybersecurity Project Report

> **Simulated enterprise security engagement across four phases: secure communications, password auditing, phishing analysis, and executive reporting.**

---

## Table of Contents

1. [Phase 1 — Secure Messaging Implementation](#phase-1-secure-messaging-implementation)
2. [Phase 2 — Password Security Audit](#phase-2-password-security-audit)
3. [Phase 3 — Phishing, Base64 & Punycode Attack Simulation](#phase-3-phishing-base64--punycode-attack-simulation)
4. [Phase 4 — Security Report & Recommendations](#phase-4-security-report--recommendations)

---

## Phase 1: Secure Messaging Implementation

### Objective
Ensure confidentiality, integrity, and authenticity of internal communications at PaySure by implementing GPG encryption and digital signatures between two users: Alice and Bob.

### Tools Used
- Kali Linux
- GnuPG (GPG)
<img width="920" height="610" alt="image" src="https://github.com/user-attachments/assets/85e6416e-c556-4e52-a0c7-2afe88129029" />

---

### Step 1: GPG Key Generation

Two GPG key pairs were generated — one for Alice and one for Bob. Each key pair consists of a public key (shared) and a private key (kept secret).

**Command:**
```bash
gpg --full-generate-key
```

**Key Configuration:**
- Key Type: RSA and RSA
- Key Size: 4096 bits
- Expiration: Never
- User IDs:
  - `Alice <alice@paysure.com>`
  - `Bob <bob@paysure.com>`
<img width="900" height="931" alt="image" src="https://github.com/user-attachments/assets/605e4932-8722-476e-8044-5311009aa31f" />
<img width="900" height="936" alt="image" src="https://github.com/user-attachments/assets/1e93088e-7cf9-45dd-bfc3-930b4d3712b1" />

---

### Step 2: Public Key Export and Exchange

Public keys were exported and securely exchanged between Alice and Bob.

```bash
gpg --export -a alice@paysure.com > alice_public.key
gpg --export -a bob@paysure.com > bob_public.key
```
<img width="900" height="123" alt="image" src="https://github.com/user-attachments/assets/f0d9994c-439f-4aa7-944e-efcdd60f27a5" />

---

### Step 3: Public Key Import

Each user imported the other's public key into their keyring.

```bash
gpg --import bob_public.key
gpg --import alice_public.key
```
<img width="900" height="213" alt="image" src="https://github.com/user-attachments/assets/78008815-827f-414e-bec5-cea0ab2e33ae" />

---

### Step 4: Message Encryption and Signing

Alice created a plaintext message, encrypted it using Bob's public key, and digitally signed it with her private key.

```bash
nano message.txt
gpg --encrypt --sign --recipient bob@paysure.com message.txt
```
<img width="900" height="934" alt="image" src="https://github.com/user-attachments/assets/d81ef4e3-71ee-4caf-b3ea-9c600ae26f30" />
<img width="900" height="937" alt="image" src="https://github.com/user-attachments/assets/bd8eaadb-47f1-43e0-9e0c-9cc6ff72c6ac" />


---

### Step 5: Message Decryption and Signature Verification

Bob decrypted the message using his private key and verified Alice's digital signature.

```bash
gpg --decrypt message.txt.gpg
```

**Result:**
- Message successfully decrypted
- Signature verified as authentic from Alice
<img width="900" height="180" alt="image" src="https://github.com/user-attachments/assets/65cb0d0f-8903-4ad3-bcb2-11809ca1dea5" />

---

### Extra Task: Security Failure Simulation

Two failure scenarios were tested:

1. **Decryption without private key**
   - Outcome: Without Bob's private key, the message remains unreadable — confirming **confidentiality**.
<img width="900" height="365" alt="image" src="https://github.com/user-attachments/assets/85842368-45d0-49f2-b37f-92980709e4f1" />
<img width="900" height="576" alt="image" src="https://github.com/user-attachments/assets/ee49d32f-ab01-4d5e-90cd-004c82057cb4" />
<img width="900" height="128" alt="image" src="https://github.com/user-attachments/assets/48ad6dd0-a944-4c6c-8bb0-a5c423e56f3f" />



2. **Receiving an encrypted message without a digital signature**
   - Outcome: The message decrypted successfully but displayed no valid signature, proving that **encryption alone does not guarantee sender authenticity**.
 <img width="900" height="56" alt="image" src="https://github.com/user-attachments/assets/f16294f4-a660-495e-a90e-8d4a40e8caeb" />
 <img width="900" height="62" alt="image" src="https://github.com/user-attachments/assets/5a0355bc-1055-4374-9e54-07b952e5b574" />
<img width="900" height="636" alt="image" src="https://github.com/user-attachments/assets/15858b68-bc5d-44aa-9462-74ae49093157" />
<img width="900" height="124" alt="image" src="https://github.com/user-attachments/assets/cd3ebaba-8ad7-47ef-a924-e72de2bcc28b" />



 

 
 


---

### Phase 1 Conclusion

GPG encryption and digital signatures successfully protected PaySure's internal communications — ensuring messages remain confidential, unaltered, and verifiably sent by trusted users.

---

## Phase 2: Password Security Audit

### Objective
Evaluate PaySure's password storage practices by demonstrating the risks of weak hashing algorithms and the effectiveness of stronger hashing with salting.

### Tools Used
- Kali Linux
- Hashcat
- OpenSSL
- Linux hashing utilities

---

### Step 1: Password Hash Creation

Test password: `password123`

```bash
# MD5
echo -n password123 | md5sum

# SHA1
echo -n password123 | sha1sum

# SHA-512
echo -n password123 | sha512sum
```
<img width="900" height="265" alt="image" src="https://github.com/user-attachments/assets/2cd9b20e-1738-49c9-9ecc-f2fd633ebabd" />
<img width="900" height="82" alt="image" src="https://github.com/user-attachments/assets/5b1a10c2-4ff1-42f8-9045-55b38d925772" />
<img width="900" height="66" alt="image" src="https://github.com/user-attachments/assets/979df17b-f8ce-4d31-ad6e-81ecce46902e" />
<img width="900" height="120" alt="image" src="https://github.com/user-attachments/assets/aa61f686-ea77-4da5-9c43-8322c00465d9" />

---

### Step 2: Hash Cracking Using Hashcat

Weak hashes were stored in a file and attacked using Hashcat with the RockYou wordlist.

```bash
# MD5 cracking
hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt

# SHA1 cracking
hashcat -m 100 hashes.txt /usr/share/wordlists/rockyou.txt
```

**Results:**
- MD5 hash cracked within **seconds**
- SHA1 hash cracked within **minutes**
<img width="900" height="479" alt="image" src="https://github.com/user-attachments/assets/95c7efdd-c24e-47b6-8992-d1c3dce99ce3" />
<img width="900" height="350" alt="image" src="https://github.com/user-attachments/assets/ba5f8dcc-2410-4b18-aa4a-f010a0d01a30" />
<img width="900" height="585" alt="image" src="https://github.com/user-attachments/assets/fba7c724-6531-4f75-80a1-e630285b7429" />
<img width="900" height="506" alt="image" src="https://github.com/user-attachments/assets/50c3b225-89da-454d-b6cf-fe04ffd385e9" />
<img width="900" height="361" alt="image" src="https://github.com/user-attachments/assets/2849948d-e4a0-4cb1-ba7c-d346460de535" />

---

### Step 3: Salting Demonstration (Extra Task)

A salt was added to the password before hashing using SHA-512.

```bash
openssl passwd -6 -salt S@LT! password123
```

The salted SHA-512 hash was loaded into Hashcat. Despite using a strong algorithm with salt, the password was cracked quickly due to its presence in the RockYou wordlist — demonstrating that **salting alone cannot protect weak passwords**.
<img width="900" height="621" alt="image" src="https://github.com/user-attachments/assets/cd802778-92ed-4bc2-a408-49af6ff35237" />
<img width="900" height="671" alt="image" src="https://github.com/user-attachments/assets/6a03e241-d4f5-40a3-83bd-3e9366325300" />

**Further Analysis:**

A stronger password (`Th!sIsAVeryStr0ngP@ssw0rd!2025`) was tested. Hashcat **failed** to recover it, confirming that salting must be paired with strong password policies.
<img width="900" height="171" alt="image" src="https://github.com/user-attachments/assets/99bf231c-b116-42fa-9692-98251d8596dd" />
<img width="900" height="498" alt="image" src="https://github.com/user-attachments/assets/3e675925-e281-4e66-95a5-e385a7a195f4" />

---

### Results Summary

| Hash Type | Salt Used | Cracked | Time Taken | Risk Level |
|-----------|-----------|---------|------------|------------|
| MD5       | No        | Yes     | Seconds    | Critical   |
| SHA1      | No        | Yes     | Minutes    | High       |
| SHA-512   | No        | Difficult | Long     | Medium     |
| SHA-512   | Yes       | No      | Failed     | Secure     |

---

### Phase 2 Conclusion

Weak hashing algorithms (MD5, SHA1) pose a critical risk to credential security. PaySure must migrate to modern salted hashing algorithms such as **bcrypt**, **Argon2**, or **PBKDF2**, combined with a strong password policy to effectively resist offline cracking attacks.

---

## Phase 3: Phishing, Base64 & Punycode Attack Simulation

### Objective
Demonstrate how attackers use encoding techniques and deceptive domain names to carry out phishing and social engineering attacks against PaySure employees.

### Tools Used
- Kali Linux
- Base64 utility
- idn2 (Internationalized Domain Names tool)

---

### Part 1: Base64 Encoded Phishing Message

Attackers often encode phishing emails using Base64 to bypass basic email content inspection systems.

**Decoding the suspicious email:**
```bash
base64 -d encoded_email.txt
<img width="900" height="175" alt="image" src="https://github.com/user-attachments/assets/0200aa2c-f728-4b25-a1e2-b906b9c734f8" />
<img width="900" height="383" alt="image" src="https://github.com/user-attachments/assets/db6650e7-c766-4ee7-92cc-defc4e6fe7a9" />


```

**Decoded Message Analysis:**
The email impersonated the HR department and requested employees to update sensitive personal and payroll information, using an urgent tone to pressure recipients.

---

### Part 2: Punycode & Homograph Attack Analysis

**Phishing domain detected:** `hr@xn--paysre-sya.com`

```bash
idn2 xn--paysre-sya.com
<img width="900" height="72" alt="image" src="https://github.com/user-attachments/assets/10fe0102-7ab2-43ac-a89d-cbefbffa17ff" />
<img width="900" height="67" alt="image" src="https://github.com/user-attachments/assets/672a99b2-4fd5-4c04-98d2-4dbfef91aec7" />


```

**How a Homograph Attack Works:**
A homograph attack exploits Unicode characters to create domain names that visually appear identical or very similar to legitimate ones — tricking users into trusting malicious emails and websites.

| Comparison Type | Real Domain (paysure.com) | Fake Domain (paysúre.com) | Matches | Explanation |
|----------------|--------------------------|--------------------------|---------|-------------|
| Visual | paysure.com | paysúre.com | No | Accent on "u" looks similar but isn't |
| Byte Length | 11 bytes | 12 bytes | No | Extra byte for ú |
| Hex — "u" | `75` | `c3 ba` | No | Different UTF-8 encoding |
| Punycode | N/A (ASCII) | xn--paysre-sya.com | N/A | Fake uses punycode |
| Risk | Legitimate | Phishing spoof | N/A | Fake can host malicious content |
<img width="900" height="212" alt="image" src="https://github.com/user-attachments/assets/17a98afd-6556-46d0-811f-cfc220d801c6" />
<img width="900" height="90" alt="image" src="https://github.com/user-attachments/assets/348974ea-a115-4809-8326-549710df0786" />
<img width="900" height="75" alt="image" src="https://github.com/user-attachments/assets/9db2b10c-9602-4610-babf-c170e1928038" />
<img width="900" height="77" alt="image" src="https://github.com/user-attachments/assets/b554beb4-ba3c-4c09-a4fe-0fb8dfd23e3c" />


---

### Part 3: Link and Email Analysis

**Red Flags Identified:**
- Urgent request for sensitive data
- Look-alike domain impersonating HR
- Request to submit personal and payroll details
- Links pointing to `mailto:hr@xn--paysre-sya.com`

---

### Extra Task: Employee Awareness Guide

**How to Spot Punycode and Phishing Attacks:**

1. **Look for "xn--"** — If a domain contains `xn--`, it is punycode-encoded. Real PaySure domains use plain ASCII: `paysure.com`.
2. **Inspect letters closely** — Fake domains may use accented characters (ú vs u) or lookalike letters from other scripts.
3. **Verify manually** — Type the domain directly into your browser; never click links in suspicious emails.
4. **Use browser extensions** — Tools like "Punycode Alert" or "IDN Homograph Attack Detector" provide real-time warnings.
5. **Report immediately** — Forward suspicious emails to IT without clicking any links.

---

### Phase 3 Conclusion

Phishing attacks rely on deception rather than technical exploits. Base64 encoding and punycode homograph attacks bypass basic defenses by exploiting human trust. Employee awareness training and robust email gateway controls are essential defensive layers.

---

## Phase 4: Security Report & Recommendations

### Executive Summary (Non-Technical)

PaySure faced targeted phishing attacks, weak password protection, and insecure communication channels. The engagement implemented encrypted messaging, audited password security, and simulated real attacker techniques using punycode phishing and encoding analysis — demonstrating both the vulnerabilities and their remediation.

---

### Risk Summary

| Risk | Severity | Description |
|------|----------|-------------|
| Weak password hashing | **Critical** | MD5 hashes leaked = instant cracking |
| Plaintext messaging | **Critical** | Interception possible without encryption |
| Punycode phishing domain | **High** | Employees may submit credentials to fake site |
| No digital signatures | **High** | Messages can be forged without verification |
| No employee awareness | **High** | Human error increases overall exposure |

---

### Recommendations

**1. Enforce Secure Password Policies**
- Migrate all passwords to SHA-512 with unique salts (or preferably bcrypt / Argon2)
- Enforce minimum 12-character password complexity requirements
- Deploy MFA company-wide

**2. Encrypt Internal Communication**
- Deploy GPG-based secure messaging, or
- Implement S/MIME certificates company-wide

**3. Block Punycode Domains**
- Configure email gateway rules to block inbound domains starting with `xn--`
- Enable domain impersonation protection

**4. Continuous Security Awareness Training**
- Monthly phishing simulations
- Short educational content explaining punycode and social engineering tactics

**5. Dark Web Monitoring**
- Implement alerting when PaySure credentials appear in breach databases or dark web forums
