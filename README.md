HELIOS Project Documentation 
(Mint / MkDocs Structure)

Live environment and simulation execution 

link: https://colab.research.google.com/drive/17gTTf9Py_ixBQdhK3wcw7okoRFcQ-IJn?usp=sharing#scrollTo=N1hMe0scWROJ
-
Chapter 1: Introduction and Architecture (Air-Space Continuum)

🛰️ HELIOS: Space Traffic Management Digital Twin Engine

HELIOS is a high-performance, modular simulation core designed for Space Traffic Management (STM) and orbital digital twins. It bridges the gap between raw orbital telemetry and proactive, automated collision avoidance by simulating tens of thousands of objects in real-time, executing lookahead conflict detection, and providing a bidirectional streaming interface for modern 3D control centers.

🚀 Key Features
 * Massive Scale Simulation: Efficiently tracks and calculates trajectories for 30,000+ satellites simultaneously using optimized spatial partitioning and vector operations (NumPy).
   
 * Lookahead Collision Detection: Uses spatial cell mapping and Time of Closest Approach (TCA) algorithms to proactively identify orbital crossings and execute deterministic safety maneuvers before risks materialize.
   
 * Bidirectional Real-Time Streaming: Built-in WebSocket server (TelemetryStreamer) that pushes live telemetry frames out to 3D frontends while listening for inbound touch, UI, or voice commands from operators.
   
 * Modular Plug-and-Play Architecture: Clean separation of concerns. The simulation engine runs entirely isolated from visualization or external data pipelines, allowing seamless integration with custom frontends (e.g., CesiumJS, Unreal Engine).
   
 * Timeline & Prediction Mode: Full support for lookahead prediction windows, historical time offsets, and custom HUD metadata for control room displays.
   
 * Operator Profiles & AI Assistant ("Helly"): Configurable operator profiles governing communication tone (professional, casual, sarcastic) and voice synthesis settings, paired with the on-board AI operations companion Helly.
   
🏗️ System Architecture
+-------------------------------------------------------------+
|                  HELIOS Simulation Core                     |
|  - NumPy Vectorized Physics & Position Updates              |
|  - Spatial Grid Conflict Resolver & TCA Lookahead           |
+------------------------------+------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|               Bidirectional Telemetry Streamer              |
|  - Thread-Safe Queue (Non-blocking Engine Integration)      |
|  - WebSocket Server (Outbound JSON Telemetry / Inbound UI)  |
+------------------------------+------------------------------+
                               |
        +----------------------+----------------------+
        | (Optional WebSocket Stream)                 |
        v                                             v
+-------------------------------+             +-------------------------------+
|     3D Control Center UI      |             |     Helly AI Assistant        |
|  - CesiumJS / WebGL Glub      |             |  - Operator Tone Customization|
|  - Touch / Tablet Commands    |             |  - Voice TTS Profiles         |
+-------------------------------+             +-------------------------------+

📦 Requirements

 * Python 3.10+
 * NumPy (for vectorized orbital math)
 * Websockets (optional, required only if enabling the real-time stream)
Install the required dependencies via pip:
pip install numpy websockets

⚙️ Quick Start

 * Clone or download the repository containing helios_simulator.py.
 * Configure your run parameters directly in the configuration block of the script:
   
   N = 30000              # Number of simulated satellites
STEPS = 100            # Total simulation steps
ENABLE_VISUALIZATION = True
STREAM_PORT = 8766     # WebSocket port for external 3D clients

 * Execute the simulation:
   python helios_simulator.py

🛠️ Configuration & Customization

Operator Profiles & AI Tone
You can configure the active operator and Helly's behavioral response profile at the runner section of the script:
active_operator = OperatorProfile(
    operator_id="OP-KPROCHAZKA", 
    tone="professional",  # Options: "professional", "casual", "sarcastic"
    voice_id="voice_space_neutral_01"
)

Output Files
 * events.csv: Detailed audit log of every resolved orbital conflict, including TCA, minimum distance, and coordinate deltas before/after maneuver correction.
   
 * heatmap_samples.csv: Spatial density tracking data for orbital corridor analysis.
   
🔌 Connecting an External 3D Frontend
To plug in a real-time visualization client (such as a CesiumJS web app or a custom control-room dashboard):

 * Set ENABLE_VISUALIZATION = True and specify your desired STREAM_PORT.
   
 * Connect your client via WebSocket: ws://localhost:8767.
  
 * The server broadcasts structured JSON frames containing step counters, prediction metadata, simulation timestamps, and satellite coordinates (x, y, z).
   
 * Send inbound JSON commands back to control state (e.g., pause, timeline shifting, or satellite focus):
   {
  "action": "focus_satellite",
  "sat_id": "SAT-00123"
}

