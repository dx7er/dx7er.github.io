# SNMP Enumeration Writeup – HTB Academy

## Overview

This task involves enumerating an SNMP service running on the target `10.129.164.238` using various tools to extract system information, discover community strings, and identify hidden data. The following documentation outlines the methodology, tools, commands, and final answers.

---

## Step 1: Initial Nmap Scan

**Command:**

```bash
nmap -sV -T4 -Pn -p161,162 10.129.161.129
```

**Purpose:**
To detect open SNMP-related ports (161 and 162) and check for service versions.

**Result:**

```
PORT    STATE    SERVICE     VERSION
161/tcp filtered snmp
162/tcp filtered snmptrap
```

The target is up and SNMP ports are filtered — enumeration proceeds with community string testing.

---

## Step 2: Enumerate SNMP Service

**Command:**

```bash
snmpwalk -v2c -c public 10.129.164.238
```

**Purpose:**
To enumerate SNMP information using the default community string `public`.

**Output Highlights:**

```
Linux NIX02 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
devadmin <devadmin@inlanefreight.htb>
InFreight SNMP v0.91
```

**Findings:**

* **System:** Ubuntu 20.04 (NIX02)
* **Admin Email:** `devadmin@inlanefreight.htb`
* **Customized SNMP Version:** `InFreight SNMP v0.91`

![SNMPWalk Output](f02a6cb6-5db6-4236-9fb7-b572d57e8208.png)

---

## Step 3: Brute Force Community Strings

**Command:**

```bash
onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.164.238
```

**Purpose:**
To identify valid SNMP community strings using a wordlist.

**Result:**

```
10.129.164.238 [public] Linux NIX02 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
```

The community string `public` is confirmed valid.

![onesixtyone Scan](91d6e8fd-b2f8-47b0-ae6a-9393d87e7cbe.png)

---

## Step 4: Deep Enumeration with Braa

**Command:**

```bash
braa public@10.129.164.238:.1.3.6.*
```

**Purpose:**
To gather more detailed SNMP tree information and descriptions.

**Result:**

```
Linux NIX02 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
devadmin <devadmin@inlanefreight.htb>
InFreight SNMP v0.91
The SNMP Management Architecture MIB.
The MIB module for managing UDP implementations.
```

![Braa Output](9cace810-6e16-4c44-9ef6-71f63d05947b.png)

---

## Step 5: Exporting SNMP Data

**Command:**

```bash
snmpwalk -v2c -c public 10.129.164.238 > snmp-out.txt
```

**Purpose:**
To save the complete enumeration output for offline analysis.

![SNMP Output Save](36e8043e-016d-48db-b277-050f1b7af312.png)

---

## Step 6: Searching for Hidden Data / Flag

**Command:**

```bash
cat snmp-out.txt | grep "{"
```

**Result:**

```
HTB{5nMp_fl4g_uidhfljnsldiuhbfsdij44738b2u763g}
```

**Flag Found:** ✅ `HTB{5nMp_fl4g_uidhfljnsldiuhbfsdij44738b2u763g}`

![Flag Extraction](d33e7530-8706-48ce-a662-53057bb3e4b4.png)

---

## Step 7: Answers Summary

| Question                        | Answer                                            |
| ------------------------------- | ------------------------------------------------- |
| **Admin Email Address**         | `devadmin@inlanefreight.htb`                      |
| **Customized SNMP Version**     | `InFreight SNMP v0.91`                            |
| **Custom Script Output (Flag)** | `HTB{5nMp_fl4g_uidhfljnsldiuhbfsdij44738b2u763g}` |

![HTB Academy Submission](5619bd55-a166-4f76-b384-b87b46a97fb4.png)

---

## Conclusion

Through SNMP enumeration using tools such as **nmap**, **snmpwalk**, **onesixtyone**, and **braa**, valuable system information and credentials were obtained. This exercise demonstrates the importance of securing SNMP configurations and avoiding the use of default community strings such as `public`.

**Key Takeaways:**

* SNMP can leak sensitive data if not properly configured.
* Always restrict SNMP access to trusted hosts.
* Avoid default community strings and disable SNMP if not required.

---

##### A supporter is worth a thousand followers! [Buy Me a Coffee](https://www.buymeacoffee.com/dx73r). If you like this blog, follow me on [GitHub](https://github.com/dx7er) and [LinkedIn](https://www.linkedin.com/in/naqvio7/). 
