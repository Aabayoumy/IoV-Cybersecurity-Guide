# Internet of Vehicles (IoV) Cybersecurity: Threats, VANET Security, and Defenses

---

## Slide 1: Introduction to the Internet of Vehicles (IoV)
**Introduction**

**On-Slide Content (Bullet points for the screen):**
- Definition of IoV
- A dynamic, mobile communication system
- Connects vehicles to their surroundings and the internet

**Speaker Notes / Deep Dive Info:**
- Welcome everyone to this presentation on the Internet of Vehicles, or IoV, and its cybersecurity landscape. Let's start from Level 0: what exactly is the IoV? Simply put, the Internet of Vehicles is a massive, dynamic network where cars aren't just mechanical machines anymore; they are smart, mobile computing platforms. They constantly gather, process, and share data with everything around them. For example, modern Tesla and Waymo vehicles generate terabytes of sensor and camera data daily, communicating with cloud servers to improve their self-driving algorithms. Think of it as the Internet of Things (IoT), but specifically designed for fast-moving vehicles.

---

## Slide 2: Evolution: From VANETs to IoV
**Introduction**

**On-Slide Content (Bullet points for the screen):**
- VANETs (Vehicular Ad Hoc Networks)
- Integration with the Internet of Things (IoT)
- The shift to smart, autonomous ecosystems

**Speaker Notes / Deep Dive Info:**
- To understand IoV, we must look at its predecessor: VANETs, which stands for Vehicular Ad Hoc Networks. Historically, VANETs were simple local networks where cars just talked directly to each other or to local roadside antennas to share basic safety warnings. As technology evolved, we integrated these isolated networks with IoT and 4G/5G cellular networks. Real-world example: the shift from basic DSRC (Dedicated Short-Range Communications) toll tags to cellular C-V2X (Cellular V2X) technology used by automakers like Ford and Audi today. This evolution created the IoV—a globally connected, smart ecosystem capable of supporting full autonomous driving rather than just local safety pings.

---

## Slide 3: The Critical Need for Cybersecurity in IoV
**Introduction**

**On-Slide Content (Bullet points for the screen):**
- Life-safety implications
- Massive attack surface
- Highly sensitive data generation

**Speaker Notes / Deep Dive Info:**
- Why do we care so much about cybersecurity in IoV? In traditional IT, a hack might mean a stolen credit card. In the IoV, a cyberattack has direct life-safety implications. Millions of connected cars create a massive "attack surface." A real-world example of this severity is the famous 2015 Jeep Cherokee hack by researchers Charlie Miller and Chris Valasek. They remotely compromised a Jeep driving 70 mph on a highway, gaining control of its steering, transmission, and brakes via its internet-connected infotainment system. Protecting this network is not just about data privacy; it's about physical safety.

---

## Slide 4: The Core Nodes of IoV
**Module 1: Architecture and Nodes**

**On-Slide Content (Bullet points for the screen):**
- On-Board Unit (OBU)
- Roadside Unit (RSU)
- Trusted Authority (TA) / Central Servers

**Speaker Notes / Deep Dive Info:**
- Let's break down the physical architecture of the IoV into its core nodes. First is the OBU, or On-Board Unit, the "brain" inside your car handling communication. Second is the RSU, or Roadside Unit. These are the antennas on traffic lights and highway signs acting as access points. Finally, we have the Trusted Authority (TA) or central servers. A real-world example of central servers in action is the US Department of Transportation's SCMS (Security Credential Management System), a centralized authority designed to issue secure certificates to vehicles and infrastructure to ensure the entire network's trust system.

---

## Slide 5: The Three-Tier Computing Architecture
**Module 1: Architecture and Nodes**

**On-Slide Content (Bullet points for the screen):**
- Edge Computing (Ultra-low latency)
- Fog Computing (Localized processing)
- Cloud Computing (Heavy computation and storage)

**Speaker Notes / Deep Dive Info:**
- Data in the IoV is processed across three distinct layers. At the bottom is the "Edge" layer. This is processing done right on the vehicle (OBU) for tasks needing ultra-low latency, like slamming the brakes. The middle tier is "Fog" computing, which handles localized data, like optimizing local traffic lights. The top tier is "Cloud" computing. A perfect real-world example of Cloud Architecture in IoV is Tesla's Over-The-Air (OTA) update system. Tesla uses massive centralized cloud servers to push gigabytes of software updates, patches, and autopilot enhancements down to millions of cars worldwide overnight.

---

## Slide 6: Vehicle-to-Everything (V2X) Communication
**Module 1: Architecture and Nodes**

