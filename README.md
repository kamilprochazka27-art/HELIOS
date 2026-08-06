HELIOS Project Documentation 
(Mint / MkDocs Structure)

Live environment and simulation execution 

Githib link:
https://github.com/kamilprochazka27-art/HELIOS

Google COLAB link: https://colab.research.google.com/drive/17gTTf9Py_ixBQdhK3wcw7okoRFcQ-IJn?usp=sharing#scrollTo=6ZQycun37SYH&uniqifier=2

-

# HELIOS: Unified Multi-Tier Continuum & Autonomous Air-Space Simulation Core

<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10%252b-blue.svg)](https://www.python.org/)
[![NumPy Performance](https://img.shields.io/badge/optimized-NumPy-orange.svg)](https://numpy.org/)
[![Google Colab Ready](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

*A sovereign, decentralized P2P continuum architecture for multi-tier spatial orchestration — spanning Low Earth Orbit (LEO), stratosphere, conventional aviation, and low-level urban drone mobility (U-space).*

</div>

---

## 🚀 Overview

**HELIOS** is a next-generation, decentralized simulation and operational framework designed to eliminate single points of failure, central ground dispatch bottlenecks, and vulnerable legacy air traffic management infrastructure. 

By unifying spatial hashing, cryptographic determinism, and quantum-safe identities (SH3X), HELIOS provides an autonomous execution core capable of scaling seamlessly across hundreds of thousands of dynamic entities—from SpaceX-scale megaconstellations in orbit to autonomous drone swarms and terrestrial logistics.

---

## 🏛️ Architectural Layers (Multi-Tier Continuum)

The HELIOS engine structures space and airspace into five distinct, hierarchically optimized operational tiers:

┌─────────────────────────────────────────────────────────┐
│                     SPACE TIER                          │  <- LEO Satellites (Full TCA, 1000km grid)
├─────────────────────────────────────────────────────────┤
│                      SKY TIER                           │  <- Aviation / Stratosphere (50km grid)
├─────────────────────────────────────────────────────────┤
│                    DRONES TIER                          │  <- U-space / Local Corridors (10km grid, Pings)
├─────────────────────────────────────────────────────────┤
│                    GROUND TIER                          │  <- Terrestrial Logistics / Autonomous Vehicles
├─────────────────────────────────────────────────────────┤
│                  UNDERGROUND TIER                       │  <- Dense Grid / Infrastructure Protection
└─────────────────────────────────────────────────────────┘

1. **Space Tier (`space`):** Optimized for Low Earth Orbit (LEO) megaconstellations. Implements Time of Closest Approach (TCA) vector forecasting and safety envelope tracking ($0.5\,\text{km}$ safety radius).
2. **Sky Tier (`sky`):** High-altitude stratosphere and conventional commercial aviation routing.
3. **Drones Tier (`drones`):** Low-Level Airspace (LLAS) and urban air mobility (U-space), utilizing lightweight ping-based tracking to minimize computational overhead.
4. **Ground Tier (`ground`):** Terrestrial autonomous vehicles, robotics, and smart infrastructure corridors.
5. **Underground Tier (`underground`):** High-density local utility tunnels, subterranean logistics, and sensitive infrastructure monitoring.

---

## 🛡️ Core Technical Pillars

### 1. Cryptographic Determinism & Priority Resolution (`deterministic_priority`)
Instead of relying on a centralized air traffic controller or ground dispatcher to resolve close encounters, HELIOS utilizes cryptographic hashing (**MD5 / SHA3-256**) to compute deterministic priority scoring (`sid` string parsing). This allows autonomous nodes to resolve conflicts independently and identically across the entire decentralized network.

### 2. Time of Closest Approach (TCA) & Vector Mechanics
Built with high-performance **NumPy** vectorization, the simulation engine calculates relative positions, velocity vectors, and TCA timelines in real-time without blocking execution loops:
```python
def time_of_closest_approach(r: np.ndarray, v: np.ndarray) -> float:
    vr2 = float((v * v).sum())
    if vr2 < 1e-12:
        return 0.0
    t = -float((r * v).sum()) / vr2
    return max(0.0, t)
```
3. Energy & Laser Layer (HeliosEnergyLayer)
​Laser Beaming & Ablation Pressure: Coordinates external orbital and ground-based laser stations to execute precision trajectory adjustments, significantly reducing onboard chemical propellant consumption.
​Energy Profiles (EnergyReceiverProfile): Optimizes energy distribution, routing, and station-keeping across distributed network grids.
​
4. Security, Defense & Biocentric Protocol (HeliosSecurityProtocol)
Biocentric Priority: Hard-coded safety rules ensuring human life and crewed vehicles (e.g., life-critical transport, personal air taxis, Emergency Services / IZS) maintain absolute operational priority.
Consensus Kill-Switch: Defensive or isolation maneuvers require cryptographic verification and consensus from a minimum of 3 independent nodes. Unprovoked threats or aggression instantly trigger node quarantine (quarantined_nodes) and network disconnection.

📊 Performance & Scalability
The NumPy-backed simulation core is designed for heavy workloads, capable of running large-scale scenarios directly inside cloud environments like Google Colab:
Tested Capacity: 30,000+ concurrent orbital objects / dynamic entities.
Stream & Export: Built-in asynchronous JSON WebSocket export (wss://) for real-time visualization on client canvases (such as GitHub Pages or local dashboard UIs).
Logging: Automated generation of event logs (events.csv) and spatial density heatmaps (heatmap_samples.csv).

🛠️ Quick Start (Google Colab Demo)
You can run the full simulation instantly in your browser via Google Colab:
Open the HELIOS LEO Simulator & Unified Demo Colab Notebook.
Adjust configuration parameters (N for object count, STEPS for duration) at the top of the script cell.
Paste and execute the cell to start the NumPy simulation engine and initialize local WebSocket streaming.

```python
# Quick snippet preview from the core config
N = 30000            # Number of simulated entities
STEPS = 60           # Simulation steps
DT = 1.0             # Seconds per step
SAFETY_RADIUS = 0.5  # km safety threshold
```
🌐 Client Integration (GitHub Pages & WebSockets)
To connect the running Colab backend to a live 2D/3D visualization canvas on GitHub Pages:

// Establish secure WebSocket connection to the active Colab tunnel
const ws = new WebSocket('wss://your-tunnel-address.loca.lt');
```JavaScript
ws.onmessage = function(event) {
    const telemetryData = JSON.parse(event.data);
    renderOperationalCanvas(telemetryData);
};
```
HELIOS AI Gateway & Secure Execution Sandbox

Enterprise-grade defense-in-depth security gateway and deterministic validation runtime designed to protect decentralized networks against rogue autonomous AI agents, prompt injection exploits, and unauthorized system commands.

🚀 Key Features
AST Structural Threat Detection: Goes far beyond naive text matching (regex) by parsing code payloads into an Abstract Syntax Tree (AST). This defeats text obfuscation, variable splitting, and string concatenation bypasses.

Isolated Application Sandbox: Safely executes verified payloads within a restricted global scope and a white-listed set of built-in functions, preventing raw system access.

Cryptographic Immutable Audit Ledger: Generates a tamper-evident, SHA3-256 cryptographically chained log of every action, security violation, and quarantine event for comprehensive forensic analysis.

Autonomous Quarantine (Kill-Switch): Automatically isolates and disconnects nodes from the network once a predefined security violation threshold is crossed.

Deterministic Signatures: Produces verifiable transaction/action signatures using SHA3-256 with timestamp verification.

🏛️ Architecture Overview

[ AI Agent Output ] 
       │
       ▼
[ HeliosAIGateway ] ──> (Active Quarantine Check)
       │
       ▼
[ AST Structural Analysis ] ──> (Detects forbidden modules/functions)
       ├─► [ IF DANGEROUS ]  ──> Increment Violation Counter ──> (Quarantine if limit reached)
       └─► [ IF SAFE ]      ──> Cryptographic Audit Log (SHA3-256 Chained)
                                      │
                                      ▼
                            [ HeliosSandbox Execution ] ──> Safe Result / Output

​🛠️ Quick Start

​Here is how to integrate and run the secure gateway in your Python project:  


```python
import json
import logging
from helios_gateway import HeliosAIGateway  # Assuming saved in module

# Initialize the gateway for a specific network node
gateway = HeliosAIGateway(node_id="AI-AGENT-NODE-ALPHA-1", max_violations=3)

# 1. Intercept and validate incoming inputs
try:
    input_result = gateway.intercept_input("Analyze network traffic patterns.")
    print("Input approved:", input_result["signature"])
except Exception as e:
    print("Input blocked:", e)

# 2. Intercept and inspect agent outputs / actions
safe_action = {
    "type": "data_processing",
    "payload": "result = sum([x * 2 for x in range(10)])"
}

output_result = gateway.intercept_output(safe_action)
print("Execution Status:", output_result["status"])
print("Sandbox Output:", output_result.get("sandbox_result"))
```
🛡️ Security Policy & Threat Mitigation

| Threat Vector | Standard Approach | HELIOS Gateway Mitigation |
| :--- | :--- | :--- |
| **Prompt Injection / Jailbreak** | Block keywords via basic blacklists | AST structural analysis evaluates semantics regardless of string formatting |
| **Malicious Code Execution** | Unchecked execution via `exec()` / `eval()` | Forced execution inside `HeliosSandbox` with restricted globals & built-ins |
| **Log Tampering** | Standard text log files | SHA3-256 cryptographically linked audit chain (Blockchain-style immutability) |
| **Rogue Agent Persistence** | Continuous retries after attack | Automatic network-level quarantine (Kill-switch) upon threshold breach |

📜 License
This project is licensed under the MIT License - see the LICENSE file for details.



