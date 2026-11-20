# Network Incident Investigation: DNS Resolution Failure (UDP/ICMP Analysis)

**Analyst:** Nake Nsani mesi Pius  
**Tools & Protocols:** `tcpdump` Packet Logs, DNS (UDP Port 53), ICMP, Wireshark / Network Troubleshooting  
**Incident Summary:** Diagnosed an internal web access outage caused by blocked/unreachable UDP Port 53 traffic between a client workstation and the DNS server.

---

## 1. Executive Summary
At **1:24 PM**, users reported an inability to reach `yummyrecipesforme.com`. By inspecting `tcpdump` network packet logs across DNS and ICMP protocols, I isolated the root cause to repeated `ICMP Destination Unreachable (UDP Port 53 Unreachable)` errors returned by the DNS server—preventing domain name resolution.

## 2. Technical Investigation & Key Findings
* **Incident Timestamp:** 1:24 PM (first error response logged)
* **Client Workstation IP:** `192.51.100.15`
* **Target DNS Server IP:** `203.0.113.2`
* **Target Domain:** `yummyrecipesforme.com`
* **Protocols & Ports Observed:** DNS over **UDP Port 53**; error responses via **ICMP**
* **Packet Behavior:**
  * The client workstation (`192.51.100.15`) repeatedly sent standard DNS A-record queries over UDP to `203.0.113.2:53`.
  * Instead of returning an IP address resolution, the server/network returned `ICMP: UDP Port 53 Unreachable` errors for every query.

## 3. Root Cause Analysis
* **Primary Cause:** UDP Port 53 on the DNS server (`203.0.113.2`) was blocked, misconfigured via firewall rules, or temporarily overwhelmed (potential DoS/DDoS traffic or server resource exhaustion).

## 4. Recommended Remediation
1. **Verify DNS Service & Firewall Rules:** Confirm the DNS daemon is actively listening on `203.0.113.2:53` and verify no firewall ACLs are blocking inbound/outbound UDP Port 53 traffic.
2. **Configure Redundant DNS:** Implement a secondary/failover DNS resolver on client endpoints to prevent single-point-of-failure outages.
3. **Monitor Traffic Volume:** Review server load and rate-limit abnormal UDP Port 53 request spikes to mitigate DNS amplification or DoS conditions.