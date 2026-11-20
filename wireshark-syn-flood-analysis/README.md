# Network Attack Investigation: TCP SYN Flood DoS Analysis

**Analyst:** Nake Nsanimesi Pius  
**Tools & Protocols:** Wireshark Packet Logs, TCP/IP (Three-Way Handshake), HTTPS (Port 443), HTTP  
**Incident Summary:** Analyzed TCP/HTTP network traffic logs to identify a direct Denial of Service (DoS) SYN flood attack causing connection resets and gateway timeouts for internal web users.

---

## 1. Executive Summary
Legitimate internal employees (`198.51.100.0/24`) experienced `HTTP/1.1 504 Gateway Time-out` errors and dropped connections while attempting to access the company web server (`192.0.2.1` on Port `443`). Packet-level log analysis revealed a single-source **TCP SYN Flood DoS attack** originating from IP `203.0.113.0` that exhausted the server's connection queue.

## 2. Normal Baseline vs. Attack Behavior
* **Normal Traffic Baseline (Packets #47–51):**
  * Legitimate clients (e.g., `198.51.100.23`) completed a standard TCP three-way handshake (`[SYN]` → `[SYN, ACK]` → `[ACK]`) within milliseconds before issuing `HTTP GET /sales.html` requests and receiving `200 OK` responses.
* **Malicious Traffic Anomaly (Packet #52 onward):**
  * External IP `203.0.113.0` began sending rapid, consecutive TCP `[SYN]` packets to destination `192.0.2.1:443` from source port `54770` (`Len=0`).
  * By sending multiple `SYN` requests per second without maintaining normal application sessions, the attacker saturated the server's half-open connection table.

## 3. Service Impact & Log Evidence
* **Packets #73 & #80:** Legitimate employee sessions (`198.51.100.16` and `198.51.100.7`) were abruptly terminated with `[RST, ACK]` packets.
* **Packet #77:** Client `198.51.100.5` received an `HTTP/1.1 504 Gateway Time-out` error after the web server failed to respond within the gateway timeout threshold.
* **Packet #125+:** Complete denial of service achieved—the web server stopped responding to legitimate employee traffic entirely, logging only incoming attack traffic from `203.0.113.0`.

## 4. Recommended Mitigation & Defense
1. **Perimeter IP Block:** Immediately block source IP `203.0.113.0` at the perimeter firewall and upstream routers.
2. **Enable SYN Cookies & Rate Limiting:** Configure TCP SYN cookies on the web server/load balancer and implement connection rate-limiting per source IP on Port `443`.
3. **Deploy IDS/IPS & DDoS Protection:** Implement an Intrusion Prevention System (IPS) or cloud WAF/DDoS mitigation service to automatically detect and drop abnormal `SYN` flood thresholds.