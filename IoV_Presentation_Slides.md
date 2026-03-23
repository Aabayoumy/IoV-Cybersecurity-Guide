# Internet of Vehicles (IoV) Cybersecurity: Threats, VANET Security, and Defenses

---

## Slide 1: What is the Internet of Vehicles (IoV)?
**Introduction**

- **IoV Definition:** A distributed network integrating vehicles, roadside infrastructure, pedestrians, and backend systems through wireless communication technologies, enabling real-time data exchange for enhanced safety, efficiency, and user experience
- **Key Transformation:** Vehicles evolved from isolated mechanical machines into intelligent, interconnected computing nodes—modern Tesla and Waymo vehicles generate terabytes of sensor and camera data daily
- **IoV Ecosystem Components:** Connected cars/trucks (Tesla, GM OnStar), smart traffic lights and toll systems, smartphones with V2P apps, 5G cellular towers, and OEM cloud data centers
- **Communication Model:** Unlike traditional telematics (vehicle-to-server only with seconds/minutes latency), IoV enables Vehicle-to-Everything (V2X) communication with millisecond latency and bidirectional real-time data flow
- **Scale:** Approximately 400 million connected vehicles globally today, projected to reach 700 million by 2030

---

## Slide 2: Evolution from VANETs to Modern IoV
**Introduction**

- **Phase 1 - Mechanical Era (Pre-1970s):** Pure mechanical systems with manual controls, no internal networking, security limited to physical theft prevention
- **Phase 2 - Electronic Control Era (1970s-1990s):** Introduction of ECUs, ABS, OBD diagnostics, and the CAN bus protocol (1986)—designed for efficiency without authentication or encryption
- **Phase 3 - Connected Era (2000s-2010s):** Telematics systems (OnStar, BMW Assist), GPS navigation, cellular connections, OTA map updates—attack surface expanded dramatically
- **Phase 4 - Autonomous/Cooperative Era (2015-Present):** ADAS, V2X communication, 5G networks, edge computing, AI-powered perception—cyberattacks now have immediate physical consequences
- **Key Timeline Events:** CAN Bus introduced (1986), OBD-II mandated (1996), First vehicle hacking paper (2010), Jeep Cherokee hack (2015), UNECE WP.29 regulations adopted (2020)

---

## Slide 3: Why IoV Cybersecurity is Life-Critical
**Introduction**

- **Cyber-Physical Systems:** Unlike servers or smartphones, vehicles are cyber-physical systems where digital attacks have direct physical consequences—compromising software can affect braking, steering, or acceleration
- **The Jeep Cherokee Watershed (2015):** Researchers Miller & Valasek remotely compromised a Jeep at 70 mph via cellular connection, controlling dashboard, steering, brakes, and transmission—resulted in 1.4 million vehicle recall ($105M+ cost)
- **Attack Surface Explosion:** Modern vehicles have 10+ wireless interfaces: cellular (4G/5G), Wi-Fi, Bluetooth, DSRC/C-V2X, GPS, TPMS, key fob, OBD-II port, USB ports, and EV charging infrastructure
- **Real-Time Requirements:** Safety messages like "brake now" must be delivered in milliseconds—traditional security protocols that take seconds are completely inadequate
- **Consequence Comparison:** E-commerce data breach = stolen credit cards; connected vehicle compromise = potential injury or death

---

## Slide 4: The Safety vs. Privacy Paradox
**Introduction**

- **Accountability Requirement:** Safety systems need message trustworthiness—when a vehicle broadcasts a hard braking warning, other vehicles must verify authenticity; authorities must trace malicious messages after accidents
- **Privacy Requirement:** Vehicles continuously broadcast location, speed, and trajectory—without protection this enables stalking, mass surveillance, profiling by insurers, and theft planning
- **The Strava Heatmap Warning:** In 2018, aggregated "anonymized" fitness data revealed secret military base locations and patrol routes—the same mass surveillance risk applies to IoV if data isn't properly anonymized
- **Fundamental Tension:** Full accountability = zero privacy; full anonymity = zero accountability—finding the cryptographic and architectural balance is the core challenge
- **Solution Approaches:** Pseudonymous identity (temporary certificates that change frequently), group signatures (prove valid membership without revealing identity), mix-zones (areas where vehicles change identities simultaneously)

---

## Slide 5: Course Structure Overview
**Introduction**

This course consists of **7 core modules** covering essential IoV cybersecurity topics:

| Module | Title | Key Topics |
|--------|-------|------------|
| **1** | Architecture and Nodes | OBU, RSU, Cloud/Fog/Edge, V2X paradigms |
| **2** | Characteristics and Challenges | Mobility, latency, scalability, constraints |
| **3** | Routing Protocols and Secure Routing | VANET, AODV, GPSR, SAODV, ARAN |
| **4** | Attack Taxonomy | Network attacks, identity attacks, physical attacks |
| **5** | Authentication and Privacy-Preservation | Pseudonyms, mix-zones, privacy regulations |
| **6** | Cryptography and Key Management | PKI, ECC, SCMS, key lifecycle |
| **7** | Intrusion Detection Systems | Signature-based, anomaly-based, ML/AI approaches |

**Additional Topics:** V2X Vulnerabilities, Blockchain for IoV, Adversarial AI, Global Standards

---

## Slide 6: IoV Core Architecture - Network Nodes
**Module 1: Architecture and Nodes**

- **On-Board Unit (OBU):** The vehicle's communication and computing hub containing DSRC/C-V2X radios, 5G modem, GPS receiver, Hardware Security Module (HSM), and CAN bus gateway—generates and verifies digital signatures at 10 messages/second
- **Roadside Unit (RSU):** Stationary nodes at intersections, highway on-ramps, work zones, and toll plazas—broadcasts signal timing (SPaT), intersection geometry (MAP), aggregates vehicle data, provides edge computing for low-latency applications
- **Backend Systems:** Traffic Management Centers (TMC) for regional traffic optimization, Security Credential Management System (SCMS) for PKI operations, OEM clouds for telemetry and OTA updates, third-party services for navigation/insurance
- **Security Implications:** OBU compromise affects single vehicle; RSU compromise affects entire region; SCMS compromise causes complete trust collapse
- **Real-World Example:** US DOT's SCMS issues secure certificates to millions of vehicles and infrastructure nodes to ensure network-wide trust

