# Module 7: Intrusion Detection Systems for Vehicles

## Overview

Even with robust authentication and cryptographic protections, IoV systems remain vulnerable to sophisticated attacks. Authorized vehicles may be compromised, legitimate credentials may be stolen, and insider threats may exist. Intrusion Detection Systems (IDS) serve as a critical second line of defense, monitoring vehicle behavior and network traffic to detect malicious activity that bypasses initial security barriers. This module provides comprehensive coverage of IDS concepts, architectures, and techniques specifically designed for vehicular environments.

---

## 7.1 The Need for Vehicular IDS

### 7.1.1 Defense in Depth

**Security Layers in IoV:**

```
Layer 1: Perimeter Security
├── Authentication
├── Access Control
└── Firewalls
    │
    ▼ (Attacker bypasses with valid credentials)
    │
Layer 2: Intrusion Detection
├── Behavior Monitoring
├── Anomaly Detection
└── Signature Detection
    │
    ▼ (Attack detected, alert generated)
    │
Layer 3: Incident Response
├── Alert Processing
├── Automated Response
└── Forensic Analysis
```

**Why Authentication Isn't Enough:**

1. **Compromised Vehicles:** A vehicle with valid credentials may be compromised
2. **Insider Threats:** Malicious actors with legitimate access
3. **Stolen Credentials:** Keys extracted from vehicles or infrastructure
4. **Zero-Day Exploits:** Unknown vulnerabilities in protocols
5. **Supply Chain Attacks:** Malicious components with valid credentials

**The IDS Role:**

IDS monitors for malicious activity that:
- Uses valid credentials
- Passes authentication checks
- Appears legitimate to protocol verification
- Would otherwise go undetected

### 7.1.2 Unique Vehicular IDS Challenges

**Challenges Not Present in Traditional IDS:**

*High Mobility:*
- Monitored entities constantly moving
- Network topology changes rapidly
- Communication relationships transient
- Baseline behavior varies by location

*Real-Time Requirements:*
- Attacks on safety systems require immediate detection
- Millisecond response times needed
- Cannot queue alerts for later processing
- False positives must not disrupt safety

*Resource Constraints:*
- Limited processing power in OBUs
- Battery/energy considerations
- Storage limitations for logging
- Bandwidth constraints for reporting

*Heterogeneous Environment:*
- Multiple vehicle types with different behaviors
- Various manufacturers with different systems
- Legacy vehicles alongside modern ones
- Mix of V2V, V2I, and V2N traffic

*Privacy Concerns:*
- Monitoring must not enable surveillance
- Data minimization required
- Cannot create detailed location logs
- Must protect while monitoring

---

## 7.2 IDS Classification

### 7.2.1 Signature-Based Detection

**Concept:**
Signature-based IDS compares observed activity against a database of known attack patterns (signatures).

**How It Works:**

1. Security researchers analyze known attacks
2. Unique patterns (signatures) extracted
3. Signatures distributed to IDS sensors
4. Real-time traffic compared against signatures
5. Match triggers alert

**Signature Examples for IoV:**

```
# Example: Sybil Attack Signature
IF (same_physical_location) AND (multiple_identities) AND (similar_transmission_characteristics)
THEN alert("Potential Sybil Attack")

# Example: Replay Attack Signature
IF (message_hash IN recent_message_cache) AND (time_delta < threshold)
THEN alert("Potential Replay Attack")

# Example: DoS Attack Signature
IF (message_rate > threshold) AND (single_source)
THEN alert("Potential DoS Attack")
```

**Advantages:**
- Fast detection (pattern matching is efficient)
- Low false positive rate for known attacks
- Easy to understand and maintain
- Definitive identification of attack type

**Disadvantages:**
- Cannot detect unknown attacks (zero-days)
- Requires continuous signature updates
- Attacker can modify attacks to avoid signatures
- Signature database management overhead

### 7.2.2 Anomaly-Based Detection

**Concept:**
Anomaly-based IDS establishes a baseline of "normal" behavior and alerts when significant deviations occur.

**How It Works:**

1. Learning phase: Observe normal system behavior
2. Build statistical model of normal
3. Operational phase: Compare current behavior to model
4. Significant deviations flagged as potential attacks

**Behavioral Metrics for IoV:**

*Network-Level Metrics:*
- Message frequency per vehicle
- Message size distribution
- Protocol compliance rates
- Network topology patterns

