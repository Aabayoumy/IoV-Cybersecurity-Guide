# Module 3: Types of Routing Protocols and Secure Routing

## Overview

Vehicular Ad-Hoc Networks (VANETs) form the wireless communication backbone of the Internet of Vehicles. Unlike traditional infrastructure-based networks, VANETs are self-organizing, decentralized networks where vehicles communicate directly with each other without relying on fixed infrastructure. This unique architecture introduces specific security challenges that require specialized solutions. This module provides comprehensive coverage of VANET fundamentals, types of routing protocols, secure routing protocols, and defense mechanisms.

---

## 3.1 VANET Fundamentals

### 3.1.1 What is a VANET?

**Definition:**
A Vehicular Ad-Hoc Network (VANET) is a subclass of Mobile Ad-Hoc Networks (MANETs) specifically designed for vehicle-to-vehicle communication, where vehicles act as mobile nodes that can join and leave the network dynamically.

**Key Characteristics:**

| Characteristic | Description | Security Implication |
|---------------|-------------|---------------------|
| **Self-Organizing** | No central authority required for network formation | Trust establishment is challenging |
| **Decentralized** | Each node can route messages | Any node can become malicious router |
| **High Mobility** | Nodes move at 30-150 km/h | Rapid topology changes |
| **Dynamic Topology** | Network structure changes every second | Routing tables quickly obsolete |
| **Intermittent Connectivity** | Connections last seconds | Multi-hop paths unstable |
| **No Fixed Infrastructure** | Pure V2V without RSUs possible | Cannot rely on trusted anchors |

### 3.1.2 VANET vs. Traditional Networks

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    TRADITIONAL NETWORK                                       │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│      ┌────────┐          ┌────────┐          ┌────────┐                     │
│      │ Client │ ──────── │ Router │ ──────── │ Server │                     │
│      └────────┘          └────────┘          └────────┘                     │
│                                                                              │
│  • Fixed topology                    • Stable connections                    │
│  • Trusted infrastructure            • Centralized routing                   │
│  • Static security policies          • Known network boundaries              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                         VANET                                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│      ┌───┐         ┌───┐         ┌───┐         ┌───┐                        │
│      │ V │◄───────►│ V │◄───────►│ V │◄───────►│ V │                        │
│      └───┘         └─┬─┘         └─┬─┘         └───┘                        │
│        │             │             │             │                           │
│        │    ┌───┐    │    ┌───┐    │                                        │
│        └───►│ V │◄───┴───►│ V │◄───┘                                        │
│             └───┘         └───┘                                              │
│                                                                              │
│  • Dynamic topology (changes every second)                                   │
│  • No central authority (decentralized)                                      │
│  • Every node is a router (peer-to-peer)                                    │
│  • Trust must be established on-the-fly                                      │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 3.1.3 VANET Communication Modes

**Single-Hop Communication:**
```
┌─────────┐                    ┌─────────┐
│Vehicle A│ ══════════════════►│Vehicle B│
└─────────┘   Direct Range     └─────────┘
              (300-1000m)
```
- Direct communication within radio range
- Lowest latency (~1-5 ms)
- Most reliable
- Limited range

**Multi-Hop Communication:**
```
┌─────────┐         ┌─────────┐         ┌─────────┐         ┌─────────┐
│Vehicle A│────────►│Vehicle B│────────►│Vehicle C│────────►│Vehicle D│
└─────────┘  Hop 1  └─────────┘  Hop 2  └─────────┘  Hop 3  └─────────┘
   Source           Forwarder           Forwarder           Destination
```
- Extends range beyond single transmission
- Each hop adds latency
- Path may break if any intermediate vehicle moves
- Requires routing protocol

**Broadcast Communication:**
```
                    ┌─────────┐
                    │Vehicle B│
                    └─────────┘
                         ▲
                         │
┌─────────┐         ┌────┴────┐         ┌─────────┐
│Vehicle A│◄────────│Vehicle S│────────►│Vehicle C│
└─────────┘         │(Source) │         └─────────┘
                    └────┬────┘
                         │
                         ▼
                    ┌─────────┐
                    │Vehicle D│
                    └─────────┘
```
- One-to-all communication
- Used for safety messages (BSM)
- Broadcast storm problem in dense networks
- No acknowledgment (fire and forget)

