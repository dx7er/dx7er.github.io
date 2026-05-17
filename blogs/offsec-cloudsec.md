---
title: From OffSec to Cloud Security
date: 2026-04-19
tags: [cloud-security, azure, career, certifications, offensive-security]
---

## Introduction

The threat landscape has moved to the cloud and so has my career direction. My background is in offensive security: penetration testing, OSINT, malware analysis, and digital forensics. That is where I spent the last several years building my technical foundation, alongside a BSc in Cyber Security and now an MSc in Cybersecurity and Forensics at the University of Westminster.

Passing the **Microsoft Certified: Azure Fundamentals (AZ-900)** on March 19, 2026 was a deliberate first step. Not because it was the most difficult, but because I value strong foundations before moving into more advanced territory like AZ-104 and AZ-500.

This blog covers what I learned, how my offensive background mapped to Azure security concepts, and why fundamentals matter more than most people admit.

---

## 1. Why Start with AZ-900?

AZ-900 is entry-level. That is the point. Before touching Azure security tooling, you need to understand how Azure is actually structured core services, the shared responsibility model, IAM fundamentals, and governance.

Skipping this layer and jumping straight to AZ-500 is like running a web app pentest without understanding HTTP. You can follow a checklist, but you will miss things.

Key areas covered:

- Azure core services (Compute, Networking, Storage)
- Identity and Access Management (IAM)
- Azure governance and compliance
- Shared responsibility model
- Pricing and support structures

---

## 2. Mapping Offensive Knowledge to Azure Security

The most valuable part of studying AZ-900 was connecting what I already knew from offensive security to how Azure structures its defences.

| Offensive Concept | Azure Security Equivalent |
|---|---|
| IAM misconfigurations | Azure RBAC and privilege escalation paths |
| Lateral movement | Service principal abuse, managed identity pivoting |
| Privilege escalation | Over-permissioned roles, Owner/Contributor misuse |
| Persistence | Backdoor app registrations, guest account abuse |
| Credential dumping | Key Vault access policy weaknesses |

The attacker mindset is not separate from cloud security thinking it is exactly the right frame for understanding where Azure controls break down.

---

## 3. The Shared Responsibility Model

One of the most important concepts in cloud security is understanding what you are responsible for versus what the provider handles. Misunderstanding this boundary has caused real incidents at real organisations.

```
Infrastructure (Microsoft)
├── Physical datacentres
├── Network infrastructure
└── Hypervisor / host OS

Platform (Shared)
├── Operating system (IaaS = customer)
├── Network controls
└── Identity and access

Application (Customer)
├── Data classification
├── Account and access management
└── Application-level security
```

Moving from IaaS → PaaS → SaaS shifts more responsibility to Microsoft, but **identity and data are always your responsibility**.

---

## 4. Certification Roadmap

Each certification builds on the last. The goal is not to collect badges — it is to build a layered understanding before targeting a Junior Azure Cloud Security Analyst role.

| Certification | Status |
|---|---|
| AZ-900 — Azure Fundamentals | ✅ Completed — 19 March 2026 |
| CCSK — Certificate of Cloud Security Knowledge | 🔄 In Progress |
| AZ-104 — Azure Administrator | ⏳ Up Next |
| AZ-500 — Azure Security Technologies | ⏳ Up Next |

---

## Conclusion

If you are coming from a traditional security background and thinking about cloud security, do not skip the fundamentals because they feel too basic. Most breaches do not start with advanced techniques — they start with the basics being overlooked.

AZ-900 gave me the mental model I needed. The attacker mindset gives me the right lens to apply it. The next phase is building that out through AZ-104, CCSK, and AZ-500 and turning it into real portfolio work on the way.

The journey continues.

---

##### A supporter is worth a thousand followers! [Buy Me a Coffee](https://www.buymeacoffee.com/dx73r). If you like this blog, follow me on [GitHub](https://github.com/dx73r73r) and [LinkedIn](https://www.linkedin.com/in/dx73r/).
