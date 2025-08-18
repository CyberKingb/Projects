# Evolving APT Simulation & Detection Lab
## *Objective*
The objective of this project is to simulate an APT group's evolving TTPs across multiple campaigns over time. Build and refine resilient detection rules that go beyond static signatures, accounting for behavioral changes in malware, C2, and persistence.
## Intelligence Gathering & Planning
- For this project Im going to be using working on APT29 (Cozy Bear)

Cozy Bear (APT29) is threat group that has been attributed to Russia's Foreign Intelligence Service (SVR). They have operated since at least 2008, often targeting government networks in Europe and NATO member countries, research institutes, and think tanks. APT29 reportedly compromised the Democratic National Committee starting in the summer of 2015.

In April 2021, the US and UK governments attributed the SolarWinds Compromise to the SVR; public statements included citations to APT29, Cozy Bear, and The Dukes. Industry reporting also referred to the actors involved in this campaign as UNC2452, NOBELIUM, StellarParticle, Dark Halo, and SolarStorm.

## TTP Evolution Map for APT29
### 2014–2016: Classic spear-phishing & custom backdoors

- Initial access: Spear-phishing with links/attachments to drop custom malware; heavy use of legit services for staging. Targets: governments, NATO-linked orgs, think tanks.
  

- Tooling / malware: MiniDuke/CozyDuke/SeaDuke family; stealthy backdoors and loaders with good OPSEC. 
  

- Persistence & C2: Registry run keys, scheduled tasks, HTTPS C2, domain fronting; strong anti-forensics. 


- What’s different here: Traditional endpoint-centric tradecraft; little cloud identity abuse yet.

### 2017–2019: Tradecraft hardening; staging through legit services

- Initial access: Phishing + living-off-the-land (LOTL) continues; more use of popular web apps (paste sites, storage/CDNs) to blend in. 
MITRE ATT&CK

- TTP shift: Early signs of identity and federation targeting as orgs adopt cloud SSO; increased OPSEC and compartmentalization noted across “Dukes” lineage. 
MITRE ATT&CK

### 2020: Supply-chain leap — SolarWinds / SUNBURST

Initial access: Trojanized SolarWinds Orion updates (supply-chain). Follow-on intrusions selective and stealthy. 
Google Cloud
CISA

Tooling / malware: SUNBURST backdoor + TEARDROP/RAINDROP second-stage, custom build-pipeline tampering (SUNSPOT). 
Google Cloud

Post-compromise: Careful credential theft, SAML token abuse, wide use of LOTL techniques. 
CISA

What changed: From endpoint phishing to strategic supply chain and identity-centric ops at scale.

2021–2022: Identity & federation persistence (AD FS), large-scale email/brand-impersonation

Initial access: Broad email campaigns masquerading as legitimate senders; credential theft and token abuse against cloud tenants. 
Microsoft

Persistence / privilege:

FoggyWeb (post-ex backdoor) to exfiltrate configuration, certificates, and tokens from AD FS, enabling long-term SSO abuse. 
Microsoft

MagicWeb: malicious AD FS DLL/claims-plugin to modify authentication and mint/accept tokens—powerful identity backdoor. 
BleepingComputer
BankInfoSecurity
Microsoft Download Center

What changed: Systematic federation & token manipulation for durable access, not just passwords.

2021–2023: Diplomatic phishing machine — multilayered lures & staging

Initial access: High-volume, high-quality lures against embassies/diplomatic targets; URL obfuscation (shorteners) and abuse of SaaS like Trello for staging. 
Google Cloud
+1

Tooling / malware: ROOTSAW/EnvyScout first-stage, BEATDROP & BOOMMIC loaders (2022). 
Google Cloud

What changed: Faster retooling and layered delivery chains to defeat filters and sandboxes. 
Google Cloud

2023: Beyond email — Teams/social-engineering & OAuth footprint

Initial access: Social-engineering over Microsoft Teams; token/consent tricks to land in Microsoft 365. 
Help Net Security

What changed: Expansion to chat platforms as phish vectors; more cloud-native initial access patterns.

2024: Cloud-first intrusion playbook — OAuth & consent abuse at scale