### 3.1.4 VANET Protocol Stack

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        APPLICATION LAYER                                     │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│  │   Safety    │ │   Traffic   │ │   Comfort   │ │  Commercial │            │
│  │ (Collision  │ │(Congestion  │ │(Infotain-   │ │   (Tolling, │            │
│  │  Warning)   │ │  Reports)   │ │   ment)     │ │  Parking)   │            │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘            │
├─────────────────────────────────────────────────────────────────────────────┤
│                        TRANSPORT LAYER                                       │
│  ┌─────────────────────────────┐ ┌─────────────────────────────┐            │
│  │           UDP               │ │           TCP               │            │
│  │   (Safety - low latency)    │ │   (Reliable - file transfer)│            │
│  └─────────────────────────────┘ └─────────────────────────────┘            │
├─────────────────────────────────────────────────────────────────────────────┤
│                        NETWORK LAYER                                         │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│  │  GeoNet     │ │    AODV     │ │    DSR      │ │    GPSR     │            │
│  │ (Geographic)│ │ (Reactive)  │ │(Source Rte) │ │ (Greedy)    │            │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘            │
├─────────────────────────────────────────────────────────────────────────────┤
│                      LLC / MAC LAYER                                         │
│  ┌─────────────────────────────┐ ┌─────────────────────────────┐            │
│  │       IEEE 802.11p          │ │         C-V2X PC5           │            │
│  │      (WAVE/DSRC MAC)        │ │     (Sidelink MAC)          │            │
│  └─────────────────────────────┘ └─────────────────────────────┘            │
├─────────────────────────────────────────────────────────────────────────────┤
│                       PHYSICAL LAYER                                         │
│  ┌─────────────────────────────┐ ┌─────────────────────────────┐            │
│  │     5.9 GHz DSRC/ITS        │ │    5.9 GHz C-V2X            │            │
│  │      (OFDM, 10 MHz)         │ │    (SC-FDMA)                │            │
│  └─────────────────────────────┘ └─────────────────────────────┘            │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 3.2 VANET Routing Protocols

### 3.2.1 Routing Classification

**Topology-Based Routing:**

*Proactive (Table-Driven):*
- Maintain routing tables continuously
- Routes available immediately
- High overhead in dynamic VANETs
- Examples: OLSR, DSDV

*Reactive (On-Demand):*
- Discover routes only when needed
- Lower overhead
- Initial latency for route discovery
- Examples: AODV, DSR

**Position-Based (Geographic) Routing:**
- Use GPS coordinates for routing decisions
- Forward to neighbor closest to destination
- No routing tables needed
- Examples: GPSR, GSR, GPCR

**Cluster-Based Routing:**
- Group vehicles into clusters
- Cluster heads handle inter-cluster routing
- Reduces overhead in dense networks
- Examples: CBLR, LORA-CBF

### 3.2.2 Key Routing Protocols

**AODV (Ad-hoc On-demand Distance Vector):**

```
Route Discovery Process:
                                                          
  Source                                              Destination
    │                                                      │
    │──── RREQ (Route Request) ────────────────────────────►
    │         │           │           │                    │
    │    ┌────▼────┐ ┌────▼────┐ ┌────▼────┐              │
    │    │ Node A  │ │ Node B  │ │ Node C  │              │
    │    └────┬────┘ └────┬────┘ └────┬────┘              │
    │         │           │           │                    │
    │◄──── RREP (Route Reply) ─────────────────────────────│
    │                                                      │
    │═══════════════ DATA PATH ════════════════════════════│
```

**Protocol Details:**

| Field | Purpose | Security Relevance |
|-------|---------|-------------------|
| Source Seq# | Freshness of source info | Can be forged |
| Dest Seq# | Freshness of destination info | Can be manipulated |
| Hop Count | Distance metric | Can be falsified |
| Lifetime | Route validity period | Can be extended maliciously |

**AODV Vulnerabilities:**
1. No authentication of RREQ/RREP messages
2. Sequence numbers can be forged
3. Hop count can be manipulated
4. Route replies can be spoofed

**GPSR (Greedy Perimeter Stateless Routing):**

```
Greedy Forwarding Mode:

     Destination ★
          ▲
          │
     ┌────┴────┐
     │ Node C  │ ◄── Selected (closest to destination)
     └─────────┘
          ▲
          │
┌─────────┐  ┌─────────┐  ┌─────────┐
│ Node A  │  │ Source  │  │ Node B  │
└─────────┘  └────┬────┘  └─────────┘
                  │
        Decision: Forward to C
        (C is closest to destination)
```