---

## Slide 7: Three-Tier Computing Architecture
**Module 1: Architecture and Nodes**

- **Edge Computing (Vehicle/RSU):** Ultra-low latency (<10ms), limited processing power, direct sensor access, autonomous operation capability—handles collision avoidance, emergency braking, intersection assistance
- **Fog Computing (Regional Data Centers):** Low-to-moderate latency (10-100ms), moderate processing, regional scope—handles traffic optimization, platooning coordination, certificate validation caching, map tile distribution
- **Cloud Computing (Centralized):** Higher latency acceptable (100ms-seconds), massive processing/storage, global scope—handles ML model training, certificate generation, OTA update packaging, fleet management, long-term analytics
- **Tesla Architecture Example:** Vehicle edge processes 8 cameras + 12 ultrasonic sensors + radar locally for real-time decisions; cloud receives telemetry from millions of vehicles, trains neural networks, pushes OTA updates globally
- **Security Trade-off:** Cloud-centric models (like Tesla) risk fleet-wide compromise from single backend breach; distributed architectures reduce systemic risk but increase coordination complexity

---

## Slide 8: V2X Communication Paradigms
**Module 1: Architecture and Nodes**

- **V2V (Vehicle-to-Vehicle):** Direct communication between vehicles without infrastructure, 300-1000m range, <10ms latency, broadcasts Basic Safety Messages (BSMs) 10 times/second containing position, speed, heading, brake status—enables collision warning, emergency brake light, blind spot warning
- **V2I (Vehicle-to-Infrastructure):** Communication with RSUs, enables Signal Phase and Timing (SPaT) so vehicles know when lights will change, red light violation warnings, work zone alerts; Audi's Traffic Light Information feature uses V2I to tell drivers exactly when lights turn green
- **V2P (Vehicle-to-Pedestrian):** Communication with smartphones/wearables, C-V2X/Bluetooth-based, enables pedestrian crosswalk alerts and cyclist detection—challenges include device penetration and battery life
- **V2N (Vehicle-to-Network):** Cellular communication (4G/5G) for navigation, OTA updates, remote diagnostics, emergency calling (eCall), infotainment—provides kilometer+ range but 20-100ms latency
- **Security Requirements Differ:** V2V needs real-time signature verification; V2I needs RSU authentication and physical security; V2N needs end-to-end encryption and update integrity

---

## Slide 9: DSRC vs. C-V2X Technologies
**Module 1: Architecture and Nodes**

- **DSRC (IEEE 802.11p):** Dedicated Short-Range Communications operating at 5.9 GHz ITS band, up to 1000m range, <2ms latency, 3-27 Mbps—mature technology deployed since 2016, no cellular subscription required, works without infrastructure for pure V2V
- **C-V2X (Cellular V2X):** Based on 3GPP LTE/5G standards, PC5 interface for direct V2V at 5.9 GHz, Uu interface for cellular connection—up to 1500m range, evolves with cellular technology (5G NR offers <3ms latency)
- **DSRC Advantages:** Mature, specifically designed for vehicles, low latency, works in infrastructure-less scenarios
- **C-V2X Advantages:** Longer range, higher data rates, synergy with mobile network infrastructure, natural 5G/6G evolution path
- **Market Reality:** DSRC deployed in US, Japan, parts of EU; C-V2X gaining momentum in China and expanding globally; both technologies may coexist

---

## Slide 10: Security Implications of IoV Architecture
**Module 1: Architecture and Nodes**

- **Attack Surface by Component:** OBU radio (jamming, spoofing, replay), OBU gateway (CAN bus injection, firmware exploit), RSU (physical tampering, message injection), fog nodes (server compromise), cloud backend (mass data breach, malicious updates), PKI/SCMS (CA compromise, certificate theft)
- **Defense-in-Depth Approach:** Layer 1: Network security (ECDSA encryption, certificate authentication, IEEE 1609.2 protocols); Layer 2: Message security (digital signatures, plausibility checking, replay protection); Layer 3: Node security (HSMs, secure boot, tamper detection); Layer 4: Operational security (certificate revocation, intrusion detection, incident response)
- **Real-World Risk:** If Tesla's cloud backend is compromised, attackers could potentially push malicious OTA updates to millions of vehicles simultaneously—a catastrophic systemic risk
- **Key Takeaway:** No single security control protects the entire architecture—defense must be layered across all domains

---

## Slide 11: Unique Network Characteristics of IoV
**Module 2: Characteristics and Challenges**

- **Extreme High Mobility:** Vehicles travel 120+ km/h on highways, passing through RSU coverage in only 10-15 seconds—minimal time for authentication handshakes; Doppler effects cause frequency shifts that can corrupt cryptographic transmissions
- **Dynamic Topology:** Network structure changes every second as vehicles move, enter, exit; at a busy intersection during rush hour, dozens of vehicles arrive/depart per minute with connections lasting only 3-5 seconds
- **Massive Scale:** Single connected vehicle generates 25GB data/hour; major cities have millions of vehicles; BSMs broadcast 10x/second create enormous processing load; peak hour may concentrate hundreds of thousands of vehicles in small areas
- **Predictable Patterns:** Unlike random MANETs, vehicles follow roads and traffic rules—this predictability enables anomaly detection but also allows attackers to plan ambushes along known routes
- **Security Implication:** Traditional authentication requiring multiple round-trips (like TLS handshakes) is too slow; IoV needs sub-millisecond cryptographic operations

---

## Slide 12: Operational and Environmental Challenges
**Module 2: Characteristics and Challenges**

- **Strict Latency Requirements:** Collision warning needs <100ms, emergency braking <50ms, platooning <20ms, remote vehicle control <10ms—any security operation adding latency could cause accidents; cryptographic signature verification must complete in milliseconds
- **Unreliable Communication Links:** Urban canyons cause signal reflections/shadows, tunnels block signals, large trucks obstruct line-of-sight, weather (rain, snow, fog) attenuates signals, channel congestion in dense traffic
- **Variable Node Density:** Rush hour urban intersections may have hundreds of vehicles generating thousands of messages/second; rural highways at night may have isolated vehicles with no neighbors—security mechanisms must adapt to both extremes
- **Resource Constraints:** Processing budget competes with real-time sensor data, navigation, infotainment, diagnostics; ECDSA-256 verification at ~1.5ms × 500 messages/second = significant computational burden
- **Heterogeneous Environment:** Multiple vehicle manufacturers with different systems, various OBU/RSU vendors, legacy 10-year-old vehicles alongside new ones—consistent security across all is challenging

