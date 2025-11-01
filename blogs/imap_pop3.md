# IMAP / POP3 Enumeration Writeup - HTB Academy

**Target:** `10.129.242.62` (FQDN: `dev.inlanefreight.htb`)
**Scope:** HTB Academy Lab (Authorized environment)
**Author:** dx73r

---

## Executive Summary

During an authorized HTB Academy lab, enumeration was performed against the mail services running on the target host. Both IMAP and POP3 protocols were identified as active, with SSL/TLS support enabled. The objective was to gather service information, authenticate using valid credentials, and retrieve hidden flags contained within email messages.

**Key Findings:**

* IMAP/POP3 services hosted via **Dovecot**.
* SSL certificate revealed **Organization:** `InlaneFreight Ltd` and **CN:** `dev.inlanefreight.htb`.
* Custom POP3 banner discovered: **`InFreight POP3 v9.188`**.
* Valid user credentials: `robin:robin`.
* Two flags were successfully retrieved from IMAP service interactions.
* Admin email discovered: `devadmin@inlanefreight.htb`.

**Discovered Flags:**

* IMAP Enumeration Flag: `HTB{roncfbw7iszerd7shni7jr2343zhrj}`
* IMAP Mailbox Flag: `HTB{983uzn8jmfgpd8jmof8c34n7zio}`

---

## Tools Used

* **nmap** – for service discovery
* **openssl** – for certificate inspection and interactive IMAP/POP3 access
* **curl** – for quick IMAP/POP3 authentication testing
* 
---

## Methodology

### 1. Service Discovery

```bash
nmap -sV -sC -p110,143,993,995 10.129.242.62 -T4
```

**Results:**

* `110/tcp`  → POP3 (Dovecot)
* `143/tcp`  → IMAP (Dovecot)
* `993/tcp`  → IMAPS (Dovecot)
* `995/tcp`  → POP3S (Dovecot)
* SSL Certificate CN: `dev.inlanefreight.htb`
* Organization: `InlaneFreight Ltd`

### 2. Certificate Inspection

```bash
echo | openssl s_client -connect 10.129.242.62:993 2>/dev/null | openssl x509 -noout -subject -issuer -dates -text
```

**Key Fields:**

```
Subject: CN=dev.inlanefreight.htb, O=InlaneFreight Ltd, ST=London, C=UK
Not Before: 2021-11-08
Not After: 2295-08-23
```

### 3. Authentication Testing via Curl

```bash
curl -k imaps://10.129.242.62 --user "robin:robin"
```

**Result:** Successful login and mailbox listing.

```
* LIST (\Noselect \HasChildren) "." DEV
* LIST (\HasNoChildren) "." INBOX
```

### 4. Interactive IMAP Enumeration

```bash
openssl s_client -connect 10.129.242.62:imaps
```

Once connected:

```
A001 LOGIN robin robin
A002 LIST "" "*"
A003 SELECT DEV.DEPARTMENT.INT
A004 FETCH 1 RFC822
A005 LOGOUT
```

**Discovered Message:**

```
Subject: Flag
From: CTO <devadmin@inlanefreight.htb>
To: Robin <robin@inlanefreight.htb>

HTB{983uzn8jmfgpd8jmof8c34n7zio}
```

### 5. POP3 Interaction Example

```bash
openssl s_client -connect 10.129.242.62:pop3
```

After connection:

```
USER robin
PASS robin
STAT
LIST
RETR 1
QUIT
```

---

## Confirmed Answers

| Question            | Answer                                                          |
| ------------------- | --------------------------------------------------------------- |
| Organization Name   | InlaneFreight Ltd                                               |
| FQDN                | dev.inlanefreight.htb                                           |
| IMAP Service Flag   | HTB{roncfbw7iszerd7shni7jr2343zhrj}                             |
| POP3 Custom Version | InFreight POP3 v9.188                                           |
| Admin Email         | [devadmin@inlanefreight.htb](mailto:devadmin@inlanefreight.htb) |
| Mailbox Flag        | HTB{983uzn8jmfgpd8jmof8c34n7zio}                                |

---

## Impact & Risk

**Severity:** Medium
If this configuration existed in production, weak credentials and verbose banners could leak sensitive information. Exposure of internal admin email addresses increases the risk of phishing and credential reuse attacks.

---

## Remediation Recommendations

* Enforce **strong password policies** and avoid using default credentials.
* Disable plaintext authentication methods like `AUTH PLAIN` without TLS.
* Replace **self-signed certificates** with valid CA-signed certificates.
* Suppress version and banner details in mail server configuration.
* Implement **account lockout** for repeated failed login attempts.
* Regularly monitor mail logs for suspicious access attempts.

---

## Conclusion

The lab demonstrated the enumeration of IMAP and POP3 services and the retrieval of sensitive mailbox contents through authenticated access. Proper configuration, secure credential policies, and TLS enforcement are critical in protecting mail servers from unauthorized enumeration and data exposure.

---

##### A supporter is worth a thousand followers! [Buy Me a Coffee](https://www.buymeacoffee.com/dx73r). If you like this blog, follow me on [GitHub](https://github.com/dx7er) and [LinkedIn](https://www.linkedin.com/in/naqvio7/). 
