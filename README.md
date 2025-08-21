# 🔐 Secure Systems Playground  

A portfolio of security-focused systems demos combining **C++**, **Android**, and **protocol design**.  
This project explores **custom cryptographic protocols, secure messaging, and automotive CAN bus security** — all under one cohesive playground.  

---

## 📌 Architecture Overview  

```
+-----------------------------------------------------------+
| Secure Systems Playground |
| |
| A portfolio of security-focused systems demos combining |
| C++, Android, and protocol design. |
+-----------------------------------------------------------+
```

```
┌─────────────────────┐     ┌─────────────────────┐
│  Protocol Module    │     │   Messenger Module  │
│  (C++ Library)      │     │  (Android + JNI)    │
│                     │     │                     │
│ • Hybrid TLS-like   │     │ • Uses Protocol lib │
│   handshake (RSA/   │<--->│ • Secure chat demo  │
│   ECDH + AES-GCM)   │     │   between clients   │
│ • Session keys      │     │ • End-to-end        │
│ • Rekeying logic    │     │   encryption        │
└─────────────────────┘     └─────────────────────┘


┌─────────────────────┐     ┌─────────────────────────┐
│  Automotive Module  │     │ Android Dashboard       │
│  (C++ Simulator)    │     │  (Java/Kotlin UI)       │
│                     │     │                         │
│ • CAN bus frames    │     │ • Connects via socket   │
│ • HMAC auth         │---->│ • Visualizes CAN data   │
│ • Replay protection │     │ • Alerts on tampering   │
│ • Attacker scripts  │     │ • Charts/logs/messages  │
└─────────────────────┘     └─────────────────────────┘


┌─────────────────────┐
│   Docs & Demos      │
│                     │
│ • Architecture docs │
│ • Data flow charts  │
│ • Build/run guides  │
│ • Testing notes     │
└─────────────────────┘
```

---

## 📂 Repository Structure  

SecureSystemsPlayground/
│
├── protocol/ # Core TLS-like handshake in C++
│
├── messenger/ # Android secure messenger (JNI + C++)
│
├── automotive/
│ ├── simulator/ # C++ CAN bus + security layer
│ └── android_dashboard/ # Android app for visualization
│
└── docs/ # Architecture diagrams, notes, testing

---

## � Modules  

### 🔑 Protocol (C++)
- Hybrid TLS-like handshake (X25519/ECDH → HKDF-SHA256 → AES-GCM).  
- Rekeying and transcript authentication.  
- Exposed as a native C++ library with `encrypt()` / `decrypt()` API.  

### 💬 Messenger (Android + JNI)
- End-to-end secure chat app.  
- Integrates `protocol/` library via JNI.  
- Identity keys stored in **Android Keystore**.  
- Demonstrates cross-platform protocol glue.  

### 🚗 Automotive Security
- **Simulator (C++):**  
	- ECU generates CAN frames with HMAC + replay protection.  
	- Includes attacker scripts (tampering/replay).  

- **Dashboard (Android):**  
	- Visualizes CAN data (speed, RPM).  
	- Warns on attacks.  
	- Real-time charts & logs.  