**Perimeter Mode (when greedy fails):**
- Activated when no neighbor is closer to destination
- Uses right-hand rule to route around void
- Returns to greedy when possible

**GPSR Vulnerabilities:**
1. Relies entirely on position information
2. Position can be spoofed
3. No verification of claimed location
4. Attacker can attract traffic with false position

### 3.2.3 Routing Protocol Security Comparison

| Protocol | Type | Primary Vulnerability | Attack Impact |
|----------|------|----------------------|---------------|
| AODV | Reactive | Sequence number manipulation | Route hijacking |
| DSR | Source Routing | Route cache poisoning | Traffic interception |
| OLSR | Proactive | MPR selection manipulation | Network isolation |
| GPSR | Geographic | Position falsification | Traffic misdirection |
| DSDV | Proactive | Routing table poisoning | Network-wide disruption |

---

## 3.3 Security Threats to VANETs

### 3.3.1 Threat Classification by Layer

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    VANET THREAT LANDSCAPE                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  APPLICATION LAYER THREATS                                                   │
│  ├── False Information Dissemination                                        │
│  ├── Message Falsification                                                  │
│  └── Malicious Application Behavior                                         │
│                                                                              │
│  NETWORK LAYER THREATS                                                       │
│  ├── Blackhole Attack                                                       │
│  ├── Grayhole Attack                                                        │
│  ├── Wormhole Attack                                                        │
│  ├── Sinkhole Attack                                                        │
│  ├── Sybil Attack                                                           │
│  └── Routing Table Overflow                                                 │
│                                                                              │
│  MAC LAYER THREATS                                                          │
│  ├── MAC Spoofing                                                           │
│  ├── Channel Jamming                                                        │
│  └── Priority Manipulation                                                  │
│                                                                              │
│  PHYSICAL LAYER THREATS                                                      │
│  ├── RF Jamming                                                             │
│  ├── Eavesdropping                                                          │
│  └── Signal Injection                                                       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 3.3.2 Detailed Attack Analysis

**Blackhole Attack:**

```
Normal Routing:
┌───┐         ┌───┐         ┌───┐         ┌───┐
│ S │────────►│ A │────────►│ B │────────►│ D │
└───┘         └───┘         └───┘         └───┘
Source       Forward       Forward       Destination

Blackhole Attack:
┌───┐         ┌───┐         ┌───┐
│ S │────────►│ M │    ✗    │ D │
└───┘         └───┘         └───┘
Source      Malicious     Destination
            (Drops all     (Never receives)
             packets)
```

**Attack Mechanism:**
1. Malicious node M responds to RREQ with low hop count
2. Source S believes M has best route to D
3. S routes all traffic through M
4. M drops all packets (creates "black hole")

**Impact:**
- Safety messages never reach destination
- Emergency warnings disappear
- Network partitioning occurs

**Grayhole Attack (Selective Dropping):**

```
┌───┐         ┌───┐         ┌───┐
│ S │────────►│ M │────────►│ D │
└───┘         └───┘         └───┘
              │
              │ Selective Drop:
              │ • Drop safety messages ✗
              │ • Forward entertainment ✓
              │ • Drop every Nth packet ✗
```

**Why Grayhole is More Dangerous:**
- Harder to detect than blackhole
- Can selectively target specific message types
- May appear as network congestion
- Malicious intent disguised as performance issue

**Wormhole Attack:**

```
                    Legitimate Network View
          ┌─────────────────────────────────────────┐
          │                                         │
     ┌────┴────┐                           ┌────────┴───┐
     │  Node A  │                           │   Node B   │
     │(Location │                           │ (Location  │
     │  West)   │                           │   East)    │
     └────┬────┘                           └─────┬──────┘
          │                                      │
          │         "Tunnel" Connection          │
          │◄────────────────────────────────────►│
          │    (Private high-speed link)         │
          │                                      │
          
Actual Attack:
     ┌─────────┐                           ┌─────────┐
     │Attacker │══════════════════════════│Attacker │
     │   M1    │    Out-of-Band Tunnel    │   M2    │
     │(West)   │      (Internet/4G)       │ (East)  │
     └─────────┘                           └─────────┘
```