---

## Slide 13: Resource and Computational Constraints
**Module 2: Characteristics and Challenges**

- **Cryptographic Processing Requirements:** ECDSA signature generation ~0.5ms, verification ~1.5ms, certificate parsing ~0.3ms, hash computation ~0.01ms, random number generation ~0.1ms—multiplied by hundreds of messages/second creates significant load
- **Energy Considerations:** For EVs, every watt impacts range; cryptographic operations consume CPU power; continuous radio transmission requires energy; HSMs add parasitic power consumption—security designs must consider energy efficiency
- **Storage Limitations:** OBUs must store thousands of pseudonymous certificates, Certificate Revocation Lists (which can grow large), security logs, map data, firmware staging areas—all competing for finite storage
- **Multi-Vendor Interoperability:** No single-vendor standardization possible; security must work across different hardware, software stacks, cryptographic implementations; interface boundaries are vulnerability points
- **Legacy Integration Challenge:** Vehicle development takes 3-5 years, vehicles remain on roads 15-20 years, infrastructure deployment takes decades—modern IoV must coexist with older vehicles that have limited or no security capabilities

---

## Slide 14: VANET Fundamentals
**Module 3: Routing Protocols and Secure Routing**

- **VANET Definition:** Vehicular Ad-Hoc Network—subclass of Mobile Ad-Hoc Networks where vehicles act as mobile nodes that join/leave dynamically; self-organizing without central authority; each node can route messages; vehicles move at 30-150 km/h with topology changing every second
- **Key Characteristics:** Self-organizing (no central authority = trust establishment challenging), decentralized (every node routes = any node can become malicious), high mobility (rapid topology changes), intermittent connectivity (connections last seconds), no fixed infrastructure (pure V2V possible = cannot rely on trusted anchors)
- **Communication Modes:** Single-hop (direct, 300-1000m, lowest latency ~1-5ms, most reliable), multi-hop (extends range via intermediate forwarding, each hop adds latency, path breaks if any vehicle moves), broadcast (one-to-all for safety messages, no acknowledgment)
- **Protocol Stack:** Application (safety, traffic, comfort apps), transport (UDP for safety/low-latency, TCP for reliable transfer), network (AODV, DSR, GPSR routing), MAC (IEEE 802.11p or C-V2X), physical (5.9 GHz band)
- **VANET vs. Traditional Networks:** Traditional = fixed topology, trusted infrastructure, centralized routing, static security; VANET = dynamic topology (changes every second), no central authority, every node routes, trust established on-the-fly

---

## Slide 15: VANET Routing Protocol Types
**Module 3: Routing Protocols and Secure Routing**

- **Proactive (Table-Driven) Routing:** Maintain routing tables continuously; routes available immediately; high overhead in dynamic VANETs; examples: OLSR, DSDV
- **Reactive (On-Demand) Routing:** Discover routes only when needed; lower overhead; initial latency for route discovery; examples: AODV, DSR
- **Position-Based (Geographic) Routing:** Use GPS coordinates for routing decisions; forward to neighbor closest to destination; no routing tables needed; examples: GPSR, GSR, GPCR
- **Cluster-Based Routing:** Group vehicles into clusters; cluster heads handle inter-cluster routing; reduces overhead in dense networks; examples: CBLR, LORA-CBF
- **Fundamental Problem:** All routing protocols assume cooperative honest nodes—but in adversarial environments, any node can lie about position, fabricate routes, or silently drop packets

---

## Slide 16: AODV and GPSR Protocols
**Module 3: Routing Protocols and Secure Routing**

- **AODV (Ad-hoc On-demand Distance Vector):** Discovers routes via RREQ (Route Request) / RREP (Route Reply); source broadcasts RREQ, destination responds with RREP, data flows along discovered path
- **AODV Vulnerabilities:** No authentication of RREQ/RREP messages; sequence numbers can be forged; hop count can be manipulated; route replies can be spoofed—designed assuming all nodes are cooperative and honest
- **GPSR (Greedy Perimeter Stateless Routing):** Uses GPS coordinates, forwards to neighbor closest to destination; perimeter mode activates when greedy fails (no neighbor closer to destination)
- **GPSR Vulnerabilities:** Relies entirely on position information that can be spoofed; attacker claims position closer to destination → attracts all traffic; no verification of claimed location
- **Protocol Comparison:** AODV vulnerable to sequence number manipulation (route hijacking); DSR vulnerable to route cache poisoning (traffic interception); OLSR vulnerable to MPR selection manipulation (network isolation); GPSR vulnerable to position falsification (traffic misdirection)

---

## Slide 17: Secure Routing Protocols
**Module 3: Routing Protocols and Secure Routing**

- **SAODV (Secure AODV):** Adds digital signatures and hash chains to AODV; immutable fields signed by source; hash chain protects hop count (h³(seed) → h²(seed) → h¹(seed), verifier checks h(received) = previous); limitations: signature overhead, doesn't prevent wormhole, doesn't address insider threats
- **ARAN (Authenticated Routing for Ad-hoc Networks):** End-to-end authentication using certificates from trusted CA; Route Discovery Packet contains source certificate, signed by source, each forwarder appends signature; prevents impersonation, detects modification
- **SEAD (Secure Efficient Ad-hoc Distance Vector):** Uses one-way hash chains instead of signatures for better performance; hash computation ~1000× faster than signing, verification ~10000× faster; suitable for resource-constrained VANETs
- **Position Verification (PLV):** Vehicle claims position → nearby verifiers check signal strength, direction of arrival, consistency with previous positions → majority vote determines acceptance
- **Security vs. Performance Trade-off:** Adding authentication to every packet must not introduce latency that makes safety messages arrive too late—balance is critical

---

## Slide 18: VANET Defense Mechanisms
**Module 3: Routing Protocols and Secure Routing**

