# Automotive CAN Security Demo – Data Flow

🔹 High-level Architecture
```
+-----------------+        +--------------------+        +--------------------+
|  ECU Simulator  | -----> |  Secure CAN Layer  | -----> | Android Dashboard  |
|  (C++ binary)   |        |  (C++: HMAC/Auth)  |        |  (Java/Kotlin UI)  |
+-----------------+        +--------------------+        +--------------------+
	|                           |                           |
	|  Generates CAN Frames     | Adds HMAC, checks replay  | Visualizes msgs
	|  (Speed, RPM, etc.)       | Rejects tampered frames   | Graphs, Logs
	|                           |                           |
```

🔹 Data Flow

**ECU Simulator (C++ in automotive/simulator/)**

Simulates CAN messages (e.g., speed, RPM, steering angle).

Example frame:

ID=0x101, DATA=[0x12, 0x34], TS=123456

**Secure CAN Layer (C++ – part of simulator lib)**

Wraps messages with HMAC(tag) using a shared key.
Appends monotonic counter for replay protection.

Record format:

{ FrameID | Payload | Counter | HMAC }

**Communication to Android Dashboard**

Simulator sends frames via TCP/UDP socket or local pipe.
Android app receives them using a lightweight socket client (Java NIO or Kotlin coroutines).

**Android Dashboard (Java/Kotlin in automotive/android_dashboard/)**

Verifies HMAC (optional JNI bridge or re-implemented in Java).
Checks Counter (rejects replayed frames).

Displays:

✅ Valid frames → Graphs (Speed over time, RPM, etc.)
⚠️ Tampered/replayed frames → Red warning in UI

🔹 Example Session

[ECU Simulator]  ->  Frame: ID=0x101, Speed=80km/h, Counter=42
[Secure CAN]     ->  Adds HMAC=abcd1234
[Android Dash]   ->  Displays "Speed = 80 km/h (Valid ✅)"

[Attacker Injects] -> Frame: ID=0x101, Speed=200km/h, Counter=40
[Secure CAN]       -> HMAC invalid or replay detected
[Android Dash]     -> Displays "⚠️ Attack Detected! Replayed/Tampered"

automotive/

🔹 Complete Folder Structure

automotive/
├── simulator/                  # C++: ECU + HMAC + Replay logic
│   ├── include/                # C++ headers (CAN, HMAC, replay)
│   ├── src/                    # C++ source files
│   ├── tests/                  # Unit and integration tests
│   ├── CMakeLists.txt          # Build config for simulator
│   └── README.md               # Module documentation
│
└── android_dashboard/          # Android UI: visualization + logs
	├── app/                    # Main Android app source (Java/Kotlin)
	│   ├── src/                # App source code
	│   ├── res/                # Layouts, drawables, etc.
	│   └── AndroidManifest.xml # Manifest file
	├── build.gradle            # Gradle build config
	├── settings.gradle         # Gradle settings
	├── local.properties        # Local config
	├── gradle/                 # Gradle wrapper files
	├── .idea/                  # IDE configs (optional)
	├── .gitignore              # Ignore rules
	└── README.md               # Dashboard documentation