**Wormhole Attack Effects:**
1. M1 captures packets in West region
2. Instantly tunnels to M2 in East region
3. M2 replays packets in East
4. Network routing believes West and East are adjacent
5. All traffic routes through wormhole
6. Attackers can intercept, modify, or drop traffic

**Sybil Attack:**

```
Single Physical Vehicle:
     ┌─────────────────────────────────────┐
     │                                     │
     │    Malicious Vehicle M              │
     │                                     │
     │   Creates Multiple Identities:      │
     │   ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐  │
     │   │ ID1 │ │ ID2 │ │ ID3 │ │ IDn │  │
     │   └─────┘ └─────┘ └─────┘ └─────┘  │
     │                                     │
     └─────────────────────────────────────┘

Network View (False):
     ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
     │ V1  │ │ V2  │ │ V3  │ │ V4  │ │ V5  │
     └─────┘ └─────┘ └─────┘ └─────┘ └─────┘
         ▲       ▲       ▲       ▲       ▲
         └───────┴───────┴───────┴───────┘
                   All same attacker!
```

**Sybil Attack Impacts:**
- Voting manipulation (fake congestion reports)
- Resource exhaustion (fake route requests)
- Reputation gaming (fake positive reviews)
- Traffic manipulation (fake vehicle presence)

**Position Falsification Attack:**

```
Actual Position:      Claimed Position:
                      
     ┌───┐                 ┌───┐
     │ M │                 │ M │ (Ghost)
     └───┘                 └───┘
       │                     │
       │                     │
  ─────┼─────           ─────┼─────
       │ Intersection        │
       │                     │
       │                 ┌───┐
       │                 │ V │ (Victim may brake
       │                 └───┘  for "ghost" vehicle)
```

**Attack Mechanism:**
1. Malicious vehicle M claims to be at intersection
2. Actually located elsewhere
3. Victim V receives collision warning
4. V brakes unnecessarily
5. May cause real accident with following vehicles

---

## 3.4 Secure Routing Protocols

### 3.4.1 Security Requirements for VANET Routing

| Requirement | Description | Implementation Challenge |
|-------------|-------------|-------------------------|
| **Authentication** | Verify sender identity | Pseudonym management |
| **Integrity** | Detect message modification | Signature overhead |
| **Non-repudiation** | Cannot deny sending | Key management |
| **Availability** | Resist DoS attacks | Resource constraints |
| **Confidentiality** | Protect sensitive data | Key distribution |
| **Privacy** | Protect location/identity | Anonymity vs. accountability |

### 3.4.2 SAODV (Secure AODV)

**Overview:**
SAODV adds digital signatures and hash chains to AODV to prevent routing message forgery.

**Security Mechanisms:**

```
SAODV Route Request:
┌─────────────────────────────────────────────────────────────────────────────┐
│                         SAODV RREQ Message                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  Standard AODV Fields:                                                       │
│  ├── Source Address                                                         │
│  ├── Destination Address                                                    │
│  ├── Source Sequence Number                                                 │
│  ├── Destination Sequence Number                                            │
│  └── Hop Count                                                              │
├─────────────────────────────────────────────────────────────────────────────┤
│  SAODV Security Extensions:                                                  │
│  ├── Digital Signature (covers immutable fields)                            │
│  │   └── Signed by source with private key                                 │
│  └── Hash Chain (for hop count)                                             │
│      └── h^(max_hops)(seed) → allows hop count increment verification      │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Hash Chain for Hop Count Protection:**

```
Initial (Source):
Hash Chain: h³(seed) = h(h(h(seed)))
Hop Count: 0

After Hop 1:
Hash Chain: h²(seed) = h(h(seed))
Hop Count: 1

After Hop 2:
Hash Chain: h¹(seed) = h(seed)
Hop Count: 2