- **Trust Management:** Calculate trust score = α × DirectTrust (own observations) + β × IndirectTrust (network feedback) + γ × RecommendedTrust (trusted third parties); update: successful interaction = +reward, failed = -penalty, decay over time; forwarding decision: select neighbor with highest (proximity × trust) score
- **Watchdog Mechanism:** After sending packet to forwarder, continue monitoring forwarder's transmissions; if forwarder doesn't retransmit within timeout, mark as suspect; Pathrater works with watchdog to rate paths, avoid suspected malicious nodes
- **Specification-Based Detection:** Rules from physics and protocols—speed consistency (claimed vs. calculated), position consistency (distance ≤ max_possible), message rate limits, geographic consistency (position on road network); violations indicate attack or malfunction
- **Wormhole Detection via Packet Leashes:** Geographic leash (sender includes GPS position + timestamp; receiver verifies distance ≤ speed_of_light × time_difference), temporal leash (tight time synchronization, reject packets exceeding max delay)
- **Sybil Prevention:** Resource testing (computational puzzle with time limit—single device solves one), position verification (respond within time proportional to distance), certificate-based (limited pseudonyms from CA)

---

## Slide 19: Threat Actors Targeting IoV
**Module 4: Attack Taxonomy**

- **Nation-State Actors:** Motivations include espionage, infrastructure disruption, military advantage; capabilities include APTs, zero-days, supply chain attacks; targets include critical infrastructure, military vehicles, government officials; have virtually unlimited funding and legal immunity
- **Organized Crime:** Motivated by financial gain through vehicle theft, cargo theft, ransomware, insurance fraud; capable of international operations with significant technical expertise; target high-value vehicles and commercial fleets
- **Individual Criminals:** Motivated by personal gain, vehicle theft, stalking; use off-the-shelf tools, social engineering; In 2022 Moscow, hackers used Yandex taxi app API to order dozens of taxis to same location, creating massive artificial traffic jam
- **Insider Threats:** Employees with authorized access, system knowledge, trust relationships—can be motivated by revenge, financial gain, or coercion; can introduce backdoors during development
- **Security Researchers:** Often allies rather than threats; responsible disclosure improves security; Miller & Valasek's Jeep hack catalyzed industry-wide security improvements and influenced UNECE WP.29 regulations

---

## Slide 20: Network and Routing Attacks
**Module 4: Attack Taxonomy**

- **Blackhole Attack:** Malicious vehicle advertises optimal routes with attractive metrics (lowest hop count), attracts traffic, then drops all packets—safety messages disappear, collision warnings never reach destinations, emergency vehicle notifications lost
- **Grayhole Attack:** More sophisticated than blackhole—attracts traffic but selectively processes packets: drops only emergency warnings while forwarding entertainment traffic; records traffic for intelligence gathering; delays critical messages just enough to reduce effectiveness
- **Wormhole Attack:** Two colluding attackers create tunnel between distant network locations using private out-of-band connection (cellular, dedicated link)—distant nodes appear adjacent, routing tables corrupted, traffic analysis enabled, geographic routing completely broken
- **DDoS Attack:** Multiple compromised nodes flood targets (RSUs, certificate servers, traffic management) with garbage messages—certificate server DDoS means vehicles can't validate messages and trust collapses network-wide
- **Detection Challenge:** These attacks often use valid credentials and pass authentication; detection requires behavioral analysis and multi-point correlation

---

## Slide 21: CAN Bus - The Fundamental Vulnerability
**Module 4: Attack Taxonomy**

- **CAN Bus Background:** Controller Area Network developed in 1980s for reliable, efficient in-vehicle communication—connects 70-100+ ECUs controlling engine, transmission, brakes, steering, airbags
- **Fatal Design Weaknesses:** No authentication (any device can send any message), no encryption (all plaintext, any device reads all traffic), broadcast architecture (all messages to all nodes), priority-based access (attackers can dominate bus), no message freshness (replay attacks trivial)
- **Remote Access Paths to CAN:** Cellular/telematics connections, WiFi/Bluetooth in infotainment, compromised mobile apps, malicious OTA updates—once attackers reach infotainment, they can often pivot to CAN bus through gateway
- **Physical Access Paths:** Direct OBD-II port connection (under steering wheel), compromised repair equipment, malicious aftermarket devices, supply chain tampering
- **The Jeep Hack Attack Chain:** Exploited open port on cellular IP → root access on infotainment → discovered gateway allowed CAN messages → controlled steering/brakes/transmission remotely at highway speeds

---

## Slide 22: Identity and Trust-Based Attacks
**Module 4: Attack Taxonomy**

- **Sybil Attack:** Single malicious entity creates multiple fake identities simultaneously—in V2X, one vehicle creates 50 fake IDs broadcasting false congestion, making alternate routes appear jammed while attacker's route is clear; can also overwhelm voting/consensus systems
- **Spoofing Attacks:** GPS spoofing with $300 SDR tricks vehicles into wrong location; Certificate spoofing requires compromised PKI but enables complete identity assumption; Emergency vehicle impersonation triggers traffic preemption and yields from other vehicles
- **Replay Attack:** Record valid "ice on road" warning in winter, replay in summer causing unnecessary caution; record key fob unlock signal, replay later to steal vehicle (RollJam attack used by car thieves today)
- **Impersonation Targets:** Emergency vehicles (clear traffic for criminal escape), RSUs (broadcast false warnings, distribute malware), Certificate Authorities (complete network compromise)
- **Prevention Requirements:** Strong cryptographic authentication, certificate pinning, multi-factor verification, behavioral analysis, rapid revocation mechanisms

---

## Slide 23: Physical and Data Attacks
**Module 4: Attack Taxonomy**

- **GPS Spoofing Danger:** Position is fundamental to safety messages, navigation, trust decisions, and time synchronization (derived from GPS); researchers demonstrated $300 SDR spoofing that redirected ships in Black Sea, fooled drones, tricked self-driving cars into wrong turns
- **ECU Exploitation:** Firmware extraction via JTAG/debug ports, reverse engineering reveals vulnerabilities and cryptographic keys, malicious firmware injection creates persistent backdoors, calibration manipulation changes vehicle behavior
- **Supply Chain Attacks:** Components compromised during manufacturing, malicious firmware from vendors, hardware backdoors implanted during assembly, tampering during shipping/storage—similar to SolarWinds attack pattern
- **Long-Term Persistent Access:** Firmware implants survive reboots, boot sector modifications persist through updates, compromised OTA mechanisms reinstall malware, sleeper malware activates only under specific conditions—may survive factory resets
- **Position Falsification:** Claim to be at intersection when actually elsewhere; victim receives false collision warning; brakes unnecessarily; may cause real accident with following vehicles