**On-Slide Content (Bullet points for the screen):**
- V2V (Vehicle-to-Vehicle)
- V2I (Vehicle-to-Infrastructure)
- V2P (Vehicle-to-Pedestrian) and V2N (Vehicle-to-Network)

**Speaker Notes / Deep Dive Info:**
- The way vehicles communicate in the IoV is called V2X, meaning "Vehicle-to-Everything." V2V is when cars talk directly to each other to prevent collisions. V2I is cars talking to RSUs for toll collection or red-light timing. An example of V2I is Audi's "Traffic Light Information" feature, which connects cars to city grids to tell the driver exactly when a light will turn green. V2P involves detecting pedestrian smartphones, and V2N connects the car to the broader network for weather updates and music streaming.

---

## Slide 7: Key Characteristics of IoV Networks
**Module 2: Characteristics and Challenges**

**On-Slide Content (Bullet points for the screen):**
- Extremely high mobility
- Massive scale and density
- Heterogeneous technologies

**Speaker Notes / Deep Dive Info:**
- The IoV is unlike any standard computer network. Its most defining characteristic is high mobility. Cars move at 70+ miles per hour, meaning the network shifts constantly. Second is massive scale and density. A real-world example of this density challenge occurs during mass evacuations (like a hurricane) or major sporting events, where thousands of cars pack into a single area, severely congesting local cellular networks and RSUs. Third is heterogeneity: the network is a mix of different car brands, communication standards (DSRC vs. 5G), and processing capabilities seamlessly speaking the same language.

---

## Slide 8: Network Topology and Intermittent Connectivity
**Module 2: Characteristics and Challenges**

**On-Slide Content (Bullet points for the screen):**
- Highly dynamic topology
- Frequent link breakages
- Short connection durations

**Speaker Notes / Deep Dive Info:**
- Because vehicles move so fast, the "topology"—the physical layout of connected nodes—changes every second. In a typical office network, connections are stable. In IoV, two cars passing each other on a highway might only have 3-5 seconds to connect, exchange data, and disconnect. A real-world parallel is how your cell phone sometimes drops a call when switching between cellular towers while driving fast. Designing a network that can reliably deliver critical safety messages before the connection drops is one of the hardest engineering problems in IoV.

---

## Slide 9: Operational and Environmental Challenges
**Module 2: Characteristics and Challenges**

**On-Slide Content (Bullet points for the screen):**
- Strict latency requirements
- Resource constraints on older nodes
- Harsh physical environments

**Speaker Notes / Deep Dive Info:**
- Beyond moving fast, IoV faces strict latency requirements. A message saying "brake now" must be delivered in milliseconds. Another challenge is resource constraints: a new luxury EV has a supercomputer, but a ten-year-old car might have very limited processing power. Finally, these systems operate in harsh environments. Consider sensors in self-driving cars operating in the freezing temperatures of a Minnesota winter or the extreme heat of Death Valley. Extreme conditions cause hardware faults that can mimic cyberattacks, causing network anomalies that the system must decipher.

---

## Slide 10: Understanding Routing in IoV
**Module 3: Routing Protocols**

**On-Slide Content (Bullet points for the screen):**
- What is routing?
- Finding paths from sender to receiver
- The challenge of moving targets

**Speaker Notes / Deep Dive Info:**
- Let's talk about "Routing." At Level 0, routing is simply finding a path to send a message from Car A to Car B. If they are too far apart, the message must "hop" through other cars. The massive challenge in IoV is that the "routers" (the other cars) are constantly moving. A path that existed a second ago might vanish because a car turned down a different street. Therefore, routing in IoV must be incredibly fast, dynamic, and self-healing. Think of it like using the Waze app, but instead of mapping roads, the cars are mapping out a temporary internet connection through other moving cars.

---

## Slide 11: Types of Routing Protocols
**Module 3: Routing Protocols**

**On-Slide Content (Bullet points for the screen):**
- Topology-based routing (Proactive vs. Reactive)
- Position-based (Geographic) routing
- Cluster-based routing

**Speaker Notes / Deep Dive Info:**
- How do cars figure out paths? They use Routing Protocols. Topology-based routing tries to maintain a map of the network, but because maps change too fast in IoV, we often use Position-based or Geographic routing. Here, a car uses GPS to know exactly where the destination is and forwards the message to whichever neighbor is physically closest. We also use Cluster-based routing, where cars group together and elect a "leader" car. A real-world example of clustering is truck platooning, where a lead semi-truck digitally links with trailing trucks, handling communications and route planning for the entire convoy to save fuel and bandwidth.

---

## Slide 12: The Need for Secure Routing
**Module 3: Routing Protocols**

