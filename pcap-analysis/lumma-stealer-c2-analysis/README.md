    Lumma Stealer C2 Activity Analysis
PCAP-Based Incident Investigation
  Overview

This project documents a blind network forensic investigation of a PCAP containing Lumma Stealer activity.

Starting from a single IDS alert, the goal was to reconstruct the full scope of compromise — identifying the infected host, attributing activity to a user, and confirming command-and-control (C2) behavior — without walkthrough guidance or prior knowledge of the dataset.

  Initial Trigger

An IDS alert flagged:

ET MALWARE Lumma Stealer Victim Fingerprinting Activity

Suspicious outbound traffic to: 153.92.1.49 (TCP/80)

This IOC served as the entry point for the investigation.

  Investigation Summary
Infected Host Identification
IP Address: 10.1.21.58
MAC Address: 00:21:5d:c8:0e:f2
Host & User Attribution
Hostname: DESKTOP-ES9F3ML
User Account: gwyatt
Full Name: Gabriel Wyatt

Attribution was achieved through correlation across Kerberos, DNS, and SAMR traffic.

  C2 Communication Behavior

Two-stage HTTP interaction identified:

  Stage 1 — Tasking

GET /api/set_agent
→ Retrieval of attacker-controlled logic (fingerprinting script)

  Stage 2 — Exfiltration

POST /api/set_agent?...&act=log
→ Structured data exfiltration (~8KB payload)
Data Exfiltrated
GPU / WebGL renderer
Screen resolution
Installed fonts
Hardware concurrency
Canvas fingerprint (base64 image)

Indicates active host fingerprinting, not simple beaconing.

  C2 Infrastructure
IP: 153.92.1.49
Domain: whitepepper.su

  Attack Narrative

The infected host initiated outbound HTTP communication with a remote C2 server, first retrieving instructions and then transmitting detailed system fingerprinting data.

Analysis of authentication traffic confirmed the activity occurred within a legitimate user session, indicating execution in user context.

This behavior aligns with Lumma Stealer’s objective of profiling infected systems for tracking, evasion, and potential monetization.

  Key Takeaways
IOC pivoting can quickly expose the full attack chain
Correlating multiple protocols enables full host attribution
Kerberos traffic is critical for identifying active users
Structured HTTP POST requests are strong indicators of exfiltration
Some malware prioritizes profiling over persistence

  Skills Demonstrated
Network traffic analysis (Wireshark)
IOC-based threat hunting
Active Directory protocol analysis
C2 traffic identification
Data exfiltration detection
Incident reconstruction