Chapter 2: Quantum-Safe Encryption and Identity (SH3X)

 * Goal: Secure all communication and node identities (satellites, drones, ground stations) against future quantum computer attacks (protection against Shor's algorithm).
   
 * Technical Specification:
   
   * Use of SHA3-256 and SHA3-512 hashing primitives.
   * Each node has its own SH3XContext class with sovereign public key generation and message signing.
 * Clean Usage Example:

# Initialization of quantum-safe context layer for a node
crypto_node = SH3XContext(node_id="SAT-HELIOS-ALPHA")

# Signing a command/message
payload = b"MANEUVER_VECTOR_X: 1.5_DELTA_V"
signature = crypto_node.sign_message(payload)

# Signature verification
is_valid = SH3XContext.verify_signature(crypto_node.public_key, payload, signature)
print(f"Signature Valid: {is_valid}")

Chapter 3: Inter-Agency Federation (HeliosFederationLayer)

 * Goal: Enable space agencies (NASA, ESA, commercial giants like SpaceX, etc.) to manage their own sovereign domains while cooperating fairly and deterministically during close orbital approaches.
   
 * Operating Principle:
   * No agency surrenders control over its assets.
   * In case of conjunction risk, priority is evaluated mathematically via cryptographic hashing of object IDs (SHA3-256), eliminating any disputes or delays.
     
 * Clean Usage Example:
   fed_layer = HeliosFederationLayer()

# Registration of agency domains
nasa_domain = SpaceAgencyDomain("US-NASA", "NASA (USA)")
esa_domain = SpaceAgencyDomain("EU-ESA", "ESA (Europe)")
fed_layer.register_domain(nasa_domain)
fed_layer.register_domain(esa_domain)

# Assigning satellites
fed_layer.assign_satellite("US-NASA", "SAT-US-NASA-ALPHA")
fed_layer.assign_satellite("EU-ESA", "SAT-EU-ESA-ALPHA")

# Resolving conjunction
result = fed_layer.resolve_conjunction(
    sat_a="SAT-US-NASA-ALPHA", 
    sat_b="SAT-EU-ESA-ALPHA", 
    agency_a="US-NASA", 
    agency_b="EU-ESA"
)
print(result)

Chapter 4: Energy and Laser Layer (HeliosEnergyLayer)

 * Goal: Coordinate external maneuvers using ground-based or orbital laser stations (laser beaming / ablation pressure), significantly saving onboard propellant consumption.
   
 * Key Classes: EnergyReceiverProfile, LaserBeamStation, HeliosEnergyLayer.
   
 * Clean Usage Example:
   import numpy as np

energy_layer = HeliosEnergyLayer()

# Registration of satellite receiver profile for external energy
receiver = EnergyReceiverProfile(
    sat_id="SAT-US-NASA-ALPHA", 
    has_laser_receiver=True, 
    has_ablation_surface=False, 
    max_safe_power_flux=50.0
)
energy_layer.register_receiver(receiver)

# Triggering external impulse for evasion maneuver
mission = energy_layer.trigger_external_impulse(
    sat_id="SAT-US-NASA-ALPHA",
    target_vector=np.array([0.1, 0.0, 0.0]),
    delta_v=1.5
)
print(mission)

Chapter 5: Security and Defense Protocol (HeliosSecurityProtocol)

 * Goal: Hard-coded safety rules protecting human life, prioritizing emergency services (IZS), and enforcing zero tolerance toward attacks or unauthorized aggression.
   
 * Main Pillars:
   * Biocentric Priority: Crewed vehicles (is_life_critical = True, e.g., personal air taxis) hold absolute priority.
   * Emergency Services Priority: IZS and strategic assets take precedence over regular traffic.
   * Defense Consensus and Kill-Switch: Any defensive action requires cryptographic verification and consensus from at least 3 independent nodes. Unprovoked aggression instantly triggers node quarantine (quarantined_nodes) and network disconnection.
     
 * Clean Usage Example:
   security = HeliosSecurityProtocol()

entity_human = {"entity_id": "PAV-TAXI-01", "is_life_critical": True, "is_izs_priority": False}
entity_cargo = {"entity_id": "DRONE-CARGO-99", "is_life_critical": False, "is_izs_priority": False}

# Priority evaluation (Human life wins)
priority_res = security.evaluate_priority_and_safety(entity_human, entity_cargo)
print(priority_res) # Output: PRIORITY_LIFE_PAV-TAXI-01

Chapter 6: Complete Unified Simulation and Google Colab

 * Goal: Run all the above layers together in a single script and verify massive scalability (e.g., testing 150,000 objects over minutes).
   
 * Where to find and run code:
   Open the attached Google Colab Notebook to find functional scripts ready for immediate execution in the cloud.
   