**On-Slide Content (Bullet points for the screen):**
- The assumption of node cooperation
- Malicious nodes dropping or altering packets
- Integrating security into routing protocols

**Speaker Notes / Deep Dive Info:**
- Standard routing protocols have a fatal flaw: they assume everyone is a good guy. They rely on the cooperation of all nodes to forward messages. But what if a hacker compromises a car? That malicious car could promise to forward a critical safety message but secretly drop it. Real-world researchers have demonstrated "wormhole attacks" in mobile networks, where two malicious nodes capture traffic at one location and tunnel it to another, completely confusing the routing algorithms. Therefore, we need "Secure Routing," integrating cryptographic checks and trust scores so cars only forward messages through verified neighbors.

---

## Slide 13: Overview of the IoV Threat Landscape
**Module 4: Attack Taxonomy**

**On-Slide Content (Bullet points for the screen):**
- What is an attack taxonomy?
- Network-level threats
- Identity, Data, and Physical threats

**Speaker Notes / Deep Dive Info:**
- Now we enter the dark side of IoV: the attacks. An "attack taxonomy" is a structured way of classifying cyberattacks. Because IoV is so complex, hackers have many ways to attack it. We divide these into four categories: Network attacks (disrupting the flow of data), Identity attacks (pretending to be someone else), Data attacks (stealing information), and Physical attacks. A famous real-world wake-up call was when Tencent's Keen Security Lab remotely hacked a Tesla Model S from miles away, manipulating its brakes, windshield wipers, and door locks while the car was in motion, proving that the threat landscape is very real.

---

## Slide 14: Network and Routing Attacks
**Module 4: Attack Taxonomy**

**On-Slide Content (Bullet points for the screen):**
- Blackhole Attack
- Sinkhole Attack
- Distributed Denial of Service (DDoS)

**Speaker Notes / Deep Dive Info:**
- Let's look at Network attacks. In a Blackhole Attack, a malicious car tricks the network into thinking it has the fastest route to everywhere, then drops all the data. A Sinkhole Attack draws in massive amounts of traffic to analyze or alter. Finally, DDoS (Distributed Denial of Service) is when attackers flood the network with garbage messages. A real-world example of DDoS impacts on mobility happened in 2022 in Moscow, where hackers ordered dozens of Yandex taxis to the same location at the same time via the app's API, creating a massive artificial traffic jam that paralyzed the city center for hours.

---

## Slide 15: Identity and Trust-Based Attacks
**Module 4: Attack Taxonomy**

**On-Slide Content (Bullet points for the screen):**
- Sybil Attack
- Spoofing (Masquerading)
- Replay Attack

**Speaker Notes / Deep Dive Info:**
- Identity attacks are where hackers lie about who they are. The most famous is the Sybil Attack, where a single malicious car creates dozens of fake digital identities. Spoofing is pretending to be a legitimate car or police RSU. A Replay Attack is recording a valid message and playing it back later. A real-world example of this is the infamous "RollJam" or relay attack used by car thieves today. Thieves use cheap radio devices to intercept the unlock signal from a victim's key fob, record it, block it from reaching the car, and replay it later to easily steal the vehicle without breaking a window.

---

## Slide 16: Data, Privacy, and Physical Attacks
**Module 4: Attack Taxonomy**

**On-Slide Content (Bullet points for the screen):**
- Eavesdropping and Location Tracking
- GPS Spoofing
- Hardware tampering (Physical Attack)

**Speaker Notes / Deep Dive Info:**
- We also have Privacy and Physical attacks. Eavesdropping leads to Location Tracking, invading driver privacy. GPS Spoofing is incredibly dangerous. In a real-world demonstration at DEF CON, security researchers successfully used a $300 software-defined radio to spoof GPS signals, tricking a smartphone navigation app and a drone into thinking they were in a completely different location. If done to an autonomous car, it could veer off the road. Physical attacks involve physically breaking into the OBU, such as plugging a malicious device into the OBD-II diagnostic port under the steering wheel to inject malicious commands directly into the car's internal network.

---

## Slide 17: The Authentication vs. Privacy Dilemma
**Module 5: Authentication and Privacy**

**On-Slide Content (Bullet points for the screen):**
- The need for strict authentication (Trust)
- The need for strict privacy (Anonymity)
- The inherent conflict