---

## Slide 24: The Authentication vs. Privacy Dilemma
**Module 5: Authentication and Privacy-Preservation**

- **Data Generation Volume:** Single connected vehicle generates 25+ GB/hour, 4 TB/day; location updates 10 times/second; hundreds of sensor readings continuously
- **Data Categories Collected:** Vehicle state (GPS position, velocity, acceleration, brake status), environmental (camera imagery, LiDAR, radar), driver behavior (driving style, reaction times, attention), connected services (navigation destinations, voice commands, media consumption)
- **The Core Conflict:** Full accountability (trace every message to source) = zero privacy (complete surveillance); full anonymity (untraceable messages) = zero accountability (cannot identify malicious actors)
- **Technical Challenge:** Can we authenticate "message from valid network participant" without revealing "message from vehicle #12345"? Can we enable accountability for authorities while preventing surveillance by others?
- **Re-identification Research:** Studies show 4 spatiotemporal points identify 95% of individuals; home and work location identify most people; unique commute patterns are near-universal—"anonymization" often fails

---

## Slide 25: Privacy-Preserving Technologies
**Module 5: Authentication and Privacy-Preservation**

- **Pseudonymous Certificates:** Vehicle receives thousands of temporary certificates from SCMS; uses one active certificate at a time; rotates periodically (e.g., every 5 minutes); SCMS design specifies ~3,000 certificates per vehicle (20/week × 3 years)
- **Mix-Zones:** Designated geographic areas (intersections, parking structures) where vehicles simultaneously change pseudonyms; all vehicles enter with old IDs, stop broadcasting, change IDs, exit with new IDs; observer cannot link entry and exit identities
- **Silent Periods:** Vehicle ceases broadcasting before pseudonym change, waits several seconds, resumes with new identity—simple but creates safety risk during silence; modern approaches use context-aware shorter silences in high-risk areas
- **Group Signatures:** Vehicle signs as "member of valid vehicle group" without revealing which member; verifier confirms valid membership; authority can "open" signature in emergency to reveal signer—strong privacy for normal operations, accountability preserved for emergencies
- **Differential Privacy:** Adds calibrated noise to data queries ensuring no individual's data significantly affects output—useful for aggregated traffic data while protecting individual presence

---

## Slide 26: Regulatory Privacy Framework
**Module 5: Authentication and Privacy-Preservation**

- **GDPR Applicability (EU):** Location data is personal data; vehicle telemetry often identifiable to person; requires lawful basis (safety = "vital interests," commercial = consent), data minimization (collect only necessary), purpose limitation (safety data cannot be used for marketing), storage limitation, data subject rights (access, deletion, portability)
- **CCPA (California):** Applies to California residents' vehicle data; requires disclosure of data collection; provides opt-out rights for sale of data; vehicle manufacturers must comply for California market
- **Cross-Border Challenges:** Vehicles travel internationally collecting data in multiple jurisdictions; different privacy requirements; data localization requirements (especially China); mutual recognition issues
- **Privacy by Design Principles:** Data minimization (collect only essential), purpose specification (define purposes before collection), use limitation (technical controls on data use), edge processing (process locally when possible, transmit only results)
- **Implementation Approaches:** Encryption (stored and transmitted data), access control (role-based with audit logging), anonymization (remove identifiers, generalize quasi-identifiers, test against re-identification)

---

## Slide 27: Cryptographic Foundations for IoV
**Module 6: Cryptography and Key Management**

- **Security Requirements:** Authentication (verify messages from legitimate participants), integrity (detect modifications), non-repudiation (prevent denial of transmission for accident investigation), confidentiality (protect sensitive data), availability (cryptographic operations must not delay safety messages)
- **Performance Challenge:** Traditional RSA-2048 verification takes 0.5-2ms; receiving 500 messages/second = 250-1000ms just for verification; per-message processing budget is only ~2ms including parsing, application logic, response
- **Why ECC for IoV:** Equivalent security with smaller keys (256-bit ECC = 3072-bit RSA security), faster operations (5× faster signing, 500× faster key generation); ECDSA P-256 is the V2X standard
- **ECDSA (Elliptic Curve Digital Signature Algorithm):** Signs messages using elliptic curve mathematics; verification confirms message authenticity and integrity; critical that random number generation is secure—poor randomness enables key recovery
- **Standard Curves:** P-256 (secp256r1) most common for V2X with 128-bit security; P-384 for higher security; Curve25519 as modern alternative; BrainpoolP256r1 as European alternative

---

## Slide 28: Security Credential Management System (SCMS)
**Module 6: Cryptography and Key Management**

- **SCMS Components:** Root CA (highest trust anchor, air-gapped, rarely used), Intermediate CA (operational, handles volume), Enrollment CA (issues long-term enrollment certificates for vehicle identity), Pseudonym CA (issues short-term pseudonymous certificates—multiple PCAs for privacy), Registration Authority (validates requests, policy enforcement), Linkage Authority (holds values connecting pseudonyms—split between parties for privacy), Misbehavior Authority (receives reports, determines revocation)
- **Certificate Lifecycle:** Initial enrollment (manufacturer provisions OBU → contacts Enrollment CA → enrollment certificate issued), pseudonym request (uses enrollment cert → requests batch of pseudonyms → linkage values embedded), usage (select pseudonym → sign messages → rotate per policy), revocation (misbehavior detected → Linkage Authorities provide linkage → all pseudonyms identified → CRL distributed)
- **Privacy-Preserving Accountability:** Linkage values connect pseudonyms but split across multiple Linkage Authorities; regular operations cannot link; only coordinated legal process can reveal true identity
- **CRL Challenges:** Certificate Revocation Lists grow with each revocation; distribution is bandwidth-intensive; vehicles may miss updates; latency exists between revocation decision and enforcement
- **Real-World Implementation:** US DOT's SCMS designed to manage billions of certificates for millions of vehicles

---

## Slide 29: Message Authentication and Key Management
**Module 6: Cryptography and Key Management**