Verification:
Recipient can verify: h(received_hash) == previous_hash
```

**SAODV Limitations:**
- Signature computation overhead
- Doesn't prevent wormhole attacks
- Key distribution challenges
- Doesn't address insider threats

### 3.4.3 ARAN (Authenticated Routing for Ad-hoc Networks)

**Overview:**
ARAN uses certificates and end-to-end authentication for all routing messages.

**Architecture:**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         ARAN Architecture                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌────────────────────┐                                                     │
│  │  Trusted Server    │                                                     │
│  │  (Certificate      │                                                     │
│  │   Authority)       │                                                     │
│  └─────────┬──────────┘                                                     │
│            │ Issues Certificates                                             │
│            │                                                                 │
│  ┌─────────▼──────────┐                                                     │
│  │                    │                                                     │
│  │  ┌───┐ ┌───┐ ┌───┐ │                                                     │
│  │  │V1 │ │V2 │ │V3 │ │  All vehicles have certificates                    │
│  │  │+C │ │+C │ │+C │ │  from trusted CA                                   │
│  │  └───┘ └───┘ └───┘ │                                                     │
│  │                    │                                                     │
│  └────────────────────┘                                                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

**ARAN Route Discovery:**

1. **RDP (Route Discovery Packet):**
   - Contains source certificate
   - Signed by source
   - Each forwarder appends signature

2. **REP (Reply Packet):**
   - Contains destination certificate
   - Signed by destination
   - Returns along discovered path

**Security Properties:**
- End-to-end authentication
- Non-repudiation
- Prevents impersonation
- Detects message modification

### 3.4.4 SEAD (Secure Efficient Ad-hoc Distance Vector)

**Overview:**
SEAD secures DSDV using hash chains for efficiency.

**Key Innovation:**
Uses one-way hash chains instead of digital signatures for better performance.

```
Hash Chain Authentication:

Node A establishes chain:
h⁰ = random seed (secret)
h¹ = H(h⁰)
h² = H(h¹) 
...
hⁿ = H(hⁿ⁻¹)    ← This is published (authentication anchor)

To authenticate metric k:
Reveal hⁿ⁻ᵏ
Verifier computes H^k(hⁿ⁻ᵏ) and checks if equals hⁿ
```

**Performance Advantage:**
- Hash computation: ~1000x faster than signature
- Verification: ~10000x faster than signature verification
- Suitable for resource-constrained VANETs

### 3.4.5 Position-Based Secure Routing

**PLV (Position-Linked Verification):**

```
Position Verification Process:

1. Vehicle claims position P(x,y)
2. Nearby vehicles V1, V2, V3 receive claim
3. Each verifier checks:
   - Is P reachable given signal strength?
   - Does P match direction of arrival?
   - Is P consistent with previous positions?
4. Verifiers vote on position validity
5. Majority determines acceptance
```

**Challenges:**
- Requires multiple witnesses
- May not work in sparse networks
- Colluding attackers can fool verification

---

## 3.5 Defense Mechanisms

### 3.5.1 Cryptographic Solutions

**Digital Signatures:**

| Algorithm | Key Size | Sign Time | Verify Time | Suitable for VANET? |
|-----------|----------|-----------|-------------|---------------------|
| RSA-2048 | 2048 bits | 0.5 ms | 0.02 ms | No (key size) |
| ECDSA-256 | 256 bits | 0.1 ms | 0.3 ms | Yes |
| EdDSA | 256 bits | 0.05 ms | 0.15 ms | Yes |

**Message Authentication Codes (MACs):**
- Faster than signatures
- Requires shared secret
- Suitable for established groups (platoons)

**Hybrid Approaches:**
- Use signatures for route establishment
- Use MACs for data forwarding
- Balance security and performance

### 3.5.2 Trust Management Systems

**Trust-Based Routing:**

```
Trust Score Calculation:

Trust(V) = α × DirectTrust + β × IndirectTrust + γ × RecommendedTrust

Where:
- DirectTrust: Based on own observations
- IndirectTrust: Based on network feedback
- RecommendedTrust: Based on trusted third parties
- α + β + γ = 1
```

**Trust Update Mechanism:**

```
After successful interaction:
Trust(V) = Trust(V) + reward

After failed interaction:
Trust(V) = Trust(V) - penalty

Trust Decay over time:
Trust(V) = Trust(V) × decay_factor
```

**Trust-Based Forwarding Decision:**

```
Select_Next_Hop(Destination D):
    candidates = neighbors closer to D
    for each candidate C:
        score[C] = proximity_to_D × Trust(C)
    return candidate with highest score
```

### 3.5.3 Intrusion Detection for VANETs

**Watchdog Mechanism:**

```
Watchdog Operation:

