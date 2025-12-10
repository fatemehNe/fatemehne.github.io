+++
title = "Incident Report: Simulated Ransomware incident response (ELCH/SiberX)"
date = "2023-01-09"
description = "Our winning IR submission: ransomware outbreak, lateral movement, and containment with actionable recommendations."
tags = ["Incident Response", "Ransomware", "IR Playbook", "Hackathon"]
thumbnail = "/images/IMG_4687.jpeg"
aliases = ["irforce-incident-response", "elch-siberx-incident"]
author = "Fatima Nezhadian"
+++

This post distills the investigative report my teammate (Nour Mousa) and I submitted for the ELCH/SiberX Hackathon Incident Response track.

## Scenario
- Ransomware encrypted file servers after a phishing-led compromise.
- Lateral movement observed; data exfiltration suspected.
- File access restored from recent backups; business impact mainly productivity loss.

## What we investigated
- **Initial access:** Active Directory vulnerability (e.g., CVE-2022-29623) leveraged for privilege escalation.
- **Lateral movement:** Abnormal Task Scheduler entries; new high-privilege user and downgraded admin accounts; suspicious outbound traffic to a Russian C2 IP (Table 1 in report).
- **Potential exfiltration:** Credential logs and network patterns raised flags; continued hunting in user and network activity.

## Actions we took
- Classified as **high risk**; executed the comms plan: IT helpdesk ↔ IT manager ↔ CISO/CEO with 4-hour/12-hour updates.
- Containment: blocked C2 via firewall rules; added YARA + SIEM alerts; patched AD flaws that enabled initial access and lateral movement.
- Recovery: restored file servers and workstations from backups.

## Recommendations (from the submission)
- Awareness training to reduce phishing risk.
- Enforce MFA/2FA, especially for privileged accounts.
- Routine vulnerability scanning and patching.
- Encrypt data at rest on servers to blunt exfiltration impact.
- Invest in lateral-movement and data-exfiltration prevention (e.g., Splunk, ExtraHop, Titanium).
- Apply Zero Trust principles across the network.

## Takeaways
- Treat ransomware with a structured comms cadence; keep executives and operators in sync.
- Validate AD hygiene early—privilege changes and scheduled tasks often tell the story.
- Mix detection (YARA/IDS/SIEM) with strong recovery (tested backups) and prevention (MFA, Zero Trust) for durable resilience.
