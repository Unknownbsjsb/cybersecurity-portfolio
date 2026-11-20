# cybersecurity-portfolio
# Internal Security Audit & Compliance Assessment: Botium Toys

**Analyst:** Nake Nsan mesi Pius  
**Frameworks & Standards:** NIST CSF (Identify), PCI DSS, GDPR, SOC 1 & SOC 2  
**Deliverables:** Risk Assessment Analysis, Controls Checklist, Compliance Gap Analysis, Stakeholder Recommendations



## 1. Executive Summary & Scope
Conducted a comprehensive internal security and compliance audit for **Botium Toys** to evaluate existing IT assets, administrative/technical/physical controls, and regulatory readiness. Based on missing technical safeguards and unclassified assets, the organization carried a **High Risk Score (8/10)**.

## 2. Key Audit Findings
* **Strengths Identified:**
  * Physical security controls (locks, CCTV surveillance, fire detection/prevention) are active across offices, storefront, and warehouse.
  * Perimeter firewall, regularly monitored antivirus software, and a 72-hour GDPR breach notification plan are established.
* **Critical Gaps Identified:**
  * **Access Control:** Absence of least privilege and separation of duties allows broad employee access to customer PII/SPII and cardholder data.
  * **Data Protection:** Cardholder data is accepted, processed, and stored locally without encryption.
  * **Detection & Resilience:** No Intrusion Detection System (IDS), automated data backups, or Disaster Recovery (DR) plan in place.
  * **Identity & Asset Management:** Nominal password rules without centralized enforcement, and unscheduled maintenance on legacy systems.

## 3. Compliance Gap Analysis
* **PCI DSS:** Non-compliant due to unencrypted cardholder data, unrestricted internal access, and weak password enforcement.
* **GDPR:** Partially compliant (72-hour breach notification and privacy procedures exist, but data classification/inventory and encryption gaps put E.U. customer data at risk).
* **SOC 1 / SOC 2:** Availability and integrity controls are present, but confidentiality and user access governance require remediation.

## 4. Prioritized Stakeholder Recommendations
1. **Encrypt Cardholder & Sensitive Data (Immediate):** Implement encryption at rest and in transit for all payment and PII/SPII databases to meet PCI DSS and GDPR requirements.
2. **Enforce Access Controls:** Implement Role-Based Access Control (RBAC), least privilege, and separation of duties across internal databases.
3. **Establish Backups & Disaster Recovery:** Deploy automated, encrypted off-site backups and formalize a tested Disaster Recovery Plan.
4. **Strengthen Authentication:** Enforce a minimum 8+ character complex password policy via a centralized password management system.
5. **Deploy Network Monitoring & Asset Inventory:** Implement an Intrusion Detection System (IDS), classify all IT assets under the **NIST CSF Identify** function, and establish a recurring maintenance schedule for legacy systems.