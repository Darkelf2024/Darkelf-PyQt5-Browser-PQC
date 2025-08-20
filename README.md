# Darkelf Vault Browser — PyQt5 (v3)

**Darkelf** is a **fully hardened, privacy-first browser** built on Qt WebEngine.  
It is designed as a **drop-in replacement for Tor Browser**, combining strong fingerprinting defenses, Tor/SOCKS proxy routing, post-quantum crypto, and even **OS-level hardening tools**.  

This README documents the **PyQt5 Vault Browser** editions included here.

---

## Variants (PyQt5)

| Variant | File | What it is |
|---|---|---|
| **Darkelf Vault Browser** | `Darkelf Vault Browser.py` | Hardened default build: tracker blocking, HTTPS-only, fingerprinting defenses, Tor/SOCKS proxy support, secure downloads, kernel monitors. |
| **Darkelf Vault TL Edition** | `Darkelf Vault TL Edition.py` | Adds advanced **TLS/Network tooling**: live certificate SHA-256 monitor, TLS fingerprint control (`tls-client`), extra DNS resolvers (DoH/DoT). |

---

## 🔥 Darkelf vs Tor Browser

| Feature | **Darkelf Vault Browser v3** | **Tor Browser** |
|---------|-------------------------------|-----------------|
| **Network routing** | SOCKS5/Tor proxy support (127.0.0.1:9050/9052). PAC file support. | Tor network routing by default. |
| **TLS stack** | TLS 1.3 enforced by default. Live SHA-256 certificate monitoring (TL Edition). | Standard Firefox TLS stack. |
| **DNS** | DNS-over-HTTPS (DoH) & DNS-over-TLS (DoT) resolvers included (configurable). | Uses Tor exit node resolvers. |
| **Fingerprinting defenses** | Over 40+ injected JS stealth APIs: canvas, audio, WebRTC, plugins, timezone, fonts, performance timers, etc. Tor-style letterboxing spoof. | Tor Browser-level defenses (canvas prompt, letterboxing, UA uniformity). |
| **Crypto bridge** | **DarkelfCrypto**: exposes NaCl, Cryptography, and **ML-KEM 768 (Kyber)** PQC to web pages via QWebChannel. Fully functional, not a demo. | None (no local crypto bridge). |
| **Post-quantum support** | ✅ **Fully working ML-KEM 768 (Kyber) via Open Quantum Safe.** | ❌ Not included. |
| **Kernel/system hardening** | DarkelfKernelMonitor: disables dynamic_pager, wipes swapfiles, monitors kernel config. | ❌ Not included. |
| **Platform** | PyQt5 (Linux, Windows, macOS). | Firefox ESR (all major OS). |
| **Default search** | DuckDuckGo private search (configurable). | DuckDuckGo onion / local search box. |

👉 **Summary**: Darkelf matches Tor Browser in anonymity features, **adds post-quantum crypto, TLS/DNS tools, and OS-level hardening.**  

---

## 🚀 Recommended Use Cases

Darkelf is designed for **users who need more than Tor Browser provides**, with particular focus on **post-quantum security, hardened environments, and forensic safety**.  

- **Everyday hardened browsing**  
  Use Darkelf Vault as your default secure browser: tracker blocking, HTTPS-only, stealth APIs, and proxy/Tor support.  

- **Post-quantum research & testing**  
  The **DarkelfCrypto bridge** with **ML-KEM 768 (Kyber)** lets developers and researchers experiment with PQC and hybrid key exchange in a real browser environment.  

- **Forensic & investigative workflows**  
  Built-in **certificate hash monitor** and **DNS/TLS tools** help with network analysis, pinning, and debugging malicious chains.  

- **System-level security environments**  
  On macOS/Linux, **DarkelfKernelMonitor** ensures swap/paging is controlled, preventing memory persistence and reducing side-channel leakage.  

- **Educational use**  
  As a teaching tool, Darkelf demonstrates hardened browser design, cryptographic bridges, and PQC integration in an accessible Python/Qt codebase.  

---

## Feature Highlights

### 🔒 Hardened Web Engine
- Chromium surfaces disabled via `QTWEBENGINE_CHROMIUM_FLAGS`.  
- HTTPS-only upgrade enforced.  
- Isolated **per-profile storage**.  

### 🕵️ Fingerprinting & Tracking Defenses
- **Massive anti-fingerprinting layer** with >40 injected overrides.  
- Tor-style letterboxing.  
- Request/ad/tracker blocking.  
- ETag/cache/supercookie busting.  

### 🌍 Networking
- Tor/SOCKS5 proxy + PAC support.  
- DoH/DoT resolvers (Cloudflare default).  
- TLS 1.3-only enforced.  
- Certificate SHA-256 live monitor (TL Edition).  

### ⚡ Post-Quantum Crypto — Fully Working
- Integrated **ML-KEM 768 (Kyber)** via **Open Quantum Safe (`oqs`)**.  
- Local key exchange and hybrid encryption through DarkelfCrypto.  

### 🛡️ DarkelfCrypto Bridge
- Exposes crypto primitives to web pages via **QWebChannel**.  
- Fully functional: not a demo.  
- Supports PQC (Kyber), NaCl, file/clipboard tools, scrypt KDF, EXIF stripping.  

### 🖥️ Kernel & System Hardening
- **DarkelfKernelMonitor** detects and disables swap/dynamic_pager.  
- Securely wipes swap files.  
- Monitors kernel fingerprint.  

### 🎨 UI
- Minimal dark theme with green accents.  
- Multi-tab interface, familiar shortcuts.  

---

## Quick Start

### Requirements

```bash
pip install PyQt5 PyQtWebEngine cryptography pynacl pillow piexif             httpx pysocks beautifulsoup4 PyPDF2
```

**Optional (extra features):**
```bash
pip install oqs          # Post-quantum ML-KEM 768 support
pip install tls-client   # TLS fingerprint tooling (TL Edition)
```

### Run

```bash
# Vault Browser (default hardened)
python "Darkelf Vault Browser.py"

# Vault TL Edition (adds TLS/DNS tooling)
python "Darkelf Vault TL Edition.py"
```

---

## Security Model

- **Drop-in replacement for Tor Browser** workflows.  
- PQC support: **fully functional**, not experimental.  
- DarkelfCrypto: **real local crypto bridge**, not demo.  
- System-level defenses beyond Tor Browser.  

---