**Speaker Notes / Deep Dive Info:**
- Moving to defenses, we face a massive paradox: the Authentication vs. Privacy dilemma. We need strict Authentication before a car hits the brakes based on a warning message. But we also need strict Privacy. If every message broadcasts a permanent ID, anyone can track you. A real-world example of location data privacy gone wrong was when the fitness app Strava published a global "heatmap" of user activity, accidentally revealing the hidden locations and patrol routes of secret military bases because soldiers were wearing fitness trackers. If IoV data isn't anonymized properly, the exact same mass-surveillance risk applies to every driver on earth.

---

## Slide 18: Public Key Infrastructure (PKI) in IoV
**Module 5: Authentication and Privacy**

**On-Slide Content (Bullet points for the screen):**
- What is PKI?
- Digital Certificates
- Trusted Certificate Authorities (CA)

**Speaker Notes / Deep Dive Info:**
- The foundation of solving this dilemma is PKI, or Public Key Infrastructure. Think of a digital certificate as a digital passport. When a car is manufactured, a highly secure entity, a Certificate Authority (CA), gives the car this passport. When the car sends a message, it attaches a digital signature. Other cars instantly check that signature to verify authenticity. The real-world implementation of this for connected cars in the US is the SCMS (Security Credential Management System), developed by the DOT and automakers to issue these certificates securely and at an unprecedented scale.

---

## Slide 19: Pseudonyms and Mix-Zones for Privacy
**Module 5: Authentication and Privacy**

**On-Slide Content (Bullet points for the screen):**
- Pseudonyms (Temporary IDs)
- Frequent identity rotation
- Mix-Zones for unlinking paths

**Speaker Notes / Deep Dive Info:**
- To solve the privacy side, we don't let cars use their real digital passports. The PKI system issues them thousands of temporary "Pseudonyms." A car uses one pseudonym for 5 minutes, throws it away, and switches to a new one, making tracking impossible. We also use "Mix-Zones," specific areas like intersections where cars temporarily stop broadcasting, silently change their pseudonyms, and drive out. Because they "mixed" together, an attacker cannot tell which new pseudonym belongs to which car. This is similar in concept to "Tor" routing used on the internet to anonymize web traffic by mixing it with others.

---

## Slide 20: Cryptography Fundamentals in IoV
**Module 6: Cryptography & Key Management**

**On-Slide Content (Bullet points for the screen):**
- What is Cryptography?
- Encryption (Confidentiality)
- Digital Signatures (Integrity and Authenticity)

**Speaker Notes / Deep Dive Info:**
- All these defenses rely on Cryptography, using complex mathematics to secure data. In IoV, we use it for Encryption to ensure confidentiality, and Digital Signatures to prove who sent a message (Authenticity) and that it wasn't tampered with (Integrity). A real-world application of digital signatures in vehicles is "Secure Boot." When you start a modern car, the computer uses cryptographic signatures to verify that the firmware hasn't been maliciously altered by a hacker before it allows the engine to turn on. 

---

## Slide 21: Symmetric vs. Asymmetric Cryptography and ECC
**Module 6: Cryptography & Key Management**

**On-Slide Content (Bullet points for the screen):**
- Symmetric Cryptography (Fast, hard to share keys)
- Asymmetric Cryptography (Easy to share, mathematically heavy)
- Elliptic Curve Cryptography (ECC) - The IoV Standard

**Speaker Notes / Deep Dive Info:**
- Symmetric cryptography is fast but sharing keys securely between moving cars is hard. Asymmetric uses a pair of keys (public/private) solving the sharing problem, but traditional algorithms like RSA take too long for cars to process in milliseconds. The standard for IoV is ECC, or Elliptic Curve Cryptography. ECC provides the same security as RSA but uses incredibly complex geometric curves with much smaller key sizes. A real-world example is your smartphone; Apple's iMessage and WhatsApp both use ECC because it is lightweight enough for mobile processors while providing military-grade encryption, exactly what vehicular networks need.

---

## Slide 22: The Key Management Lifecycle
**Module 6: Cryptography & Key Management**

**On-Slide Content (Bullet points for the screen):**
- Generation and Distribution
- Secure Storage (HSMs)
- Revocation (CRLs)

**Speaker Notes / Deep Dive Info:**
- We must manage the entire "Lifecycle" of a cryptographic key. Keys must be stored inside the OBU in a tamper-proof chip called a Hardware Security Module (HSM). In the real world, automotive semiconductor companies like Infineon and NXP manufacture these specialized, physically hardened chips (like the TPM in your laptop) directly onto the car's motherboard. Finally, if a car is hacked, the central authority must instantly revoke its keys by broadcasting a Certificate Revocation List (CRL) to all cars, essentially saying "Do not trust this car anymore."

---

## Slide 23: Introduction to IDS in Vehicular Networks
**Module 7: Intrusion Detection Systems (IDS)**