- **IEEE 1609.2 Security Format:** All V2X messages secured with protocol version, content/data, header info (generation time, expiry, location, certificate), ECDSA signature covering content + headers
- **Certificate Options:** Full certificate included (no prior knowledge needed, bandwidth-intensive) or certificate digest (8-byte hash, receiver must have certificate, much smaller)
- **Verification Process:** Parse message → extract/find certificate → verify chain to trusted root → check validity period → check not revoked → verify ECDSA signature → check timestamp freshness → check content plausibility
- **Performance Optimizations:** Certificate caching (store recently seen, LRU eviction), batch verification (verify multiple signatures together, ~30% throughput improvement), hardware acceleration (dedicated crypto co-processor essential for high message rates)
- **Key Management:** On-vehicle generation (private keys never leave OBU), HSM storage (tamper-resistant, side-channel resistant), pseudonym pool management (track available, schedule rotation, request refills before exhaustion)

---

## Slide 30: Advanced Cryptographic Techniques
**Module 6: Cryptography and Key Management**

- **Zero-Knowledge Proofs:** Prove you know something without revealing what—prove valid certificate without revealing which one; prove vehicle meets standards without revealing VIN; prove moving (not stationary attacker) without revealing trajectory
- **Attribute-Based Credentials:** Certificates that certify attributes ("valid vehicle in jurisdiction," "emergency vehicle," "commercial >10 tons") rather than identity; selective disclosure reveals only needed attributes
- **Implicit Certificates (ECQV):** Combine public key and certificate data; reconstruction value enables key derivation during verification; reduces certificate size from ~120 bytes (X.509+ECDSA) to ~50 bytes
- **Post-Quantum Cryptography Threat:** Quantum computers using Shor's algorithm can break RSA and ECDSA; current vehicles may still be operational when quantum computers arrive; "harvest now, decrypt later" attacks threaten current sensitive data; migration planning and crypto-agility needed now
- **Implementation Security:** Side-channel attacks (timing, power, EM emanations) can reveal keys; requires constant-time implementations, power masking, EM shielding; random number generation critical—poor randomness = complete system compromise

---

## Slide 31: Why Vehicular IDS is Essential
**Module 7: Intrusion Detection Systems**

- **Defense in Depth Requirement:** Authentication is first line (verify credentials), IDS is second line (monitor for malicious behavior from authenticated entities)—what if authenticated vehicle is compromised, credentials stolen, or zero-day exploit used?
- **Why Authentication Isn't Enough:** Compromised vehicles have valid credentials; insider threats have legitimate access; stolen keys pass verification; zero-day exploits bypass protocol checks; supply chain attacks embed valid credentials
- **Unique Vehicular IDS Challenges:** High mobility (baseline behavior varies by location), real-time requirements (millisecond response needed, cannot queue alerts), resource constraints (limited processing/storage in OBUs), heterogeneous environment (different vehicles, manufacturers, legacy systems), privacy concerns (monitoring must not enable surveillance)
- **IDS Role:** Monitor for activity that uses valid credentials, passes authentication, appears legitimate to protocol verification—detect behavioral anomalies and specification violations
- **Real-World Example:** CAN bus voltage monitoring IDS detects slight electrical anomalies when malicious device plugged in—even if attacker has valid message format, physical layer reveals intrusion

---

## Slide 32: IDS Detection Methods
**Module 7: Intrusion Detection Systems**

- **Signature-Based Detection:** Compare activity against database of known attack patterns; fast detection, low false positives for known attacks; cannot detect zero-days; requires continuous signature updates; attackers can modify attacks to evade
- **Anomaly-Based Detection:** Learn "normal" behavior baseline, flag significant deviations; can detect unknown attacks; adapts to environment; higher false positive rate; attackers can slowly train model to accept malicious behavior
- **Specification-Based Detection:** Monitor for violations of predefined rules based on physics and protocol specifications—vehicle speed cannot exceed 250 km/h, cannot accelerate >15 m/s², cannot be in two places simultaneously, position must follow road network, BSM must include mandatory fields, timestamps must be recent; low false positive rate, no training required
- **Context-Aware AI:** Traditional IDS lacks context (high message rate = attack OR rush hour); incorporate vehicle sensors, environmental conditions, historical patterns, neighbor data; example: verify hard braking claim against own radar, neighbor messages, weather, reputation
- **Federated Learning for IDS:** Vehicles train local models on local data; share model updates (not raw data); central server aggregates; privacy preserved while building global threat detection

---

## Slide 33: Misbehavior Detection Systems
**Module 7: Intrusion Detection Systems**

- **MDS vs. Traditional IDS:** Traditional IDS focuses on network intrusions and protocol violations; MDS focuses on false vehicular data by comparing V2X messages against physical reality and sensor data
- **Plausibility Checks - Position:** Road network matching (is claimed position on valid road?), physical possibility (distance from last position ≤ time × max_speed), consistency with neighbors (do nearby vehicles confirm existence?), signal direction (does RF direction match claimed position using directional antennas?)
- **Plausibility Checks - Speed:** Absolute limits (no vehicle exceeds ~500 km/h), acceleration limits (max ~15 m/s² for acceleration, ~20 m/s² for emergency braking), road type consistency (highway speeds on residential street = suspicious), consistency (speed × time ≈ distance traveled)
- **Sensor Fusion Verification:** V2X claims "vehicle ahead braking hard" → cross-check camera (vehicle visible?), radar (closing rate?), LiDAR (confirm position), speed sensor (approaching?) → all agree = trust; all disagree = reject and alert
- **Response Actions:** Local (drop suspicious messages, reduce trust weight, fall back to local sensors, driver alerts), network (report to Misbehavior Authority, temporary local blacklist, share with nearby RSUs)

---

## Slide 34: ML-Based Intrusion Detection
**Module 7: Intrusion Detection Systems**