*Vehicle-Level Metrics:*
- Acceleration patterns
- Speed variations
- Position changes
- Braking behavior

*Application-Level Metrics:*
- Safety message patterns
- Event frequencies
- Request/response patterns
- Service usage patterns

**Advantages:**
- Can detect unknown attacks (zero-days)
- Adapts to environment changes
- No signature maintenance required
- Detects subtle attack patterns

**Disadvantages:**
- Higher false positive rate
- Requires training period
- Normal behavior can be complex to model
- Attacker can slowly train model to accept attack

### 7.2.3 Specification-Based Detection

**Concept:**
Specification-based IDS monitors for violations of predefined rules based on protocol or system specifications.

**How It Works:**

1. Develop specifications for correct behavior
2. Specifications derived from standards, physics, logic
3. Monitor for specification violations
4. Violations indicate attack or malfunction

**Specification Examples for IoV:**

```
# Physical Plausibility Specifications
- Vehicle speed cannot exceed 250 km/h
- Vehicle cannot accelerate > 15 m/s²
- Vehicle cannot be in two places simultaneously
- Vehicle position must follow road network

# Protocol Specifications
- BSM must include all mandatory fields
- Certificate must be valid and unrevoked
- Message timestamp must be recent
- Sequence numbers must increment

# Logical Specifications
- Emergency braking should precede collision
- Turn signals should precede lane change
- Vehicle heading should match position changes
```

**Advantages:**
- Low false positive rate
- Detects violations even with valid credentials
- Based on ground truth (physics, protocol)
- No training required

**Disadvantages:**
- Requires complete specifications
- Complex behaviors hard to specify
- May not cover all attack types
- Specifications must be updated with protocol changes

---

## 7.3 IDS Architecture for IoV

### 7.3.1 Deployment Models

**In-Vehicle IDS:**

```
┌─────────────────────────────────────┐
│            Vehicle                  │
│  ┌─────────────────────────────┐   │
│  │      On-Board IDS           │   │
│  │  ┌─────┐ ┌─────┐ ┌─────┐   │   │
│  │  │Sensor│ │Analyze│ │Respond│   │   │
│  │  └──┬──┘ └──┬──┘ └──┬──┘   │   │
│  │     │       │       │       │   │
│  │  ┌──┴───────┴───────┴──┐   │   │
│  │  │    Local Alerts     │   │   │
│  │  └─────────────────────┘   │   │
│  └─────────────────────────────┘   │
└─────────────────────────────────────┘
```

*Functions:*
- Monitor in-vehicle CAN bus traffic
- Analyze incoming V2X messages
- Detect local attacks
- Immediate response capability

*Advantages:*
- Lowest latency detection
- No network dependency
- Full local context
- Privacy preserving

*Limitations:*
- Limited processing resources
- Cannot see network-wide patterns
- Isolated view of attacks
- Storage constraints

**Infrastructure-Based IDS:**

```
┌─────────────────────────────────────┐
│              RSU                    │
│  ┌─────────────────────────────┐   │
│  │    Infrastructure IDS       │   │
│  │  ┌─────────────────────┐   │   │
│  │  │    Monitor Region   │   │   │
│  │  └──────────┬──────────┘   │   │
│  │             │               │   │
│  │  ┌──────────▼──────────┐   │   │
│  │  │  Correlate Vehicles │   │   │
│  │  └──────────┬──────────┘   │   │
│  │             │               │   │
│  │  ┌──────────▼──────────┐   │   │
│  │  │   Report to Cloud   │   │   │
│  │  └─────────────────────┘   │   │
│  └─────────────────────────────┘   │
└─────────────────────────────────────┘
```

*Functions:*
- Monitor all traffic in coverage area
- Correlate multiple vehicle behaviors
- Detect regional attack patterns
- Report to central systems

*Advantages:*
- Multi-vehicle correlation
- More resources available
- Wider attack visibility
- Centralized logging

*Limitations:*
- Higher latency
- Coverage gaps
- Single point of attack
- Privacy implications

**Hybrid/Distributed IDS:**

```
┌──────────┐    ┌──────────┐    ┌──────────┐
│ Vehicle 1│    │ Vehicle 2│    │ Vehicle 3│
│   IDS    │    │   IDS    │    │   IDS    │
└────┬─────┘    └────┬─────┘    └────┬─────┘
     │               │               │
     └───────────┬───┴───────────────┘
                 │
          ┌──────▼──────┐
          │    RSU      │
          │    IDS      │
          └──────┬──────┘
                 │
          ┌──────▼──────┐
          │   Cloud     │
          │    IDS      │
          └─────────────┘
```

