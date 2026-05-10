# Snort — Investigation Walkthrough

**Analyst:** Andy Dela Quarshie Wright  
**Tool:** Snort 2.9.7.0  
**Environment:** TryHackMe Ubuntu Lab  

---

## Environment Setup

- **Host:** ubuntu@ip-10-49-188-87
- **Snort version:** 2.9.7.0 GRE (Build 149)
- **Interface:** eth0 (passive), eth0:eth1 (inline IPS)
- **DAQ:** pcap (passive), afpacket (inline)

---

## Step 1 — Run Snort in Sniffer Mode

**Command:**
```bash
sudo snort -dev -l .
```

Snort initialised successfully — acquiring traffic from eth0. Output confirmed:
- Initialization Complete
- Running in packet logging mode
- pcap DAQ configured to passive
- Acquiring network traffic from eth0

---

## Step 2 — Analyse Captured Packets

Snort captured bidirectional TCP traffic between 10.10.140.29:22 and 10.10.245.36.

**Key observations:**
- Port 22 — active SSH session in progress
- TCP flags AP (ACK + PSH) — data transfer confirmed
- Packet sizes vary — 1108 bytes (data), 100 bytes (ACK), 52 bytes (ACK)
- Sequence numbers consistent with an established session

**Packet analysis:**
```
10.10.140.29:22 -> 10.10.245.36:46660  DgmLen:1108  (large data packet — SSH response)
10.10.245.36:46660 -> 10.10.140.29:22  DgmLen:100   (ACK from client)
10.10.140.29:22 -> 10.10.245.36:46660  DgmLen:52    (final ACK)
```

---

## Step 3 — Write Custom Rule

Created local.rules file and wrote a drop rule to block SSH traffic:

```bash
touch local.rules
nano local.rules
```

Rule:
```
drop tcp any 22 <> any any (msg:"block access"; sid: 100001; rev:1;)
```

This rule drops all TCP traffic on port 22 in both directions — effectively blocking SSH on the network segment.

---

## Step 4 — Deploy Rule in IPS Mode

Deployed the rule using afpacket DAQ in inline mode — bridging eth0 and eth1:

```bash
sudo snort -c local.rules -q -Q --daq afpacket -i eth0:eth1 -A full
```

In inline mode Snort sits between two network interfaces and actively drops packets matching the drop rule — functioning as an IPS rather than just a passive IDS.

---

## Key Learnings

- Snort has three primary modes: sniffer, packet logger, and NIDS/NIPS
- Passive mode (pcap DAQ) detects but does not block — IDS
- Inline mode (afpacket DAQ) actively blocks matched traffic — IPS
- Custom rules allow analysts to target specific threats or block specific services
- The sid must be unique per rule — local rules typically start at 1000001
- The rev field tracks rule versions — increment when modifying a rule