**On-Slide Content (Bullet points for the screen):**
- What is an IDS?
- The second line of defense
- Monitoring network traffic and behavior

**Speaker Notes / Deep Dive Info:**
- Cryptography is our first line of defense. But what if an authenticated car gets infected and becomes an "insider threat"? This is why we need Intrusion Detection Systems, or IDS. An IDS is like a security camera for the network. It constantly monitors data packets and car behavior. A critical real-world example of this is CAN bus voltage monitoring. Researchers have developed physical IDS sensors that monitor the exact electrical voltage levels on the car's internal wires. If a hacker plugs a malicious device into the car to inject fake brake commands, the IDS detects the slight anomaly in the electrical voltage and immediately flags the attack.

---

## Slide 24: Signature-based vs. Anomaly-based Detection
**Module 7: Intrusion Detection Systems (IDS)**

**On-Slide Content (Bullet points for the screen):**
- Signature-based IDS (Known threats)
- Anomaly-based IDS (Behavioral deviations)
- Pros and cons of each approach

**Speaker Notes / Deep Dive Info:**
- Signature-based IDS recognizes known patterns, acting like traditional antivirus software. It's fast but blind to "zero-day" attacks. Anomaly-based IDS learns what "normal" traffic looks like and flags deviations. A real-world example of an anomaly-based trigger would be if a vehicle's infotainment system, which usually just streams Spotify, suddenly starts attempting to send thousands of commands to the car's steering control module. The IDS would flag this massive behavioral anomaly and instantly block the traffic, even if the attack had never been seen before.

---

## Slide 25: Machine Learning and AI in Modern IDS
**Module 7: Intrusion Detection Systems (IDS)**

**On-Slide Content (Bullet points for the screen):**
- Overcoming traditional IDS limitations
- Pattern recognition with Machine Learning
- Federated Learning for privacy-preserving AI

**Speaker Notes / Deep Dive Info:**
- Modern IDS uses Artificial Intelligence to catch zero-days without overwhelming false positives. A major advancement is Federated Learning. Instead of cars sending private driving data to the cloud, the cloud sends the AI model down to the cars. The cars train the model locally. A real-world parallel is how Google's Gboard keyboard on your phone learns the words you type to improve autocorrect without ever sending your actual typed messages back to Google's servers. This allows the IoV network to build powerful global threat detection without compromising driver privacy.

---

## Slide 26: Blockchain for Decentralized Trust
**Module 8: Blockchain for IoV Security**

**On-Slide Content (Bullet points for the screen):**
- Removing the single point of failure
- Decentralized identity management
- Smart contracts for vehicle interactions

**Speaker Notes / Deep Dive Info:**
- As we scale the IoV, relying on a single central server (like a massive government database) creates a "single point of failure." If hackers take down that server, the whole trust system collapses. Enter Blockchain. Blockchain allows us to create a decentralized, distributed ledger of trust. Instead of one server holding all the security certificates, thousands of nodes share the verification duties. Using smart contracts, vehicles can automatically and securely negotiate interactions—for example, automatically paying a toll or paying an EV charging station instantly and securely, without relying on a central bank server.

---

## Slide 27: Immutable Record Keeping and Liability
**Module 8: Blockchain for IoV Security**

**On-Slide Content (Bullet points for the screen):**
- Tamper-proof event data recorders (Digital Black Box)
- Accident reconstruction and liability
- Insurance industry applications

**Speaker Notes / Deep Dive Info:**
- Another powerful use case for Blockchain in IoV is immutable record keeping. In traditional cars, the Event Data Recorder (the black box) can theoretically be physically tampered with by an attacker to hide evidence of a hack. If a self-driving car gets into an accident, who is at fault? The human or the AI? By logging critical sensor data and decisions onto a blockchain, we create a mathematically tamper-proof history of events leading up to the crash. Insurance companies and law enforcement can trust this ledger absolutely to determine liability, knowing the data could not have been altered after the fact.

---

## Slide 28: Securing Over-The-Air (OTA) Updates
**Module 8: Blockchain for IoV Security**

**On-Slide Content (Bullet points for the screen):**
- The vulnerability of centralized OTA servers
- Blockchain for firmware verification
- Distributed update delivery

**Speaker Notes / Deep Dive Info:**
- We discussed Tesla's OTA updates earlier. While convenient, centralized OTA servers are prime targets. If a hacker breaches the automaker's central server, they could push a malicious software update to millions of cars at once—a devastating scenario. Blockchain solves this by securing the OTA process. Automakers can publish the cryptographic hash (the digital fingerprint) of the new software update onto the blockchain. Before a car installs any downloaded update, it checks the blockchain. If the fingerprint doesn't match the immutable ledger, the car knows the update was tampered with and aborts the installation.