- **Supervised Learning:** Train on labeled normal/malicious examples using SVM, Random Forests, Neural Networks; challenge: obtaining labeled vehicular attack data is difficult; class imbalance (attacks rare)
- **Unsupervised Learning:** Learn normal patterns without labels using clustering, Isolation Forest, Autoencoders; can detect unknown attacks; challenge: defining anomaly threshold, handling legitimate outliers
- **Deep Learning Architectures:** CNN for spatial patterns, RNN/LSTM for temporal sequences, Autoencoders for anomaly detection, Transformers for complex reasoning—end-to-end attack detection
- **Feature Engineering:** Network traffic (message rate, unique sources, protocol compliance), vehicle behavior (speed consistency, acceleration profile, heading alignment, position plausibility), temporal (inter-message interval, sequence progression, pattern periodicity)
- **Federated Learning Implementation:** Each vehicle trains locally → shares model updates (not data) → central aggregation → improved global model distributed → repeat; benefits: data stays local (privacy), reduced bandwidth, leverages diverse data; challenges: non-IID distribution, model poisoning attacks

---

## Slide 35: Blockchain Fundamentals for IoV
**Additional: Blockchain for IoV Security**

- **Core Properties:** Decentralization (no single controlling entity, data replicated across nodes), immutability (once recorded cannot be altered, cryptographic hashes link blocks), transparency (all participants can view/audit ledger), consensus (agreement mechanism for adding data)
- **Why Blockchain for IoV:** Traditional centralized PKI creates single points of failure—if hackers compromise the central SCMS, entire trust system collapses; blockchain distributes verification across thousands of nodes
- **Consensus Mechanisms:** Proof of Work (too slow/expensive for IoV), Proof of Stake (faster, validators stake tokens), PBFT (voting-based, fast finality, better for permissioned IoV), IoV-specific concepts like "Proof of Driving" (consensus weight based on driving behavior)
- **Smart Contracts:** Self-executing code that automatically enforces agreements—vehicles can automatically pay tolls, pay EV charging stations, negotiate platooning membership without relying on central servers
- **Permissioned vs. Permissionless:** Public blockchains (Bitcoin, Ethereum) are slow and expensive; permissioned consortium blockchains (Hyperledger Fabric) with known participants better suited for IoV—consortium includes OEMs, infrastructure operators, government, insurers

---

## Slide 36: Blockchain Applications in IoV
**Additional: Blockchain for IoV Security**

- **Decentralized Identity Management:** Vehicle registers identity on blockchain; public key recorded in transaction; certificate validity verifiable by any node; revocation recorded instantly visible to all—no single point of failure, instant revocation propagation, cross-border trust without bilateral agreements
- **Secure OTA Updates:** Manufacturer creates update → signs it → records cryptographic hash on blockchain; vehicle downloads update → computes hash → compares with blockchain-recorded hash → verifies signature → installs only if all match; attacker cannot modify (hash mismatch), cannot inject fake (signature fails), rollback detectable (blockchain history)
- **Immutable Event Logging (Digital Black Box):** Critical events (collision warnings, emergency braking, sensor anomalies, security alerts) hashed, signed, submitted to blockchain; events cannot be retroactively modified; timestamps network-verified; survives vehicle destruction if transmitted; provides cryptographic evidence for accident investigation and liability determination
- **Decentralized Trust/Reputation:** Good behavior (accurate messages, consistent data, participates in consensus) → +reputation; bad behavior (false claims, inconsistent data, misbehavior) → -reputation; trust decisions check blockchain reputation score; messages weighted by sender reputation
- **Platooning Coordination:** Smart contract defines platoon rules and membership; critical commands signed and logged; disputes resolvable via logs; automatic micropayments for leading vehicle (expends more fuel)

---

## Slide 37: Adversarial AI - The Threat to Autonomous Vehicles
**Additional: Adversarial AI**

- **AI Role in Autonomous Vehicles:** Object detection (vehicles, pedestrians, obstacles), lane detection (road boundaries), traffic sign recognition, semantic segmentation, depth estimation, sensor fusion, path planning, motion control
- **Why Neural Networks Are Vulnerable:** High dimensionality (millions of pixels = many adversarial perturbations possible), non-linear decision boundaries (small input changes can cross boundaries), lack of uncertainty quantification (confident wrong predictions), distributional assumptions (trained on specific data, real world differs)
- **What Are Adversarial Examples:** Inputs crafted to cause incorrect predictions by adding carefully calculated perturbations—often imperceptible to humans but highly effective against models; can be targeted (cause specific misclassification)
- **Attack Types:** White-box (full model access, most powerful), black-box (query-only access, must infer or transfer), gray-box (partial knowledge)
- **Attack Algorithms:** FGSM (Fast Gradient Sign Method—single step, fast), PGD (Projected Gradient Descent—iterative, stronger), C&W (Carlini & Wagner—optimization-based, very effective)

---

## Slide 38: Physical Adversarial Attacks
**Additional: Adversarial AI**

- **Digital vs. Physical:** Digital attacks modify pixel values directly (unrealistic for real vehicles); physical attacks modify real-world objects and must survive environmental variation (viewing angle, lighting, distance, weather)
- **Stop Sign Attack:** Researchers placed carefully calculated black/white stickers on stop signs; human drivers still clearly saw stop sign; autonomous vehicle AI classified as "Speed Limit 45" and accelerated through intersection; 80%+ misclassification rates across multiple architectures
- **Lane Detection Attack:** Small stickers or painted markings create fake lane lines; Tesla Autopilot followed fake markers, diverting vehicle across lanes; can lead vehicle off road or into opposing traffic
- **LiDAR Spoofing:** $60 of components (laser diode synchronized to victim LiDAR); inject fake laser pulses to create phantom obstacles (cause unnecessary braking) or hide real obstacles (cause collision); demonstrated against commercial LiDAR
- **Sensor Blinding/Jamming:** Bright lights/lasers saturate cameras; radar jammers flood frequencies with noise (commercial devices available, illegal but obtainable); sunlight or artificial sources overwhelm LiDAR; creates temporary or permanent blindness

---

## Slide 39: AI Defenses and Secure System Design
**Additional: Adversarial AI**