*Functions:*
- Local detection at vehicle
- Regional correlation at RSU
- Global analysis in cloud
- Distributed consensus on threats

*Advantages:*
- Best of both approaches
- Resilient to single failures
- Scalable architecture
- Comprehensive coverage

*Challenges:*
- Complex coordination
- Consistent policy
- Data sharing protocols
- Trust establishment

### 7.3.2 Detection Components

**Data Collection:**
- CAN bus sniffers
- V2X message loggers
- Sensor data aggregators
- Network traffic monitors

**Preprocessing:**
- Message parsing
- Feature extraction
- Normalization
- Temporal alignment

**Analysis Engine:**
- Pattern matching
- Statistical analysis
- Machine learning models
- Rule evaluation

**Alert Management:**
- Prioritization
- Correlation
- Deduplication
- Escalation

**Response Interface:**
- Driver warnings
- Automatic responses
- Reporting to authorities
- Evidence preservation

---

## 7.4 Machine Learning for Vehicular IDS

### 7.4.1 ML Approaches

**Supervised Learning:**

*Concept:* Train on labeled examples of normal and malicious behavior

*Algorithms:*
- Support Vector Machines (SVM)
- Random Forests
- Neural Networks
- Gradient Boosting

*Requirements:*
- Labeled training data
- Balanced attack/normal examples
- Representative coverage of attack types

*Challenges:*
- Obtaining labeled vehicular attack data
- Class imbalance (attacks rare)
- Adversarial manipulation of training

**Unsupervised Learning:**

*Concept:* Learn normal patterns without labels, detect outliers

*Algorithms:*
- K-means clustering
- Isolation Forest
- Autoencoders
- DBSCAN

*Advantages:*
- No labels required
- Can detect unknown attacks
- Adapts to environment

*Challenges:*
- Defining what constitutes anomaly
- Threshold selection
- Handling legitimate outliers

**Deep Learning:**

*Applications:*
- Complex pattern recognition
- Temporal sequence analysis (LSTM)
- Multi-modal sensor fusion
- End-to-end attack detection

*Architectures:*
- Convolutional Neural Networks (CNN) for spatial patterns
- Recurrent Neural Networks (RNN) for temporal patterns
- Autoencoders for anomaly detection
- Transformer models for sequence analysis

### 7.4.2 Feature Engineering

**Network Traffic Features:**

| Feature | Description | Attack Relevance |
|---------|-------------|------------------|
| Message rate | Messages per second | DoS detection |
| Unique sources | Distinct sender count | Sybil detection |
| Message size | Byte distribution | Protocol violation |
| Protocol compliance | Field validation | Malformed messages |

**Vehicle Behavior Features:**

| Feature | Description | Attack Relevance |
|---------|-------------|------------------|
| Speed consistency | Variation from mean | Position spoofing |
| Acceleration profile | Rate of change | False kinematic data |
| Heading alignment | Match with trajectory | Message falsification |
| Position plausibility | Road network match | GPS spoofing |

**Temporal Features:**

| Feature | Description | Attack Relevance |
|---------|-------------|------------------|
| Inter-message interval | Time between messages | Timing attacks |
| Sequence progression | ID increments | Replay detection |
| Time drift | Clock synchronization | Temporal attacks |
| Pattern periodicity | Regular patterns | Automated attacks |

### 7.4.3 Context-Aware AI

**The Context Problem:**

Traditional IDS lacks context:
- High message rate might be: attack OR rush hour
- Sudden braking might be: false data OR real obstacle
- Position jump might be: spoofing OR GPS correction

**Context-Aware Approach:**

Incorporate multiple data sources:
1. V2X message content
2. Vehicle's own sensor data
3. Environmental conditions
4. Historical patterns
5. Neighboring vehicle data

**Example: Hard Braking Verification:**

```
Received: "Hard braking event" from Vehicle A

Context Checks:
1. Does Vehicle A's radar show obstacle? [Check own sensors]
2. Are nearby vehicles also braking? [Check neighbor messages]
3. Is road condition icy? [Check weather data]
4. Is Vehicle A's history consistent? [Check reputation]
5. Does deceleration match claim? [Check physics]

Decision:
- If all context confirms: LEGITIMATE
- If context contradicts: SUSPICIOUS
- If insufficient context: FLAG FOR REVIEW
```

