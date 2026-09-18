# Cisco 300-615 DCIT Exam: Troubleshooting Cisco Data Center Infrastructure

[![Cisco Certified](https://img.shields.io/badge/Cisco_Certified-CCNP_Data_Center_|_DCIT-049fd9?style=for-the-badge&logo=cisco&logoColor=white)](https://www.cisco.com/)
[![Track](https://img.shields.io/badge/Track-Data_Center_Troubleshooting-049fd9?style=for-the-badge&logo=cisco)](https://www.cisco.com/)
[![Level](https://img.shields.io/badge/Level-Professional_Concentration-1BA0D7?style=for-the-badge)](https://www.cisco.com/)
[![Duration](https://img.shields.io/badge/Duration-90_Minutes-orange?style=for-the-badge)](https://www.cisco.com/)
[![Score](https://img.shields.io/badge/Passing_Score-~825%20%2F%201000-blue?style=for-the-badge)](https://www.cisco.com/)
[![Practice Partner](https://img.shields.io/badge/Practice_Partner-CertsClub_(20%25_Off_Code:_club20)-28a745?style=for-the-badge&logo=shield)](https://www.certsclub.com/cisco/)

---

## 1. Exam Overview & Candidate Profile

The **Cisco 300-615 DCIT (Troubleshooting Cisco Data Center Infrastructure)** exam tests a network engineer's diagnostic and resolution methodologies across mission-critical data center platforms. The exam validates troubleshooting mastery across Cisco Nexus switching (NX-OS, vPC, VXLAN BGP EVPN, OSPF, BGP), Cisco Unified Computing System (UCS B-Series, C-Series, Intersight, HyperFlex), storage networking (Fibre Channel fabric logins, zoning conflicts, FCoE), and Cisco Application Centric Infrastructure (ACI faults, contracts, and endpoint tracking).

Passing the 300-615 DCIT exam earns the **Cisco Certified Specialist - Data Center Operations** certification and satisfies the concentration requirement for the **CCNP Data Center** certification.

### Target Candidate Profile & Roles
* **Data Center Tier-3 Escalation & Operations Engineer**
* **Senior Infrastructure Support Specialist**
* **Data Center Implementation & TAC Escalation Lead**
* **Unified Computing Systems Administrator**
* **Prerequisites:** In-depth troubleshooting experience across Cisco Nexus switches, MDS SAN directors, and Cisco UCS compute platforms (CCNP DCCOR level).

---

## 2. Key Exam Specifications

| Parameter | Official Specification |
| :--- | :--- |
| **Exam Code** | 300-615 |
| **Exam Name** | Troubleshooting Cisco Data Center Infrastructure (DCIT) |
| **Associated Credential** | Cisco Certified Specialist - Data Center Operations / CCNP Data Center |
| **Duration** | 90 Minutes |
| **Passing Score** | ~825 / 1000 (Scaled dynamic calibration) |
| **Question Count** | 55–65 questions |
| **Question Formats** | Multiple Choice (single/multiple select), Drag-and-Drop, CLI Trouble Tickets / Simlets |
| **Delivery Vendor** | Pearson VUE Authorized Test Centers & OnVUE Online Remote Proctored |
| **Practice Test Partner** | **[300-615 Practice Test](https://www.certsclub.com/cisco/)** (Coupon: `club20` for 20% off) |

---

## 3. Skills Measured & Blueprint Domain Weighting

| Domain Code | Domain Title | Exam Weight | Key Technical Objectives Covered |
| :--- | :--- | :---: | :--- |
| **1.0** | **Network Technologies** | **25%** | Troubleshooting routing protocols (OSPF, BGP) on Nexus; vPC issues (Type 1 consistency, split-brain scenarios, peer-link and keepalive failures); VXLAN BGP EVPN fabrics (underlay multicast, NVE interface, BGP EVPN Route Type 2/5 validation); Data Center QoS and PFC pause issues. |
| **2.0** | **Compute Platforms** | **25%** | Troubleshooting Cisco UCS B-Series/C-Series blade and rack servers; Service profile association failures (identity pool exhaustion, vNIC/vHBA placement, server qualification); LAN/SAN uplink pinning and failover; Cisco Intersight connectivity; HyperFlex cluster health and replication. |
| **3.0** | **Storage Area Networking** | **25%** | Troubleshooting Fibre Channel interface states (bit errors, SFP transceivers, B2B credit depletion); FC fabric login sequence (FLOGI, PLOGI, FCNS registration); SAN zoning and fabric merge conflicts; FCoE / FIP protocol negotiation failures. |
| **4.0** | **Automation and Security** | **25%** | Troubleshooting Cisco ACI fabric (faults, health score degradation, contract drops, leaf-spine discovery); Endpoint loop protection; AAA/TACACS+ on Nexus/UCS; CoPP drops on Nexus supervisor modules; Port Security, DAI, and DHCP Snooping. |

---

## 4. Scenario-Based Technical Practice Questions

### Scenario 1: Nexus vPC - Peer-Link Failure with Active Keepalive (Split-Brain Prevention)
**Topology Background:**  
Two Cisco Nexus 9300 switches operate as a vPC pair. Nexus-1 is the vPC Primary, and Nexus-2 is the vPC Secondary. A backhoe severs the fiber bundle carrying the dedicated 40Gbps **vPC Peer-Link**. However, the **vPC Peer-Keepalive** link (routed across an out-of-band management network) remains completely operational and active. What automated failover action does the secondary switch (Nexus-2) perform?

* A. Nexus-2 promotes itself to vPC Primary and begins forwarding all traffic.
* B. Because the peer-keepalive link is alive, Nexus-2 recognizes that Nexus-1 is healthy; to prevent a split-brain condition and bridge loops, Nexus-2 shuts down all of its local vPC member ports and its vPC SVIs.
* C. Both switches reload immediately.
* D. Nexus-2 takes over only the orphan ports while keeping vPCs open.

**Correct Answer:** **B**

**Detailed Technical Explanation:**  
* When the **vPC Peer-Link fails** while the **vPC Peer-Keepalive link remains UP**:
  1. The peer-keepalive link informs both switches that the peer switch is still alive and operational.
  2. If both switches continued forwarding traffic on their vPC member ports independently, dual-active split-brain behavior would occur, causing MAC address flapping, duplicate packets, and blackholing.
  3. By standard vPC design rules, the **Secondary switch (Nexus-2)** automatically disables all of its local vPC member ports and shuts down vPC VLAN SVIs.
  4. All downstream dual-homed servers detect the link drop on Nexus-2 and redirect 100% of traffic through the Primary switch (Nexus-1), preserving network stability.
* Distractor analysis: Option A describes an uncoordinated split-brain takeover. Option C fabricates destructive reloads. Option D leaves loop vulnerabilities active.

---

### Scenario 2: VXLAN BGP EVPN - Troubleshooting Route Installation
**Topology Background:**  
A network administrator deploys a new leaf switch (Leaf-103) into a VXLAN BGP EVPN fabric. The underlay OSPF network is fully converged, and BGP EVPN peering to the spine route reflectors is established. Running `show bgp l2vpn evpn` on Leaf-103 confirms that EVPN Route Type 2 (host MAC/IP) updates are being received from remote leaves. However, running `show ip route vrf Tenant-1` reveals that the host routes are **not** being installed into the tenant routing table. What is the root cause?

* A. The MTU on the spine uplinks is set to 9216.
* B. The Route Distinguisher (RD) on Leaf-103 is identical to Leaf-101.
* C. The VRF definition for `Tenant-1` on Leaf-103 has an omitted or mismatched **Route Target (RT) import** policy (`route-target import ...`).
* D. The leaf has too many TCAM carving profiles enabled.

**Correct Answer:** **C**

**Detailed Technical Explanation:**  
* In BGP EVPN:
  * The BGP EVPN table stores all received L2VPN EVPN NLRI routes.
  * For a route to transition from the BGP EVPN table into a specific tenant VRF's Forwarding Information Base (FIB), the route's **extended BGP Route-Target (RT) community** must match the **`route-target import`** statement configured under that tenant's VRF.
  * If the import RT is missing or incorrectly typed, the BGP process accepts the route into the global EVPN table, but cleanly filters it out from being imported into the tenant VRF routing table.
* Distractor analysis: Option A (Jumbo MTU) is required for data plane VXLAN, but does not prevent BGP control plane route installation. Option B (identical RD) is valid or benign in EVPN. Option D does not block BGP RIB installation.

---

### Scenario 3: Storage Networking - Fibre Channel Zone Merge Conflicts
**Topology Background:**  
An engineer connects an Inter-Switch Link (ISL) between two Cisco MDS 9148S SAN switches in `VSAN 10`. Immediately after the link initializes, the ISL port transitions to the **`down (zone merge failure)`** state. The engineer reviews the syslog and observes:
`%ZONE-2-ZS_MERGE_FAILED: Zone merge failed for VSAN 10, reason: Zone merge conflict`.
What caused this fabric merge failure?

* A. The two switches have different physical chassis serial numbers.
* B. Both switches have an active zoneset with the exact same zoneset name, but the active zoneset on Switch-A contains different zone members or zone definitions than Switch-B.
* C. The switches have identical Domain IDs configured in VSAN 10.
* D. Buffer-to-Buffer credits were set to 64 on both sides.

**Correct Answer:** **B**

**Detailed Technical Explanation:**  
* During a Fibre Channel fabric merge across an ISL:
  * The switches exchange their active zonesets and zone databases for each shared VSAN.
  * If the active zonesets on both switches share the **exact same name** but contain **conflicting contents** (e.g., a zone with the same name contains different WWPN members, or different zone pairs exist), a **Zone Merge Conflict** occurs.
  * To prevent corrupting either switch's active SAN configuration, the MDS switches isolate the ISL interface for that VSAN, transitioning the port to `down (zone merge failure)`.
* Distractor analysis: Option A is normal. Option C causes a Domain ID overlap failure (`isolated: domain ID conflict`), not a zone merge failure. Option D is standard flow control.

---

### Scenario 4: Cisco UCS Compute - Service Profile Association Allocation Failure
**Topology Background:**  
An administrator creates a new Service Profile from a template in Cisco UCS Manager and attempts to associate it with an available UCS B200 M5 blade server. The association wizard fails at 20% with the error: `Insufficient resources: MAC Address Pool Exhausted`. What corrective action must the administrator take to allow the association to complete?

* A. Reboot the UCS Fabric Interconnects.
* B. Expand the size of the referenced MAC Address Pool in UCS Manager or assign a different MAC pool that contains unallocated MAC addresses.
* C. Replace the server blade's mLOM virtual interface card.
* D. Delete the server's local RAID configuration.

**Correct Answer:** **B**

**Detailed Technical Explanation:**  
* Cisco UCS Service Profiles derive their virtual identifiers (MAC addresses, WWPNs, UUIDs) dynamically from pre-defined **Identity Pools**.
* If a pool reaches 100% allocation and an administrator attempts to provision a new Service Profile that requests additional vNIC MAC addresses from that exhausted pool, UCS Manager halts the association workflow with `Insufficient resources: MAC Address Pool Exhausted`.
* The administrator must navigate to `Servers > Pools > MAC Pools`, expand the block of available MAC address ranges (e.g., adding another block of 32 or 64 addresses), and re-trigger association.
* Distractor analysis: Options A, C, and D do not replenish software identity pools in UCS Manager.

---

### Scenario 5: Cisco UCS Networking - End-Host Mode Uplink Pinning Failure
**Topology Background:**  
A Cisco UCS domain operates in End-Host Mode. All server vNICs are pinned to upstream border ports on Fabric Interconnect A (FI-A). An engineer notices that all network traffic from Server 3's `vNIC 0` suddenly drops. Running `show pinning server-interfaces` on FI-A reveals that Server 3's vNIC is pinned to uplink port `Ethernet1/1`, which is currently in a `link-down` state, and no failover occurred. What configuration was omitted from the vNIC policy?

* A. The server was not configured for static IP routing.
* B. Fabric Failover was not enabled on the vNIC template in Cisco UCS Manager.
* C. The Fabric Interconnect was not running BGP.
* D. Spanning tree was disabled on the blade.

**Correct Answer:** **B**

**Detailed Technical Explanation:**  
* In Cisco UCS:
  * When a server's vNIC is defined, enabling **Fabric Failover** instructs the Cisco Virtual Interface Card (VIC) adapter to manage hardware link failover automatically.
  * If the primary Fabric Interconnect (FI-A) loses its upstream network uplinks, the VIC transparently redirects the vNIC's traffic internally to Fabric Interconnect B (FI-B).
  * Without Fabric Failover enabled, the vNIC remains pinned to FI-A; when FI-A's uplink fails, traffic is blackholed because the OS-level vNIC cannot transmit across the fabric.
* Distractor analysis: Option A is an OS configuration. Options C and D are not used in UCS End-Host Mode.

---

### Scenario 6: Cisco ACI Fabric - Endpoint Loop Protection (ELP) Activation
**Topology Background:**  
A server administrator accidentally connects both NICs of an unmanaged virtual switch into two separate leaf access switchports in an ACI fabric. Shortly afterward, APIC generates a major fault, and traffic to the server's MAC address (`0050.56b2.1122`) is completely suppressed. The APIC event log records: `EP-LOOP-DETECTED: Endpoint marked in freeze state`. What ACI security mechanism was triggered, and what did it do?

* A. Rogue EP Control / Endpoint Loop Protection detected that the MAC address was rapidly moving between leaf ports within a short timer threshold, and temporarily froze/quarantined the endpoint to prevent fabric-wide bridging loops.
* B. The ACI fabric erased the tenant VRF.
* C. The leaf switch rebooted to clear the TCAM.
* D. Spanning tree shut down all leaf switches in the pod.

**Correct Answer:** **A**

**Detailed Technical Explanation:**  
* In **Cisco ACI**:
  * Because traditional Spanning Tree BPDUs are typically filtered at the fabric boundary, physical cabling loops or misconfigured hypervisor vSwitches can cause an endpoint MAC/IP to flap rapidly between different leaf interfaces.
  * **Endpoint Loop Protection (ELP) / Rogue Endpoint Control:** Monitors the rate of endpoint moves across leaf ports.
  * When the number of moves exceeds the configured threshold (e.g., 4 moves in 60 seconds), ACI flags the MAC as a **Rogue Endpoint** and puts it into a **Freeze (quarantine) state**, disabling learning and dropping packets from that MAC to protect spine and leaf control plane tables from being overwhelmed.
* Distractor analysis: Options B, C, and D are destructive, non-existent, or incorrect architectural behaviors.

---

### Scenario 7: Fibre Channel - Buffer-to-Buffer (B2B) Credit Depletion Diagnostics
**Topology Background:**  
An enterprise storage team notices severe write latency on an all-flash array connected to a Cisco MDS 9700 director switch. A storage engineer runs:
```bash
MDS-1# show interface fc1/5 counters detailed
```
The output reports a steadily incrementing counter for:
`TxWait due to 0 BB credits: 45892104 frames`
What does this specific error counter indicate?

* A. The physical fiber cable has an optical attenuation break.
* B. The transmitter on port fc1/5 had frames queued and ready to send, but was forced to wait because the receiver at the other end of the link had exhausted all Buffer-to-Buffer credits (Slow-Drain device).
* C. The switch was infected with malware.
* D. The storage array is using iSCSI instead of Fibre Channel.

**Correct Answer:** **B**

**Detailed Technical Explanation:**  
* In Fibre Channel:
  * Flow control between ports is governed by **Buffer-to-Buffer (B2B) Credits**.
  * A port can only transmit if its B2B credit balance is greater than zero. Each transmitted frame decrements the credit count by 1, and each received `R_RDY` signal from the peer increments it by 1.
  * When a connected device (e.g., a congested storage array or misconfigured HBA) processes frames slower than they arrive (**Slow-Drain Device**), it stops returning `R_RDY` credits.
  * The transmitting MDS switch port exhausts its credits and pauses transmission, incrementing the **`TxWait due to 0 BB credits`** counter. This backpressure can cascade through the SAN fabric, causing head-of-line blocking.
* Distractor analysis: Option A would increment CRC or bit error counters, not clean TxWait credit counters. Options C and D are incorrect.

---

### Scenario 8: Nexus OSPF - MTU Mismatch on Routed Point-to-Point Links
**Topology Background:**  
An engineer configures an OSPFv2 point-to-point adjacency between two Cisco Nexus 9000 switches across interface `Eth1/1`. The command `show ip ospf neighbors` shows that the neighbor state remains permanently trapped in **`EXSTART/EXCHANGE`**. What is the most common cause of this condition on Cisco Nexus switches?

* A. The OSPF process IDs do not match on both switches.
* B. The Maximum Transmission Unit (MTU) configured on interface `Eth1/1` differs between the two switches (e.g., MTU 9216 vs. MTU 1500), causing Database Description (DBD) packet rejection.
* C. One switch is running NX-OS and the other is running Cisco IOS XE.
* D. The OSPF router priority is set to 0.

**Correct Answer:** **B**

**Detailed Technical Explanation:**  
* During the OSPF **ExStart/Exchange** state:
  * Routers negotiate the master/slave relationship and exchange **Database Description (DBD)** packets.
  * In Cisco NX-OS, OSPF includes the interface MTU in the DBD packet header.
  * If the local interface MTU does not match the neighbor's interface MTU, the router with the smaller MTU rejects the larger DBD packet from the peer, preventing link-state database synchronization and leaving the adjacency permanently stuck in **`EXSTART/EXCHANGE`**.
* Distractor analysis: Option A is false; OSPF process IDs are locally significant. Option C is false; OSPF is an open standard that interoperates across OS platforms. Option D only disables DR/BDR election on multi-access links.

---

### Scenario 9: Cisco HyperFlex - Troubleshooting Cluster Health and Read-Only State
**Topology Background:**  
A 3-node Cisco HyperFlex cluster experiences two concurrent physical hard drive failures on Node 1, followed by an unexpected power supply trip on Node 2. The HyperFlex Data Platform (HXDP) automatically transitions the storage cluster into a **`Read-Only`** state. Why does HXDP enforce a read-only lock under these multiple hardware failure conditions?

* A. The storage cluster license has expired.
* B. To prevent data inconsistency and split-brain corruption when the cluster loses required quorum and falls below its configured Replication Factor (RF) threshold.
* C. The hypervisor automatically formats the disks to NTFS.
* D. The Fabric Interconnects shut down all 10Gbps ports during power trips.

**Correct Answer:** **B**

**Detailed Technical Explanation:**  
* In **Cisco HyperFlex**:
  * Storage resilience is governed by **Zookeeper quorum** and the **Replication Factor (RF)** (RF2 requires at least 2 replicas; RF3 requires 3).
  * When simultaneous hardware failures exceed the cluster's fault tolerance threshold, the cluster can no longer guarantee that new write operations will be replicated across the required quorum of nodes.
  * To preserve existing data integrity and prevent data corruption, HXDP locks the file system into **Read-Only mode**. Existing data remains accessible to running VMs, but new writes are blocked until cluster health and node replication are restored.
* Distractor analysis: Options A, C, and D fabricate unrelated or destructive behaviors.

---

### Scenario 10: Security - Cisco Nexus Control Plane Policing (CoPP) Diagnostics
**Topology Background:**  
A network administrator suspects that a Denial-of-Service (DoS) attack is flooding a Nexus 7000 supervisor module with excessive SSH and SNMP traffic. Which NX-OS CLI command displays real-time packet conform and drop counters for all traffic classes policed by Control Plane Policing?

* A. `show policy-map interface control-plane`
* B. `show running-config copp`
* C. `show hardware internal statistics`
* D. `show logging logfile`

**Correct Answer:** **A**

**Detailed Technical Explanation:**  
* In Cisco NX-OS:
  * **`show policy-map interface control-plane`** displays the active runtime status of the CoPP policy applied to the supervisor engine.
  * It shows each defined traffic class (e.g., `copp-system-p-class-management`, `copp-system-p-class-critical`), the current packet rate, and the exact number of packets that were permitted (**conformed**) versus discarded (**dropped**) due to rate-limiting violations.
* Distractor analysis: Option B displays static configuration text without packet counters. Option C displays low-level ASIC hardware registers. Option D shows general syslog entries.

---

## 5. Recommended Study Resources & Official Documentation

* [Cisco 300-615 DCIT Official Exam Blueprint Topics](https://learningnetwork.cisco.com/s/dcit-exam-topics)
* [Cisco Press: CCNP Data Center Troubleshooting DCIT 300-615 Official Cert Guide](https://www.ciscopress.com/)
* [300-615 Practice Test - CertsClub](https://www.certsclub.com/cisco/) (Use coupon `club20` for 20% off)
* [Cisco Nexus 9000 Series Troubleshooting Guide](https://www.cisco.com/c/en/us/support/switches/nexus-9000-series-switches/products-troubleshooting-guides-list.html)
* [Cisco UCS Faults and Troubleshooting Guide](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-manager/products-troubleshooting-guides-list.html)
* [Cisco MDS 9000 SAN Troubleshooting Reference](https://www.cisco.com/c/en/us/support/storage-networking/mds-9000-series-multilayer-switches/products-troubleshooting-guides-list.html)

---

## 6. SEO Keywords & Search Index Topics

```
300-615, 300-615 exam, 300-615 practice test, 300-615 study guide, cisco 300-615,
dcit, cisco dcit, ccnp data center troubleshooting, troubleshooting cisco data center,
certsclub 300-615, nexus vpc peer link failure keepalive split brain,
vxlan evpn route target import missing vrf, mds san zone merge conflict zoneset,
ucs service profile mac address pool exhausted, ucs fabric failover vnic link-down,
aci endpoint loop protection rogue ep freeze, mds txwait 0 bb credits slow drain,
nexus ospf exstart mtu mismatch, hyperflex read-only quorum loss,
copp show policy-map interface control-plane drop counters
```

---

## 7. Community Discussions & Contributions

* **Troubleshooting Scenarios & Log Reviews:** Share Nexus vPC logs, ACI fault analyses, and SAN trace outputs in [GitHub Discussions](../../discussions).
* **Issue Submissions:** To report errata or suggest new scenario additions, open a ticket in [GitHub Issues](../../issues).
* **Lab Contributions:** Community trouble ticket labs for Cisco Modeling Labs (CML) and UCS Emulator are welcomed via Pull Requests.

---
*Maintained by the Cisco Certified Curriculum Community. Contributions and pull requests are welcomed.*