┌───────┐         ┌───────┐         ┌───────┐
│   A   │────────►│   B   │────────►│   C   │
└───────┘         └───────┘         └───────┘
    │                                    
    │ A sends packet to B for forwarding
    │ A continues to monitor B's transmissions
    │
    └──►  If B forwards within timeout: OK
          If B doesn't forward: Mark B as suspect
```

**Pathrater Mechanism:**
- Works with watchdog
- Rates paths based on node reliability
- Avoids paths with suspected malicious nodes

**Specification-Based Detection:**

```
VANET Behavioral Specifications:

Rule 1: Speed Consistency
IF |claimed_speed - calculated_speed| > threshold
THEN flag as suspicious

Rule 2: Position Consistency  
IF distance(pos_t, pos_t-1) > max_possible_distance
THEN flag as suspicious

Rule 3: Message Rate Limits
IF message_rate > max_allowed_rate
THEN flag as DoS attack

Rule 4: Geographic Consistency
IF claimed_position NOT on_road_network
THEN flag as suspicious
```

### 3.5.4 Wormhole Detection

**Packet Leashes:**

*Geographic Leash:*
```
Sender includes:
- Own GPS position
- Send timestamp

Receiver verifies:
- Distance = speed_of_light × time_difference
- If claimed_distance > calculated_max_distance
- Then: Wormhole suspected
```

*Temporal Leash:*
```
Sender includes:
- Precise send timestamp (GPS synchronized)

Receiver verifies:
- Current_time - send_time < max_allowed_delay
- If exceeded, packet may have been tunneled
```

**Challenges:**
- Requires tight time synchronization
- GPS can be spoofed
- May have false positives

### 3.5.5 Sybil Attack Prevention

**Resource Testing:**
```
Challenge-Response:
1. Send computational puzzle
2. Require response within time limit
3. Single physical device can only solve one puzzle
4. Multiple identities would fail to respond in time
```

**Position Verification:**
```
1. RSU challenges vehicle to prove position
2. Vehicle must respond within time proportional to distance
3. Sybil identities at different claimed positions
   cannot all respond correctly
```

**Certificate-Based Prevention:**
```
1. Each vehicle gets limited number of pseudonyms from CA
2. CA tracks total pseudonyms issued
3. Cannot create unlimited identities
4. Privacy preserved through pseudonyms
```

---

## 3.6 Privacy in VANETs

### 3.6.1 The Privacy Challenge

**What's at Risk:**
- Real-time location tracking
- Travel pattern analysis
- Home/work location inference
- Social relationship mapping
- Commercial profiling

**Privacy Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Anonymity** | Identity not revealed |
| **Unlinkability** | Cannot link messages to same sender |
| **Unobservability** | Cannot detect presence |
| **Pseudonymity** | Use temporary identifiers |

### 3.6.2 Pseudonym Schemes

**Basic Pseudonym Rotation:**

```
Time     Pseudonym    Messages
─────────────────────────────────
T0-T5    PS1          M1, M2, M3
T5-T10   PS2          M4, M5, M6
T10-T15  PS3          M7, M8, M9
```

**Silent Period Strategy:**

```
┌─────────┐    Silent     ┌─────────┐
│   PS1   │───Period────►│   PS2   │
│transmit │   (no TX)     │transmit │
└─────────┘               └─────────┘
     T1        T2            T3

During silent period:
- No messages transmitted
- Change pseudonym
- Resume with new identity
```

**Mix-Zone Integration:**

```
                    MIX ZONE
              ┌─────────────────┐
              │                 │
Vehicle A ───►│  ●  ●  ●  ●    │───► Vehicle ?
(PS_A)        │     (mixing)   │     (PS_X)
              │                 │
Vehicle B ───►│  ●  ●  ●  ●    │───► Vehicle ?
(PS_B)        │                │     (PS_Y)
              │                 │
Vehicle C ───►│  ●  ●  ●  ●    │───► Vehicle ?
(PS_C)        │                │     (PS_Z)
              └─────────────────┘