### 7.4.4 Federated Learning for IDS

**The Challenge:**
Centralized ML training requires collecting data from all vehicles, creating privacy and bandwidth issues.

**Federated Learning Solution:**

1. Each vehicle trains local model on local data
2. Vehicles share model updates (not data)
3. Central server aggregates updates
4. Improved global model distributed back
5. Repeat iteratively

**Benefits:**
- Data stays local (privacy)
- Reduced bandwidth (model updates small)
- Leverages diverse training data
- Continuous improvement

**Challenges:**
- Non-IID data distribution
- Model poisoning attacks
- Communication overhead
- Convergence guarantees

---

## 7.5 Misbehavior Detection Systems

### 7.5.1 MDS Concept

**Misbehavior Detection vs. Intrusion Detection:**

| Aspect | Traditional IDS | Misbehavior Detection |
|--------|-----------------|----------------------|
| Focus | Network intrusions | False vehicular data |
| Data | Network packets | V2X messages + sensors |
| Normal baseline | Network traffic | Physical reality |
| Indicators | Protocol violations | Physics violations |

**What MDS Detects:**
- False position claims
- Incorrect speed reports
- Fabricated events
- Phantom vehicles
- Impossible trajectories

### 7.5.2 Plausibility Checks

**Position Plausibility:**

```
Check 1: Road Network Matching
- Claimed position on valid road? [Road map lookup]
- Previous positions form valid route? [Path analysis]

Check 2: Physical Possibility
- Distance from last position physically possible? 
- Max_distance = time_delta × max_speed
- If claimed_distance > max_distance: SUSPICIOUS

Check 3: Consistency with Neighbors
- Do nearby vehicles confirm existence?
- Does any vehicle's sensors see this position?

Check 4: Signal Direction
- Does RF signal direction match claimed position?
- Requires directional antenna infrastructure
```

**Speed Plausibility:**

```
Check 1: Absolute Limits
- Speed claimed within physical limits?
- No vehicle exceeds ~500 km/h

Check 2: Acceleration Limits
- Speed change physically achievable?
- Max acceleration ~15 m/s² (performance cars)
- Max deceleration ~20 m/s² (emergency braking)

Check 3: Speed vs. Road Type
- Highway speeds on residential street? SUSPICIOUS
- Very low speed on highway? Investigate

Check 4: Consistency
- Speed matches position change rate?
- Speed × time ≈ distance traveled?
```

**Temporal Plausibility:**

```
Check 1: Message Timing
- Timestamp recent? (within tolerance window)
- Timestamps monotonically increasing?

Check 2: Sequence Integrity
- Message sequence numbers incrementing correctly?
- No gaps indicating dropped/modified messages?

Check 3: Temporal Consistency
- Event timing physically possible?
- Cause before effect?
```

### 7.5.3 Sensor Fusion for Verification

**Cross-Validating V2X with Sensors:**

```
V2X Message Claims: Vehicle ahead braking hard
Own Sensors Check:
├── Camera: Is there a vehicle ahead?
├── Radar: What is closing rate?
├── LiDAR: Confirm vehicle position
└── Speed sensor: Am I approaching?

Cross-Validation Result:
├── All agree → TRUST message
├── Some disagree → WEIGHT evidence
└── All disagree → REJECT message, alert
```

**Challenges:**
- Sensor failures can mimic attacks
- Attacker might spoof sensors too
- Weather affects sensor reliability
- Computational cost of fusion

---

## 7.6 Response Mechanisms

### 7.6.1 Alert Classification

**Severity Levels:**

| Level | Description | Example | Response Time |
|-------|-------------|---------|---------------|
| Critical | Immediate safety threat | False emergency braking | Milliseconds |
| High | Likely active attack | Sybil attack detected | Seconds |
| Medium | Suspicious activity | Anomalous message patterns | Minutes |
| Low | Minor anomaly | Single implausible reading | Hours |
| Info | Informational | Statistics for analysis | None required |

### 7.6.2 Automated Responses

**Local Responses (Vehicle-Level):**

1. **Message Filtering:**
   - Drop messages from suspicious sources
   - Reduce trust weight for questionable data
   - Fall back to local sensors