---

## Slide 29: The Rise of Adversarial AI
**Module 9: Adversarial AI in Autonomous Vehicles**

**On-Slide Content (Bullet points for the screen):**
- Machine Learning as the brain of autonomous cars
- What is an adversarial attack?
- Tricking AI without touching the code

**Speaker Notes / Deep Dive Info:**
- As we shift towards fully autonomous vehicles, the AI algorithms (like Computer Vision) become the "brain" of the car. But AI has a unique vulnerability: Adversarial Attacks. This is where a hacker doesn't hack the software code itself, but instead manipulates the physical environment to completely trick the AI's sensors. Because AI sees the world in pixels and mathematical weights, a hacker can introduce tiny, calculated visual noises that are invisible to a human but look like a completely different object to an AI.

---

## Slide 30: Stop Sign Modification and Sensor Spoofing
**Module 9: Adversarial AI in Autonomous Vehicles**

**On-Slide Content (Bullet points for the screen):**
- The famous Stop Sign attack
- Phantom objects and projected images
- LiDAR and Radar spoofing

**Speaker Notes / Deep Dive Info:**
- Let's look at real-world examples of adversarial AI. Security researchers famously demonstrated that by placing a few small, carefully calculated pieces of black and white tape onto a physical Stop sign, human drivers still clearly saw a Stop sign, but the autonomous car's AI classified it as a "Speed Limit 45" sign and accelerated right through the intersection. Another attack involves projecting a momentary image of a pedestrian onto the road for a split second, causing a Tesla's autopilot to violently "phantom brake" to avoid a person that doesn't actually exist. We also see hardware attacks where lasers are fired into LiDAR sensors to make the car hallucinate obstacles.

---

## Slide 31: Data Poisoning and Defenses
**Module 9: Adversarial AI in Autonomous Vehicles**

**On-Slide Content (Bullet points for the screen):**
- Data poisoning during the training phase
- Model evasion at runtime
- Adversarial training and robust AI

**Speaker Notes / Deep Dive Info:**
- Adversarial attacks can happen before the car even leaves the factory. "Data Poisoning" is when a hacker secretly injects bad data into the massive datasets used to train the self-driving AI, essentially teaching the car the wrong rules from birth. To defend against these AI-specific threats, engineers use "Adversarial Training." This means we actively try to trick our own AI in the lab using millions of distorted images, teaching the neural network to ignore the tape on the stop sign and focus on the shape of the sign instead. It is a constant arms race between hackers creating new optical illusions and engineers making the AI more robust.

---

## Slide 32: Introduction to VANETs
**Module 11: VANET Security**

**On-Slide Content (Bullet points for the screen):**
- Vehicular Ad-Hoc Networks (VANETs)
- Self-organizing, infrastructure-less networks
- The wireless backbone of IoV

**Speaker Notes / Deep Dive Info:**
- Before we discuss regulations, we need to understand the wireless foundation that makes IoV possible: VANETs, or Vehicular Ad-Hoc Networks. Unlike traditional networks with fixed routers, VANETs are self-organizing networks where vehicles themselves act as mobile routers. Each car can relay messages to other cars, creating a dynamic mesh network without relying on cellular towers. This is critical for safety applications—if two cars are about to collide, they can't wait for a message to bounce through a cell tower and back. They need direct, millisecond-level communication. VANETs enable this through standards like IEEE 802.11p (DSRC) and now C-V2X, allowing vehicles to communicate directly within a range of 300-1000 meters.

---

## Slide 33: VANET Routing Protocols
**Module 11: VANET Security**

**On-Slide Content (Bullet points for the screen):**
- AODV (Ad-hoc On-Demand Distance Vector)
- DSR (Dynamic Source Routing)
- GPSR (Greedy Perimeter Stateless Routing)

**Speaker Notes / Deep Dive Info:**
- VANETs use specialized routing protocols designed for high mobility. AODV discovers routes on-demand—when a car needs to send a message, it broadcasts a route request, and intermediate nodes build a path dynamically. DSR is similar but stores the entire route in the packet header, allowing intermediate nodes to learn routes passively. GPSR uses geographic positioning—each node forwards packets to whichever neighbor is geographically closest to the destination. The critical security problem is that all these protocols were designed assuming cooperative, honest nodes. They have no built-in authentication or integrity verification. A malicious vehicle can easily lie about its position, advertise fake routes, or simply drop packets it promised to forward.

---

