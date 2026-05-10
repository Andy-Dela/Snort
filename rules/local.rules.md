# Snort Rules — local.rules

All custom Snort rules written in this lab.

---

## Rules Written

### Rule 1 — Block SSH Traffic (Port 22)

```
drop tcp any 22 <> any any (msg:"block access"; sid: 100001; rev:1;)
```

**Purpose:** Drops all bidirectional TCP traffic on port 22 — blocks SSH connections  
**Action:** drop — packet is blocked and logged  
**Direction:** <> bidirectional — applies to both inbound and outbound SSH  

---

## Snort Rule Syntax Reference

```
[action] [protocol] [src_ip] [src_port] [direction] [dst_ip] [dst_port] ([options])
```

| Field | Options | Description |
|-------|---------|-------------|
| Action | alert, log, drop, reject, pass | What Snort does when rule matches |
| Protocol | tcp, udp, icmp, ip | Network protocol |
| Direction | -> (one way), <> (bidirectional) | Traffic direction |
| Options | msg, sid, rev, content, flags | Rule metadata and detection options |

---

## Common Rule Actions

| Action | Description |
|--------|-------------|
| alert | Generate alert and log packet |
| log | Log packet only — no alert |
| drop | Block packet and log (IPS mode only) |
| reject | Block and send TCP reset or ICMP unreachable |
| pass | Ignore the packet |

---

## Snort Commands Used

**Sniffer mode:**
```bash
sudo snort -dev -l .
```

**Packet logging mode:**
```bash
sudo snort -dev -l . -h 10.10.140.0/24
```

**IDS mode with rules:**
```bash
sudo snort -c local.rules -A full
```

**IPS inline mode:**
```bash
sudo snort -c local.rules -q -Q --daq afpacket -i eth0:eth1 -A full
```