2. **Driver Alerting:**
   - Visual warnings on dashboard
   - Audio alerts for critical issues
   - Haptic feedback (steering vibration)

3. **Operational Adjustments:**
   - Increase following distance
   - Reduce reliance on V2X
   - Enable enhanced sensor monitoring

**Network Responses (Infrastructure-Level):**

1. **Misbehavior Reporting:**
   - Report to Misbehavior Authority
   - Include evidence logs
   - Request revocation if warranted

2. **Local Revocation:**
   - Temporarily blacklist locally
   - Share with nearby RSUs
   - Pending official revocation

3. **Traffic Management:**
   - Adjust signal timing
   - Reroute traffic if needed
   - Alert emergency services

### 7.6.3 Evidence Preservation

**Forensic Requirements:**
- Complete message logs
- Timestamp preservation
- Chain of custody
- Tamper evidence
- Legal admissibility

**What to Log:**
- Full message content
- Sender identification
- Reception timestamp
- Detection indicators
- Sensor corroboration
- Context information

**Storage Considerations:**
- Secure, tamper-evident storage
- Encryption of sensitive data
- Retention policies (legal requirements)
- Privacy-preserving logging

---

## 7.7 Challenges and Future Directions

### 7.7.1 Current Challenges

**False Positives:**
- Legitimate anomalies exist
- Unusual but valid behavior
- Sensor malfunctions
- Driver erratic behavior (non-malicious)

**Adversarial Machine Learning:**
- Attackers craft inputs to evade detection
- Model poisoning through gradual training
- Transferability of evasion techniques

**Real-Time Constraints:**
- Detection must be fast enough to matter
- Cannot delay safety messages
- Resource constraints on vehicles

**Privacy vs. Monitoring:**
- Effective IDS requires observation
- Observation conflicts with privacy
- Balancing detection and privacy

### 7.7.2 Research Directions

**Explainable AI for IDS:**
- Why did IDS flag this activity?
- Important for trust and legal proceedings
- Human-understandable explanations

**Collaborative Detection:**
- Vehicles sharing detection insights
- Crowdsourced threat intelligence
- Distributed consensus on threats

**Adaptive Thresholds:**
- Context-aware alert thresholds
- Self-tuning based on environment
- Learning from feedback

**Quantum-Resistant Detection:**
- Preparing for post-quantum attackers
- Detection of quantum-enabled attacks
- Future-proofing IDS systems

---

## 7.8 Summary

Intrusion Detection Systems provide essential defense-in-depth for IoV environments, detecting attacks that bypass authentication and cryptographic protections. The unique challenges of vehicular environments—high mobility, real-time requirements, resource constraints—require specialized IDS architectures and techniques. Signature-based, anomaly-based, and specification-based detection each offer different strengths. Machine learning, particularly context-aware AI, shows promise for improving detection accuracy. Misbehavior Detection Systems specifically address the challenge of false vehicular data by cross-validating V2X messages with physical sensors and plausibility checks.

Key takeaways:
1. IDS provides critical second-line defense
2. Multiple detection approaches needed (signature, anomaly, specification)
3. Context-awareness essential for reducing false positives
4. Misbehavior detection focuses on physical plausibility
5. Response mechanisms must balance speed and accuracy

---

## Review Questions

1. Why is authentication alone insufficient to secure IoV systems?
2. Compare and contrast signature-based and anomaly-based detection for vehicular environments.
3. What makes specification-based detection particularly well-suited for IoV?
4. How can sensor fusion improve misbehavior detection accuracy?
5. Describe three challenges specific to deploying IDS in vehicular environments.

---

## Practical Exercise

**Misbehavior Detection Simulation:**

Using a V2X simulation environment:

1. Implement basic plausibility checks for position data:
   - Road network matching
   - Maximum speed bounds
   - Maximum acceleration bounds

2. Generate attack scenarios:
   - Position spoofing (false location)
   - Speed falsification
   - Sudden position jumps

3. Evaluate detection performance:
   - True positive rate
   - False positive rate
   - Detection latency

4. Analyze failure cases:
   - When does detection fail?
   - How can attacks evade detection?

5. Propose improvements based on findings

---

[Next Module: Blockchain for IoV Security](08-blockchain-for-iov.md) | [Previous Module: Cryptography and Authentication in IoV](06-cryptography-and-authentication.md) | [Back to Index](../README.md)