## Slide 34: VANET Attack Vectors
**Module 11: VANET Security**

**On-Slide Content (Bullet points for the screen):**
- Blackhole and Grayhole Attacks
- Wormhole Attack
- Sybil Attack in VANETs

**Speaker Notes / Deep Dive Info:**
- Let's examine the most dangerous VANET attacks. In a Blackhole attack, a malicious vehicle advertises itself as having the shortest route to every destination, attracts all traffic, and silently drops it. A Grayhole is more insidious—the attacker selectively drops certain packets (like emergency braking warnings) while forwarding others, making detection much harder. The Wormhole attack involves two colluding vehicles connected by a private tunnel; they capture packets at one location and replay them at another, completely disrupting distance-based routing calculations. Finally, the Sybil attack is devastating in VANETs—a single malicious vehicle creates hundreds of fake identities, making the road appear congested or creating "ghost vehicles" that outvote legitimate cars in any consensus-based decision.

---

## Slide 35: Position Falsification and GPS Spoofing
**Module 11: VANET Security**

**On-Slide Content (Bullet points for the screen):**
- Lying about geographic position
- Impact on position-based routing
- Real-world GPS spoofing demonstrations

**Speaker Notes / Deep Dive Info:**
- Position falsification is unique to VANETs because so many protocols rely on GPS coordinates. A malicious vehicle can claim to be at a location it's not, completely breaking geographic routing protocols like GPSR. If a car claims it's on a different road, emergency messages might be routed through non-existent paths and lost. Real-world researchers have demonstrated GPS spoofing using software-defined radios costing under $300. They've redirected ships in the Black Sea, fooled Pokémon Go players, and tricked self-driving cars into taking wrong turns. In VANETs, an attacker spoofing their position could make other vehicles think they're about to collide when they're miles apart, or worse, make them ignore an actual imminent collision.

---

## Slide 36: Secure Routing Protocols for VANETs
**Module 11: VANET Security**

**On-Slide Content (Bullet points for the screen):**
- SAODV (Secure AODV)
- ARAN (Authenticated Routing for Ad-hoc Networks)
- SEAD (Secure Efficient Ad-hoc Distance Vector)

**Speaker Notes / Deep Dive Info:**
- To defend against these attacks, researchers have developed secure routing protocols. SAODV adds digital signatures to AODV messages—every route request and reply is cryptographically signed, so malicious nodes can't forge routes. ARAN goes further, using end-to-end authentication where every node in the path must verify its identity with a certificate from a trusted authority. SEAD uses efficient one-way hash chains instead of expensive public-key cryptography, allowing faster verification suitable for high-speed vehicular networks. The challenge is balancing security with performance—adding cryptographic verification to every packet must not introduce latency that makes safety messages arrive too late to be useful.

---

## Slide 37: Trust Management in VANETs
**Module 11: VANET Security**

**On-Slide Content (Bullet points for the screen):**
- Reputation-based trust systems
- Behavioral trust scoring
- Hybrid trust models

**Speaker Notes / Deep Dive Info:**
- Beyond cryptographic security, VANETs need trust management systems. These systems maintain reputation scores for each vehicle based on past behavior. If a car consistently provides accurate traffic information, its trust score increases. If it sends false collision warnings, its score drops. High-trust vehicles have their messages prioritized; low-trust vehicles may be temporarily excluded from routing. Behavioral trust goes deeper, analyzing patterns—a car that accelerates toward pedestrian crossings and ignores traffic signals gets flagged even if it hasn't explicitly lied in its messages. Hybrid models combine cryptographic verification with behavioral trust, creating defense-in-depth. The SCMS system in the US incorporates elements of trust management, allowing misbehavior authorities to revoke certificates from vehicles that demonstrate malicious behavior.

---

## Slide 38: Defending VANETs: Practical Countermeasures
**Module 11: VANET Security**

**On-Slide Content (Bullet points for the screen):**
- Position verification techniques
- Plausibility checks on received messages
- Cooperative misbehavior detection

**Speaker Notes / Deep Dive Info:**
- Practical VANET defense requires multiple layers. Position verification uses techniques like radar cross-checking—if a car claims to be 100 meters away, but my radar shows the nearest vehicle is 500 meters away, that claim is false. Plausibility checks validate message content against physical constraints—a car cannot claim it's traveling 300 mph or that it accelerated from 0 to 60 in 0.5 seconds. If a message violates physics, it's rejected. Cooperative misbehavior detection has multiple vehicles cross-validate each other's messages. If 20 cars see a green light but one claims it's red, the outlier is flagged. This crowdsourced verification makes it extremely difficult for isolated attackers to spread false information, though it requires defending against Sybil attacks where one attacker controls many fake identities.