```

### 3.6.3 Group Signatures for VANETs

**Concept:**
- Vehicle signs as "member of group" not as individual
- Verifier confirms valid group member
- Cannot identify which member signed
- Authority can "open" signature in emergency

**Process:**
```
1. Setup: Group manager creates group
2. Join: Vehicle receives group signing key
3. Sign: Vehicle signs with group key
4. Verify: Anyone verifies group membership
5. Open: (Emergency) Authority reveals signer
```

**Advantages:**
- Strong privacy for normal operation
- Accountability preserved for emergencies
- No pseudonym pool management

---

## 3.7 Implementation Considerations

### 3.7.1 Performance Requirements

**Latency Budget:**

| Operation | Max Latency | Notes |
|-----------|-------------|-------|
| Signature generation | 1 ms | Per outgoing message |
| Signature verification | 2 ms | Per incoming message |
| Route decision | 5 ms | Including security checks |
| Total processing | 10 ms | Safety message deadline |

**Throughput Requirements:**

| Scenario | Messages/sec | Processing Load |
|----------|-------------|-----------------|
| Light traffic | 100 | Low |
| Normal traffic | 500 | Medium |
| Dense traffic | 1000+ | High |
| Emergency | 2000+ | Critical |

### 3.7.2 Hardware Security

**Recommended Security Hardware:**

1. **Hardware Security Module (HSM):**
   - Secure key storage
   - Cryptographic acceleration
   - Tamper resistance
   - Examples: Infineon SLS 32, NXP i.MX

2. **Trusted Platform Module (TPM):**
   - Secure boot verification
   - Platform integrity
   - Key management

3. **GPS Anti-Spoofing:**
   - Multi-constellation receivers
   - Authenticated signals (future)
   - INS integration

### 3.7.3 Deployment Considerations

**Incremental Deployment:**

```
Phase 1: Safety infrastructure
├── Deploy RSUs at critical intersections
├── V2I for signal timing
└── Basic authentication

Phase 2: V2V enablement
├── OBU deployment in new vehicles
├── Aftermarket OBU availability
└── Full authentication + privacy

Phase 3: Advanced security
├── Trust management systems
├── Intrusion detection
└── Blockchain integration
```

---

## 3.8 Summary

Vehicular Ad-Hoc Networks form the communication foundation of IoV, enabling vehicles to communicate directly without infrastructure dependency. The self-organizing, highly mobile nature of VANETs creates unique security challenges not found in traditional networks. Routing protocols designed for VANETs must balance performance with security, using techniques like digital signatures, hash chains, and trust management. Defense mechanisms including watchdog systems, intrusion detection, and position verification help identify and isolate malicious nodes. Privacy-preserving techniques such as pseudonyms and group signatures protect driver location and identity while maintaining accountability.

**Key Takeaways:**

1. VANETs are fundamentally different from traditional networks—security solutions must account for high mobility and dynamic topology
2. Standard routing protocols (AODV, DSR, GPSR) are vulnerable to numerous attacks
3. Secure routing protocols add authentication but introduce performance overhead
4. Trust management enables informed routing decisions without central authority
5. Privacy and security must be balanced—pseudonyms enable both
6. Hardware security (HSM, TPM) is essential for key protection
7. Defense-in-depth combining multiple mechanisms provides best protection

---

## Review Questions

1. **Q:** Why is secure routing in VANETs more challenging than in traditional networks?
   **A:** VANET routing is difficult because the network changes constantly as vehicles move, so routes are short-lived and trust decisions must be made quickly. Traditional routing protocols often assume relatively stable topologies and cooperative nodes, but VANETs can include malicious or compromised participants that advertise fake routes, falsify position data, or drop forwarded packets. Security mechanisms like signatures and route validation are needed, but they add processing and communication overhead. The core challenge is balancing strong protection with strict timing requirements for safety applications where delays can reduce effectiveness.

---

## Practical Exercise

**VANET Security Simulation:**

Using NS-3 or Veins simulator:

1. **Setup:**
   - Create VANET scenario with 50 vehicles
   - Implement AODV routing
   - Configure realistic mobility (SUMO)

2. **Attack Implementation:**
   - Implement blackhole attack (drop all packets)
   - Implement grayhole attack (selective dropping)
   - Implement Sybil attack (multiple fake identities)

3. **Measurement:**
   - Packet delivery ratio (normal vs. attack)
   - End-to-end latency
   - Network throughput

4. **Defense Implementation:**
   - Add watchdog mechanism
   - Implement basic trust scoring
   - Measure detection accuracy

5. **Analysis:**
   - Compare attack effectiveness
   - Evaluate defense performance
   - Identify remaining vulnerabilities

---

[Back to Index](../README.md) | [Previous Module: References and Resources](11-references.md)
