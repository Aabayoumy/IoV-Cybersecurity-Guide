# Module 1: Architecture and Nodes

---

## Table of Contents

1. [Introduction](#introduction)
2. [Foundational Architecture Overview](#foundational-architecture-overview)
3. [Key Network Nodes](#key-network-nodes)
4. [Computing Layers in IoV](#computing-layers-in-iov)
5. [Communication Paradigms (V2X)](#communication-paradigms-v2x)
6. [Communication Technologies](#communication-technologies)
7. [Security Implications of Architecture](#security-implications-of-architecture)
8. [Key Takeaways](#key-takeaways)

---

## Introduction

Understanding the architecture of the Internet of Vehicles is fundamental to grasping its security challenges. Unlike traditional computer networks with relatively static topologies and well-defined perimeters, IoV represents a **heterogeneous, highly dynamic, and distributed system** that spans multiple communication technologies, computing paradigms, and trust domains.

This module provides a comprehensive examination of the IoV architecture, including the key nodes (vehicles, infrastructure, cloud), the computing layers (edge, fog, cloud), and the communication paradigms (V2V, V2I, V2P, V2N) that together form the Vehicle-to-Everything (V2X) ecosystem.

---

## Foundational Architecture Overview

### The Layered Model

IoV architecture can be conceptualized as a multi-layered system:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          APPLICATION LAYER                                   │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐│
│  │   Safety    │ │  Traffic    │ │Infotainment │ │    Fleet Management     ││
│  │Applications │ │ Management  │ │  Services   │ │       & Logistics       ││
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────────────────┘│
├─────────────────────────────────────────────────────────────────────────────┤
│                          NETWORK/TRANSPORT LAYER                             │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐│
│  │    IPv6     │ │   GeoNet    │ │    TCP/     │ │    Security Protocols   ││
│  │  Addressing │ │  Routing    │ │    UDP      │ │    (IEEE 1609.2)        ││
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────────────────┘│
├─────────────────────────────────────────────────────────────────────────────┤
│                          ACCESS LAYER                                        │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐│
│  │  IEEE       │ │   C-V2X     │ │    5G NR    │ │      Satellite          ││
│  │  802.11p    │ │  (PC5/Uu)   │ │             │ │                         ││
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────────────────┘│
├─────────────────────────────────────────────────────────────────────────────┤
│                          PHYSICAL LAYER                                      │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐│
│  │   5.9 GHz   │ │   Sub-6 GHz │ │   mmWave    │ │    Wired (Ethernet)     ││
│  │   ITS Band  │ │   Cellular  │ │             │ │                         ││
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

### Architectural Domains

The IoV ecosystem spans three interconnected domains:

| Domain | Description | Key Components | Trust Level |
|--------|-------------|----------------|-------------|
| **In-Vehicle Domain** | Internal vehicle systems | ECUs, CAN bus, gateways | High (physically secured) |
| **Ad-Hoc Domain** | Direct vehicle-to-vehicle/infrastructure | OBU, RSU, DSRC/C-V2X | Medium (authenticated peers) |
| **Infrastructure Domain** | Backend systems and cloud | Traffic centers, PKI, OEM clouds | Variable (service-dependent) |

### Reference Architecture Standards

Several standardization bodies have defined IoV reference architectures:

- **ETSI ITS Architecture** (European)
- **SAE J2945 Series** (North American)
- **ISO 21217** (International)
- **3GPP V2X Architecture** (Cellular)

While implementations vary by region and manufacturer, the core concepts remain consistent across these standards.

---

## Key Network Nodes

### On-Board Unit (OBU)

The **On-Board Unit (OBU)** is the computing and communication hub installed within each connected vehicle. It serves as the vehicle's interface to the external IoV network.

#### OBU Components

```
┌─────────────────────────────────────────────────────────────────────┐
│                         ON-BOARD UNIT (OBU)                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────────┐    ┌──────────────────┐    ┌────────────────┐ │
│  │   COMMUNICATION  │    │    PROCESSING    │    │    SECURITY    │ │
│  │      MODULE      │    │      MODULE      │    │     MODULE     │ │
│  ├──────────────────┤    ├──────────────────┤    ├────────────────┤ │
│  │ • DSRC Radio     │    │ • Application    │    │ • HSM (Hardware│ │
│  │ • C-V2X Modem    │    │   Processor      │    │   Security     │ │
│  │ • 5G Module      │    │ • GPS Receiver   │    │   Module)      │ │
│  │ • Wi-Fi/BT       │    │ • Sensor Fusion  │    │ • Secure Boot  │ │
│  │ • Satellite      │    │ • Message Queue  │    │ • Key Storage  │ │
│  └──────────────────┘    └──────────────────┘    └────────────────┘ │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────────┤
│  │                    VEHICLE INTERFACE                              │
│  ├──────────────────────────────────────────────────────────────────┤
│  │ • CAN Bus Gateway    • Ethernet Switch    • Diagnostic Port     │
│  └──────────────────────────────────────────────────────────────────┘
└─────────────────────────────────────────────────────────────────────┘
```

#### OBU Functions

| Function | Description | Security Relevance |
|----------|-------------|-------------------|
| **Message Generation** | Creates Basic Safety Messages (BSMs) with position, speed, heading | Must be accurate and authenticated |
| **Message Reception** | Receives and processes messages from other vehicles/RSUs | Must validate signatures |
| **Sensor Integration** | Fuses data from GPS, cameras, radar, LiDAR | Sensors can be spoofed |
| **Gateway Function** | Bridges external communications to internal vehicle networks | Critical isolation point |
| **Cryptographic Operations** | Signs outgoing messages, verifies incoming signatures | Performance-critical |
| **Pseudonym Management** | Rotates temporary identities for privacy | Timing affects linkability |

#### OBU Security Requirements

1. **Hardware Security Module (HSM):** Secure storage for private keys
2. **Secure Boot:** Verification of firmware integrity at startup
3. **Isolation:** Separation between safety-critical and infotainment functions
4. **Tamper Resistance:** Physical protection against extraction attacks
5. **Update Capability:** Secure Over-The-Air (OTA) update support

### Roadside Unit (RSU)

The **Roadside Unit (RSU)** is a stationary communication node deployed along roadways, at intersections, and at key infrastructure points.

#### RSU Deployment Locations

| Location | Primary Function | Example Use Cases |
|----------|-----------------|-------------------|
| **Intersections** | Signal Phase and Timing (SPaT) | Red light warnings, green wave optimization |
| **Highway On-Ramps** | Merge assistance | Speed recommendations, gap finding |
| **Work Zones** | Hazard warnings | Construction alerts, lane closures |
| **Toll Plazas** | Electronic tolling | Seamless payment, congestion pricing |
| **Parking Structures** | Availability info | Space finding, pricing |
| **Emergency Scenes** | First responder alerts | Accident warnings, detour information |

#### RSU Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         ROADSIDE UNIT (RSU)                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────────┐    ┌──────────────────┐    ┌────────────────┐ │
│  │   V2X RADIO      │    │    BACKHAUL      │    │   EDGE COMPUTE │ │
│  │                  │    │                  │    │                │ │
│  │ • DSRC 5.9 GHz   │    │ • Fiber Optic    │    │ • Local Cache  │ │
│  │ • C-V2X PC5      │    │ • LTE Backhaul   │    │ • Map Database │ │
│  │ • Multi-channel  │    │ • Microwave      │    │ • ML Inference │ │
│  └──────────────────┘    └──────────────────┘    └────────────────┘ │
│                                                                      │
│  ┌──────────────────┐    ┌──────────────────┐    ┌────────────────┐ │
│  │   TRAFFIC        │    │    SECURITY      │    │   MANAGEMENT   │ │
│  │   INTERFACE      │    │    MODULE        │    │                │ │
│  │                  │    │                  │    │                │ │
│  │ • Signal Control │    │ • HSM            │    │ • SNMP Agent   │ │
│  │ • Detector Input │    │ • Certificate    │    │ • Remote Mgmt  │ │
│  │ • Camera Feed    │    │   Authority      │    │ • Logging      │ │
│  └──────────────────┘    └──────────────────┘    └────────────────┘ │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

#### RSU Functions

| Function | Description |
|----------|-------------|
| **Broadcasting** | Transmits MAP (intersection geometry), SPaT (signal timing), BSM relay |
| **Aggregation** | Collects BSMs from vehicles, aggregates traffic data |
| **Edge Computing** | Processes data locally for low-latency applications |
| **Relay** | Extends communication range, bridges to backend |
| **Local Authority** | May issue short-term certificates, manage local pseudonyms |

#### RSU Security Challenges

1. **Physical Exposure:** Deployed in public spaces, vulnerable to tampering
2. **Trust Anchor:** Vehicles trust RSU messages; compromise is catastrophic
3. **Network Position:** Bridge between vehicle ad-hoc network and backend
4. **Scale:** Thousands of RSUs create large management overhead

### Backend Systems

The backend infrastructure supports IoV operations at scale:

| System | Function | Security Role |
|--------|----------|---------------|
| **Traffic Management Center (TMC)** | Aggregates regional traffic data, optimizes signal timing | Integrity of traffic control |
| **Security Credential Management System (SCMS)** | Issues certificates, manages pseudonyms, handles revocation | Root of trust |
| **OEM Cloud** | Vehicle telemetry, OTA updates, customer services | Data protection, update integrity |
| **Third-Party Services** | Navigation, parking, insurance telematics | Data sharing governance |

---

## Computing Layers in IoV

IoV employs a **multi-tier computing architecture** to balance latency requirements, processing capabilities, and scalability.

### Edge Computing

**Location:** Within vehicles (OBU) and at RSUs

**Characteristics:**
- Ultra-low latency (< 10 ms)
- Limited processing power
- Direct sensor access
- Autonomous operation capability

**Use Cases:**
| Application | Latency Requirement | Processing Location |
|-------------|--------------------|--------------------|
| Collision avoidance | < 10 ms | Vehicle OBU |
| Intersection assistance | < 50 ms | RSU |
| Emergency braking | < 20 ms | Vehicle OBU |
| Local hazard detection | < 100 ms | RSU + Vehicle |

**Security Considerations:**
- Limited resources for complex cryptography
- Must operate when disconnected from cloud
- Local decision-making without central validation

### Fog Computing

**Location:** Regional data centers, cellular base stations, aggregation points

**Characteristics:**
- Low-to-moderate latency (10-100 ms)
- Moderate processing power
- Regional scope
- Partially autonomous

**Use Cases:**
| Application | Latency Tolerance | Processing Location |
|-------------|------------------|---------------------|
| Regional traffic optimization | 100-500 ms | Fog node |
| Platooning coordination | 50-100 ms | Fog node |
| Map tile distribution | 1-5 seconds | Fog node |
| Certificate validation caching | 50-200 ms | Fog node |

**Security Considerations:**
- Certificate revocation list (CRL) distribution
- Regional anomaly detection
- Trust relationship with multiple edge nodes

### Cloud Computing

**Location:** Centralized data centers (OEM, government, service provider)

**Characteristics:**
- Higher latency acceptable (100 ms - seconds)
- Massive processing and storage capacity
- Global scope
- Dependent on network connectivity

**Use Cases:**
| Application | Latency Tolerance | Processing Location |
|-------------|------------------|---------------------|
| Long-term analytics | Hours | Cloud |
| ML model training | Days | Cloud |
| Certificate generation | Seconds | Cloud (SCMS) |
| OTA update packaging | Minutes | Cloud |
| Fleet management | Seconds | Cloud |
| Navigation routing | 1-5 seconds | Cloud |

**Security Considerations:**
- Massive data aggregation creates privacy risks
- Central point of compromise
- Must secure data in transit and at rest

### Computing Tier Comparison

| Aspect | Edge | Fog | Cloud |
|--------|------|-----|-------|
| **Latency** | < 10 ms | 10-100 ms | > 100 ms |
| **Compute Power** | Limited | Moderate | Unlimited |
| **Storage** | GBs | TBs | PBs |
| **Connectivity** | Intermittent | Reliable | Always-on |
| **Scope** | Single vehicle/RSU | Region | Global |
| **Autonomy** | High | Medium | Low |
| **Example Hardware** | OBU, RSU | MEC server | Data center |

### Real-World Example: Tesla's Architecture

Tesla provides an instructive example of IoV computing architecture:

1. **Edge (Vehicle):**
   - Autopilot computer processes 8 cameras, 12 ultrasonic sensors, radar
   - Makes real-time driving decisions locally
   - Can operate fully without cloud connectivity

2. **Cloud (Tesla Backend):**
   - Receives telemetry from millions of vehicles
   - Trains neural networks on fleet data ("shadow mode")
   - Packages and validates OTA updates
   - Provides navigation and charging network data

3. **Distribution:**
   - OTA updates pushed globally
   - Map data distributed to vehicles
   - Fleet-wide safety alerts

**Security Note:** Tesla's cloud-centric model means a compromise of Tesla's backend could potentially affect millions of vehicles simultaneously—a systemic risk that distributed architectures attempt to mitigate.

---

## Communication Paradigms (V2X)

**Vehicle-to-Everything (V2X)** is the umbrella term for all IoV communication paradigms. Each paradigm has distinct characteristics and security requirements.

### V2V (Vehicle-to-Vehicle)

**Direct communication between vehicles without infrastructure involvement.**

```
    ┌─────────┐                              ┌─────────┐
    │ Vehicle │ ◄──── Broadcast BSM ────►    │ Vehicle │
    │    A    │                              │    B    │
    └─────────┘                              └─────────┘
         │                                        │
         │    Direct Radio Communication          │
         │    (DSRC 5.9 GHz or C-V2X PC5)        │
         └────────────────────────────────────────┘
```

| Aspect | Details |
|--------|---------|
| **Range** | 300-1000 meters |
| **Latency** | < 10 ms |
| **Message Type** | Basic Safety Message (BSM) |
| **Frequency** | 10 Hz (10 messages per second) |
| **Content** | Position, speed, heading, brake status, size |

**Use Cases:**
- Forward collision warning
- Emergency electronic brake light
- Intersection movement assist
- Blind spot warning
- Do not pass warning

**Security Requirements:**
- Message authentication (digital signatures)
- Pseudonymous identity (privacy)
- Plausibility checking (is message physically possible?)
- Replay protection (freshness)

### V2I (Vehicle-to-Infrastructure)

**Communication between vehicles and roadside infrastructure.**

```
                    ┌─────────────┐
                    │  Traffic    │
                    │  Management │
                    │   Center    │
                    └──────┬──────┘
                           │ Backhaul
                    ┌──────▼──────┐
                    │     RSU     │
                    └──────┬──────┘
                           │ DSRC/C-V2X
              ┌────────────┼────────────┐
              │            │            │
         ┌────▼────┐  ┌────▼────┐  ┌────▼────┐
         │Vehicle A│  │Vehicle B│  │Vehicle C│
         └─────────┘  └─────────┘  └─────────┘
```

| Aspect | Details |
|--------|---------|
| **RSU → Vehicle** | MAP, SPaT, TIM (Traveler Information Message) |
| **Vehicle → RSU** | BSM (aggregated), SRM (Signal Request Message) |

**Use Cases:**
- Signal Phase and Timing (SPaT) — know when light will change
- Red light violation warning
- Curve speed warning
- Work zone warnings
- Railroad crossing alerts

**Security Requirements:**
- RSU authentication (vehicles must trust RSU)
- Message integrity (prevent tampering)
- Availability (RSU must function reliably)
- Physical security (RSU tamper resistance)

### V2P (Vehicle-to-Pedestrian)

**Communication between vehicles and vulnerable road users (pedestrians, cyclists).**

```
         ┌─────────┐
         │ Vehicle │
         └────┬────┘
              │ Broadcast
              ▼
    ┌─────────────────────┐
    │    Smartphone /     │
    │    Wearable with    │
    │    V2P Application  │
    └─────────────────────┘
              │
              │
         ┌────▼────┐
         │Pedestrian│
         └─────────┘
```

| Aspect | Details |
|--------|---------|
| **Technologies** | C-V2X, Bluetooth, Wi-Fi Aware |
| **Challenges** | Device penetration, battery life, false positives |

**Use Cases:**
- Pedestrian in crosswalk alert
- Cyclist approaching from blind spot
- School zone alerts
- Emergency vehicle approach notification

**Security & Privacy Challenges:**
- Pedestrians may not want to be tracked
- Phone apps can be spoofed
- High density areas create message congestion
- Vulnerable population protection

### V2N (Vehicle-to-Network)

**Communication between vehicles and cellular/cloud networks.**

```
    ┌─────────┐     Cellular Network      ┌────────────────┐
    │ Vehicle │ ◄────────────────────────► │  Cloud/Backend │
    └─────────┘     (4G LTE / 5G)         └────────────────┘
```

| Aspect | Details |
|--------|---------|
| **Technologies** | 4G LTE, 5G NR, Satellite |
| **Range** | Kilometers (cell coverage) |
| **Latency** | 20-100 ms (LTE), 1-10 ms (5G) |

**Use Cases:**
- Real-time navigation and traffic
- Over-The-Air (OTA) updates
- Remote diagnostics
- Emergency calling (eCall)
- Infotainment streaming
- Fleet management

**Security Requirements:**
- End-to-end encryption
- Server authentication
- Update integrity verification
- Data minimization (privacy)

### V2X Summary Comparison

| Paradigm | Communication Type | Latency | Range | Primary Use |
|----------|-------------------|---------|-------|-------------|
| **V2V** | Direct (ad-hoc) | < 10 ms | 300-1000 m | Safety |
| **V2I** | Direct/Backhaul | < 50 ms | 300-500 m | Traffic management |
| **V2P** | Direct | < 100 ms | 50-200 m | Vulnerable road users |
| **V2N** | Cellular | 20-100 ms | km+ | Services, updates |

---

## Communication Technologies

Two primary technology families compete for V2X communication:

### DSRC (Dedicated Short-Range Communications)

**Based on IEEE 802.11p (now IEEE 802.11bd)**

| Aspect | Specification |
|--------|---------------|
| **Frequency** | 5.850-5.925 GHz (ITS Band) |
| **Bandwidth** | 10 MHz channels |
| **Range** | Up to 1000 m |
| **Latency** | < 2 ms |
| **Data Rate** | 3-27 Mbps |

**Advantages:**
- Mature technology (deployed since 2016)
- Low latency, specifically designed for vehicles
- No cellular subscription required
- Works without infrastructure (V2V)

**Disadvantages:**
- Requires dedicated spectrum
- Limited to V2X (no synergy with other services)
- RSU infrastructure costs

### C-V2X (Cellular Vehicle-to-Everything)

**Based on 3GPP LTE and 5G NR standards**

| Aspect | LTE C-V2X (PC5) | 5G NR C-V2X |
|--------|-----------------|-------------|
| **Frequency** | 5.9 GHz (ITS Band) | 5.9 GHz + mmWave |
| **Range** | Up to 1500 m | Longer |
| **Latency** | < 10 ms | < 3 ms |
| **Data Rate** | Higher than DSRC | Much higher |

**Advantages:**
- Evolves with cellular (5G, 6G)
- Synergy with mobile network infrastructure
- Higher data rates
- Longer range

**Disadvantages:**
- Newer, less deployed
- Dependency on cellular operators
- Coexistence concerns with DSRC

### Technology Comparison

| Criteria | DSRC (802.11p) | C-V2X (PC5) |
|----------|----------------|-------------|
| **Maturity** | Mature | Emerging |
| **Deployment** | US, Japan, EU | China, expanding |
| **Direct V2V** | Yes | Yes |
| **Infrastructure** | RSU network | Can use cellular |
| **Evolution Path** | 802.11bd | 5G NR V2X |

---

## Security Implications of Architecture

Each architectural component introduces specific security concerns:

### Attack Surface Analysis

| Component | Attack Vectors | Potential Impact |
|-----------|---------------|------------------|
| **OBU Radio** | Jamming, spoofing, replay | Loss of V2X communication |
| **OBU Gateway** | CAN bus injection, firmware exploit | Vehicle control compromise |
| **RSU** | Physical tampering, message injection | Region-wide false alerts |
| **Fog Node** | Server compromise, data manipulation | Regional service disruption |
| **Cloud Backend** | Mass data breach, malicious updates | Fleet-wide compromise |
| **PKI/SCMS** | CA compromise, certificate theft | Complete trust collapse |

### Defense-in-Depth Approach

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         SECURITY LAYERS                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│  Layer 1: NETWORK SECURITY                                                   │
│    • Encryption (ECDSA, AES)                                                │
│    • Certificate-based authentication                                        │
│    • Secure protocols (IEEE 1609.2)                                         │
├─────────────────────────────────────────────────────────────────────────────┤
│  Layer 2: MESSAGE SECURITY                                                   │
│    • Digital signatures                                                      │
│    • Plausibility checking                                                   │
│    • Replay protection                                                       │
├─────────────────────────────────────────────────────────────────────────────┤
│  Layer 3: NODE SECURITY                                                      │
│    • Hardware Security Modules (HSM)                                         │
│    • Secure boot                                                             │
│    • Tamper detection                                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│  Layer 4: OPERATIONAL SECURITY                                               │
│    • Certificate revocation                                                  │
│    • Intrusion detection                                                     │
│    • Incident response                                                       │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaways

1. **IoV is a heterogeneous system** spanning in-vehicle, ad-hoc, and infrastructure domains with different trust levels

2. **OBUs and RSUs are critical nodes** — their compromise can have safety-critical consequences

3. **Multi-tier computing (edge/fog/cloud)** balances latency requirements against processing capabilities

4. **V2X encompasses multiple paradigms** (V2V, V2I, V2P, V2N), each with distinct security requirements

5. **DSRC and C-V2X compete** as communication technologies, with C-V2X gaining momentum due to 5G evolution

6. **Security must be layered** — no single control protects the entire architecture

---

## Discussion Questions

1. What are the trade-offs between centralized (cloud-heavy) and distributed (edge-heavy) IoV architectures from a security perspective?

2. How should security responsibilities be divided between OEMs, infrastructure operators, and government agencies?

3. What happens to V2X safety applications if the cellular network fails? How should systems degrade gracefully?

4. As vehicles become more autonomous, should OBU security requirements become legally mandated like seatbelts?

---

## Next Module

Continue to **[Module 2: Characteristics and Challenges](02-characteristics-and-challenges.md)** to understand the unique operational constraints that make IoV security particularly challenging.

---

*This module is part of the [IoV Cybersecurity Guide](../README.md)*
