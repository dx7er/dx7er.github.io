# CARTP Exam Review: Passing the Certified Azure Red Team Professional

I passed the Certified Azure Red Team Professional (CARTP) exam from Altered Security in October 2026. This post covers the course, the 24-hour exam and the report, plus what I would do differently. There are no spoilers: no targets, no attack paths, no flags.

A bit about me: I am an MSc Cyber Security & Forensics student at the University of Westminster and work in security alongside my studies. I took CARTP because most UK organisations now run on Microsoft 365 and Entra ID, and I wanted to understand how attackers really move through Azure, not just how the controls are meant to work.

You can verify my credential [here](https://www.credential.net/357ad961-3a1b-4098-9648-abd3c95c0323).

## What CARTP is

CARTP is a hands-on Azure red team certification. You get a bootcamp, a practice lab and then a practical exam where you attack a live Azure and Entra ID environment.

| Part | What it involves |
| --- | --- |
| Bootcamp | Live classes covering 26 learning objectives (LOs) |
| Practice lab | Hands-on Azure tenant with flags for each LO |
| Exam | 24 hours of hands-on attacking, starting with very little information |
| Report | A full write-up of your attack path, submitted after the exam |

The course covers the full attack chain: initial access, enumeration, privilege escalation, lateral movement across tenants and resources, and persistence. It also touches on hybrid identity, where on-prem Active Directory and Entra ID meet.

[Add: course price and how long your lab access lasted]

## The course and lab

I finished the practice lab on 28 June 2026 with all 49 flags. Doing every flag, not just the ones I found interesting, was the single best preparation for the exam.

What I liked:

- **It teaches the attacker's mindset.** You learn to ask "what can this identity reach?" at every step, rather than following a checklist.
- **Real tools.** You use AADInternals, ROADtools, AzureHound, MicroBurst, Az PowerShell, Azure CLI and Microsoft Graph, the same tools used in real engagements.
- **Tokens everywhere.** Much of Azure attacking is about getting, reading and reusing tokens. The course makes this click.
- **It maps well to defence.** Each LO taught me something I could later turn into a detection. I have since been turning LOs into purple-team labs on my blog.

What could be better:

- [Add: anything you found weak, e.g. pace of the bootcamp, outdated commands, lab stability]

I kept notes per LO, plus cheat sheets for Az PowerShell, Azure CLI, the Microsoft Graph module and token handling. Those cheat sheets saved me hours in the exam.

## Exam day

I started the exam on 3 October 2026 and reached the final flag the same day. [Add: roughly how many hours it took you]

**Setting up.** The exam machine is a clean Windows VM with internet access but no tools installed. My first hour went on building a toolkit: Az PowerShell, Microsoft.Graph, AADInternals, Azure CLI, Python, Git, ROADtools, AzureHound and MicroBurst. Two small fixes helped: enabling .NET Framework 3.5 so older PowerShell tooling ran properly, and turning off IE Enhanced Security so downloads worked. Have your install list ready before you start.

**Starting point.** You begin with very little: one entry point and no credentials. Everything after that comes from your own enumeration.

**My approach.**

1. Recon the entry point slowly and properly before attacking it.
2. After every foothold, stop and enumerate what the new identity can access.
3. Log every command, output and credential as you go, in sections: findings, timeline, identities, resources.
4. When stuck, go back to enumeration. In my experience the answer was almost always something I had already seen but not followed up.

[Add: the moment that felt hardest, described without spoilers, e.g. "crossing into the second part of the environment took me longest"]

## The report

My biggest mistake: I took no screenshots on my first run. I was so focused on moving forward that I reached the final flag with almost no evidence. I had to go through the whole attack chain again to capture everything.

The good news is the second run was much faster, because I knew the path. It also helped me understand each step properly, which made the report better.

My final report was 19 pages, written phase by phase: each step of the attack chain with the commands, output, screenshots and a short explanation of why it worked.

One rule to know: Altered Security allows AI tools in the lab, but rejects reports with AI-written content. The report must be in your own words. Plan time for writing, and do not leave it to the last few hours.

I submitted on time and got my pass on 7 October 2026.

## Tips for future candidates

1. **Finish every lab flag.** The exam assumes you know all the LOs, not just your favourites.
2. **Build your own cheat sheets.** Writing them yourself is half the learning, and you will search them constantly.
3. **Prepare a toolkit install script.** The exam VM starts empty. Save your first hour.
4. **Screenshot as you go.** Every foothold, every token, every new access. Do not repeat my mistake.
5. **Understand tokens deeply.** Know which token works for which resource, and how to decode one to see what it allows.
6. **Enumerate after every step.** New identity means new permissions. Check them before moving on.
7. **Rest.** 24 hours is long. A short break often shows you what you missed.
8. **Write the report in your own words.** It is a rule, and it is also how you prove you understood the attack.

## Verdict

CARTP is worth it if you work with Azure or Microsoft 365 from either side. It made me a better defender as much as an attacker.

| Good for | Less suited to |
| --- | --- |
| Pentesters moving into cloud | Complete beginners with no Azure or AD basics |
| Blue teamers who want to know what attacks look like | People looking only for theory or multiple-choice exams |
| Cloud security engineers and incident responders | |

**Rating:** [Add: your score out of 10]

**What's next for me:** I am turning CARTP learning objectives into purple-team labs, each one showing the attack, the Sentinel detections and the hardening controls. The first, on OAuth illicit consent grants, is already [on my GitHub](https://github.com/dx7er/AzureConsentLab), with more in my Defender Pack series.