Initial access: Credential attacks (password spraying/credential stuffing) + malicious OAuth apps / consent grants to get initial cloud access; residential proxies to mask origin. 
NCSC

High-profile case: Microsoft corporate email breach (detected Jan 12, 2024): theft of mail and some source code; actor persists via OAuth apps/credentials and escalates targeting based on stolen intel. 
Microsoft Security Response Center
+1

Phishing ops: New spear-phishing waves (late 2024), consistent with OAuth- and identity-centric follow-through. 
HHS.gov

What changed: Mature cloud identity tradecraft (OAuth, consent, refresh tokens) as the default entry and persistence path. 
NCSC

2024–2025: Political parties & Europe focus; continued Teams/OAuth; relentless re-access

Targeting: European political parties (e.g., Germany’s CDU) with themed lures; WINELOADER backdoor delivered via ROOTSAW chain. 
Google Cloud

Ongoing pressure: Post-breach attempts to re-enter Microsoft and others using stolen data and token/consent techniques; activity volume spikes reported. 
Microsoft Security Response Center
Wall Street Journal

What changed: Blending political-party targeting into the long-running diplomatic set; persistence in re-exploitation using data from prior intrusions. 
Google Cloud

Cross-cutting TTP themes (then → now)

Initial access: Spear-phishing w/ attachments & links → Supply-chain (SolarWinds) → Chat/SaaS phishing (Teams) → OAuth/consent & token abuse as primary cloud door. 
Google Cloud
Help Net Security
NCSC

Persistence: File/registry/run keys → Golden SAML & federation tampering → AD FS backdoors (FoggyWeb/MagicWeb) → Malicious OAuth apps / service principals / refresh tokens. 
Microsoft
BleepingComputer
NCSC

Defense evasion: LOTL (PowerShell/WMI), staging via legit SaaS (Trello/shorteners), residential proxies, careful OPSEC and compartmentalized C2. 
Google Cloud
+1
NCSC

Objectives: Strategic intel on governments/diplomacy → broad IT/service providers → cloud identity & email for long-tail collection and lateral targeting. 
MITRE ATT&CK
Google Cloud

Quick ATT&CK-aligned mapping (selected)

Initial Access: Phishing: links/attachments; Supply-Chain Compromise; Valid Accounts (cloud); External Remote Services; Drive-by over SaaS/Teams. 
MITRE ATT&CK
Google Cloud
Help Net Security

Execution: Malicious Office macros, script interpreters (PowerShell), DLL search-order hijack. 
MITRE ATT&CK

Persistence: Scheduled tasks, registry, AD FS plugin/DLL (MagicWeb), OAuth app/service-principal persistence, token theft/refresh tokens. 
BleepingComputer
NCSC

Privilege Escalation/Credential Access: Credential dumping, SAML token/signing cert theft (FoggyWeb), password spraying/credential stuffing. 
Microsoft
NCSC

Defense Evasion: LOTL (WMI/PowerShell), staging via Trello/shorteners, signed binaries/proxied execution. 
Google Cloud
+1

C2 & Exfil: HTTPS over common platforms; cloud mail/API access; selective, low-and-slow exfil from email and file stores. 
Microsoft Security Response Center

Practical detection & hardening by era (what to hunt now)

Identity & OAuth (current priority):

Hunt for illicit OAuth apps/consent; anomalous service principals; tokens minted outside normal IPs; unexpected Graph scopes.

Enforce admin consent policies, conditional access on OAuth apps, step-up auth for risky token minting. 
NCSC

Federation/AD FS:

Verify AD FS plugin integrity; monitor for non-Microsoft DLLs and modified claims providers; protect signing keys; rotate certs after suspected compromise. 
BleepingComputer

Email & chat vectors:

Block external Teams chats (or restrict) for sensitive tenants; banner and detonate links from URL shorteners; enforce attachment detonation for diplomatic-style lures. 
Help Net Security
Google Cloud

LOTL & SaaS staging:

Alert on PowerShell with network calls, WMI process creation, and traffic to unusual SaaS (e.g., Trello) used as staging. 
Google Cloud

Supply chain lessons:

Monitor build systems and code-signing; egress allow-listing for update channels; provenance checks for packages/updates. 
Google Cloud