---

## Slide 39: The Regulatory Landscape
**Module 10: Global Cybersecurity Standards & Regulations**

**On-Slide Content (Bullet points for the screen):**
- Moving from voluntary guidelines to strict law
- Why regulations are necessary for IoV
- Global harmonization of standards

**Speaker Notes / Deep Dive Info:**
- For years, automotive cybersecurity was a wild west, with automakers relying on voluntary best practices. That has completely changed. Because connected cars are moving weapons, governments worldwide realize that cybersecurity can no longer be optional. We are seeing a massive shift from voluntary guidelines to strict, legally binding regulations. Automakers cannot simply promise their cars are safe; they must legally prove they have built secure systems, conducted risk assessments, and have incident response plans ready, or they won't be allowed to sell cars in those countries.

---

## Slide 40: UNECE WP.29 and ISO/SAE 21434
**Module 10: Global Cybersecurity Standards & Regulations**

**On-Slide Content (Bullet points for the screen):**
- UNECE WP.29 Regulation No. 155 (CSMS)
- ISO/SAE 21434 Standard: Security by Design
- Mandatory compliance for vehicle type approval

**Speaker Notes / Deep Dive Info:**
- The two most important regulations in the world right now are UNECE WP.29 and ISO/SAE 21434. The United Nations Economic Commission for Europe (UNECE) issued Regulation 155, which is now legally binding in over 60 countries (including the EU and Japan). It legally requires automakers to have a certified Cyber Security Management System (CSMS). If they don't, they are legally banned from selling cars there. To achieve this, automakers follow the ISO/SAE 21434 standard. This standard forces "Security by Design," meaning cybersecurity must be built into the car's engineering process from day one on the drawing board, rather than bolted on as an afterthought.

---

## Slide 41: Data Privacy Laws: GDPR and Connected Cars
**Module 10: Global Cybersecurity Standards & Regulations**

**On-Slide Content (Bullet points for the screen):**
- The car as a data-gathering machine
- GDPR (Europe) and CCPA (California) implications
- Consent, right to be forgotten, and data minimization

**Speaker Notes / Deep Dive Info:**
- It's not just about hacking; it's about privacy law. Connected cars gather biometric data, voice recordings, location histories, and camera footage. Under strict privacy laws like Europe's GDPR (General Data Protection Regulation) or the California Consumer Privacy Act (CCPA), this data belongs to the driver, not the car company. Automakers face massive fines if they do not build systems that allow drivers to easily give consent, request to delete their data (the right to be forgotten), or if the company suffers a data breach. Real-world example: Volkswagen and other manufacturers had to completely overhaul their infotainment user agreements to ensure strict GDPR compliance.

---

## Slide 42: Summary and Future Outlook
**Conclusion**

**On-Slide Content (Bullet points for the screen):**
- IoV transforms mobility but introduces critical risks
- Multi-layered security: cryptography, IDS, blockchain, and VANET defenses
- Securing wireless ad-hoc networks is fundamental to IoV safety
- The future: 6G, Autonomous AI, and Regulatory Compliance

**Speaker Notes / Deep Dive Info:**
- To wrap up, the Internet of Vehicles represents a monumental leap in transportation. However, transforming a car into a networked computer introduces life-critical risks. Securing this ecosystem requires a multi-layered approach: lightweight cryptography, robust identity management, advanced AI-driven Intrusion Detection Systems, and critically, secure wireless ad-hoc networking. VANETs form the communication backbone of IoV, and their unique vulnerabilities—from blackhole attacks to position falsification—demand specialized secure routing protocols and trust management systems. Moving forward, the integration of Blockchain for trust and the defense against Adversarial AI will be paramount. With strict global regulations like WP.29 now in full effect, automotive cybersecurity is no longer just a technical challenge; it is a fundamental pillar of the future of global mobility.

---

## Slide 43: Q&A Session
**Conclusion**

**On-Slide Content (Bullet points for the screen):**
- Questions?
- Thank you for your time and attention.

**Speaker Notes / Deep Dive Info:**
- Thank you all for your time and attention today as we navigated the complex world of IoV cybersecurity. We've covered a massive amount of ground, from the basic architecture and routing challenges to complex cryptographic defenses, VANET security protocols, AI vulnerabilities, and global regulations. I'd now like to open the floor to any questions you might have. Whether you want to dive deeper into how secure routing protocols defend against blackhole attacks, discuss VANET trust management systems, or explore specific adversarial AI attacks like the Stop sign hack, I'm happy to elaborate. Are there any questions?