- **Defense: Adversarial Training:** Include adversarial examples in training with correct labels; model learns to correctly classify adversarial inputs; limitations: computationally expensive, may reduce clean accuracy, only robust to seen attacks
- **Defense: Input Preprocessing:** Denoising, JPEG compression, random resizing/padding—removes some perturbations; limitations: adaptive attacks can evade, may degrade input quality, adds latency
- **Defense: Multi-Sensor Fusion:** Attack must fool multiple sensor types (camera + LiDAR + radar); cross-validation between sensors; disagreement triggers safe fallback; if camera sees obstacle but LiDAR doesn't confirm, investigate before acting
- **Defense in Depth for AI:** Layer 1: Robust models, Layer 2: Input validation, Layer 3: Sensor fusion, Layer 4: Behavioral monitoring, Layer 5: Safe fallbacks (human takeover, conservative defaults)
- **Uncertainty Quantification:** Know when model is confident vs. uncertain; don't act on uncertain predictions; request human intervention when uncertain

---

## Slide 40: UNECE WP.29 Regulations
**Additional: Global Standards**

- **Regulatory Evolution:** Pre-2015 = voluntary guidelines, cybersecurity as afterthought; 2015 Jeep hack = watershed moment; 2020 = UNECE WP.29 regulations adopted; now = mandatory compliance in 60+ countries including EU, Japan, Korea, Australia
- **UN Regulation No. 155 (CSMS):** Requires certified Cybersecurity Management System for vehicle type approval; governance (board-level accountability, defined responsibilities, adequate resources), processes (risk assessment, security by design, testing, incident response), operations (threat monitoring, vulnerability management, update deployment)
- **Threat Categories (R155 Annex 5):** Backend servers (certificate authority compromise), communication channels (V2X spoofing), update procedures (malicious updates), external connectivity (OBD attacks), data/code (extraction, modification), physical (sensor attacks, tampering)
- **Compliance Timeline:** July 2022 = CSMS certification required for new type approvals; July 2024 = all new vehicles sold must have CSMS; certificate valid 3 years, must be renewed, can be withdrawn for non-compliance
- **UN Regulation No. 156 (SUMS):** Software Update Management System required; documented processes, security assessment for updates, validation before deployment, rollback capability, authentication and integrity protection

---

## Slide 41: ISO/SAE 21434 Standard
**Additional: Global Standards**

- **Relationship to WP.29:** R155 mandates WHAT must be done; ISO 21434 describes HOW to do it; compliance with 21434 supports R155 compliance; not strictly mandatory but effectively required
- **Cybersecurity Engineering Lifecycle:** Concept phase (define item, perform TARA), product development phase (design, implement, verify), post-development phase (monitor, respond, maintain); throughout: cybersecurity management
- **TARA (Threat Analysis and Risk Assessment):** Asset identification (what needs protection, CIA properties), threat identification (who might attack, methods, attack trees), impact assessment (safety/financial/operational/privacy impact → negligible to severe), attack feasibility assessment (time, expertise, knowledge, opportunity, equipment → feasibility rating), risk determination (combine impact + feasibility via matrix), risk treatment (avoid, reduce, transfer, accept)
- **Cybersecurity Assurance Levels (CAL):** CAL 1-4 define rigor of development and verification; CAL 1 (low risk, self-review), CAL 2 (moderate, peer review), CAL 3 (high risk, expert review), CAL 4 (critical, independent review + red team penetration testing)
- **Supply Chain Security:** Interface agreements documenting responsibilities, capability assessment of suppliers, work products (suppliers provide evidence, OEM validates), security flows through entire Tier 1/2/3 supplier chain

---

## Slide 42: Summary - Multi-Layered IoV Security
**Conclusion**

- **IoV Transformation:** Vehicles evolved from isolated machines to networked computing platforms generating terabytes of data daily; this creates unprecedented safety benefits and unprecedented security risks
- **Core Modules Covered:**
  - **Module 1:** Architecture (OBU, RSU, Cloud/Fog/Edge, V2X)
  - **Module 2:** Characteristics and Challenges (mobility, latency, constraints)
  - **Module 3:** Routing Protocols (VANET, AODV, GPSR, secure routing)
  - **Module 4:** Attack Taxonomy (network, identity, physical attacks)
  - **Module 5:** Authentication and Privacy (pseudonyms, mix-zones)
  - **Module 6:** Cryptography and Key Management (ECC, SCMS, PKI)
  - **Module 7:** Intrusion Detection Systems (signature, anomaly, ML-based)
- **Defense Strategy:** Lightweight cryptography, robust PKI/SCMS, pseudonymous privacy, intrusion/misbehavior detection, sensor fusion verification, secure VANET routing protocols

---

## Slide 43: Future Outlook and Key Takeaways
**Conclusion**

- **Emerging Challenges:** 5G/6G connectivity expanding attack surface, fully autonomous Level 4/5 vehicles with AI-centric architectures, quantum computing threatening current cryptography, AI-specific regulations, increasing V2X deployment scale
- **Critical Success Factors:** Defense-in-depth (no single control protects everything), crypto-agility (prepare for post-quantum migration), privacy-by-design (balance accountability and anonymity), continuous monitoring (threats evolve constantly), supply chain security (vulnerabilities cascade)
- **Key Takeaway 1:** IoV cybersecurity is fundamentally a public safety issue—not just IT security; cyberattacks have direct physical consequences including injury and death
- **Key Takeaway 2:** Multi-layered security is mandatory—cryptography alone isn't enough; IDS, misbehavior detection, sensor fusion, trust management, and physical security all required
- **Key Takeaway 3:** Regulatory compliance is now legally required—UNECE WP.29, ISO 21434, GDPR create binding obligations; non-compliant vehicles cannot be sold in major markets

---

## Slide 44: Q&A Session
**Conclusion**

- **Core Topics Covered:** IoV architecture and evolution (Module 1), characteristics and challenges (Module 2), routing protocols and secure routing (Module 3), attack taxonomy (Module 4), authentication and privacy (Module 5), cryptography and key management (Module 6), intrusion detection systems (Module 7)
- **Additional Topics:** V2X vulnerabilities, blockchain applications, adversarial AI threats, global regulatory requirements
- **Further Exploration:** Module deep-dives available for each topic area; practical exercises include V2X vulnerability assessment, cryptographic performance analysis, misbehavior detection simulation, VANET attack simulation
- **Industry Resources:** Auto-ISAC for threat intelligence sharing, MOBI consortium for blockchain exploration, IEEE 1609 and 3GPP standards documentation, NHTSA best practices guidance
- **Questions Welcome!**

---

**Thank you for your attention!**

**لنؤمّن مستقبل التنقل المتصل معاً**

**Let's secure the future of connected mobility together.**
