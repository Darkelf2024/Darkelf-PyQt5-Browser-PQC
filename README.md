# Darkelf Browser — PyQt5 Variants

**Darkelf** is a small, hardened, privacy‑first web browser built on Qt WebEngine.  
This README describes the **PyQt5** editions included in this repo and how to run them.

> Looking for the ML‑KEM 768 experimental build? That's the **PySide6** edition; see the separate note at the end.

---

## Variants (PyQt5)

| Variant | File | What it is |
|---|---|---|
| **Darkelf Vault Browser** | `Darkelf Vault Browser.py` | The “everyday hardened” build: tracker blocking, HTTPS‑only, fingerprinting defenses, Tor/SOCKS proxy support, secure downloads. |
| **Darkelf Vault TL Edition** | `Darkelf Vault TL Edition.py` | Adds **TLS/Network tooling** on top of Vault Browser: live certificate hash/monitoring, client‑TLS fingerprint control via `tls-client`, and additional DNS (DoH/DoT) resolvers. |

### Feature highlights

- **Hardened Qt WebEngine config**
  - Many risky surfaces disabled by default via `QTWEBENGINE_CHROMIUM_FLAGS` (WebRTC, WebGL/3D APIs, some HTTP/2, etc.).
  - HTTPS‑only upgrade on navigation.
  - Per‑profile storage: cookies, cache and history under an isolated `QWebEngineProfile`.
- **Tracking & fingerprinting defenses**
  - Rule‑based request blocker (simple ad/tracker lists).
  - Anti‑fingerprinting helpers: canvas spoofing, letterboxing, WebRTC IP leak blocking, supercookie/ETag/cache busting, referrer trimming.
- **Networking**
  - **Tor/SOCKS5** support (e.g. `127.0.0.1:9050`/`9052`) and PAC file support.
  - Optional **DoH/DoT** DNS resolvers (Cloudflare endpoints), with a pure‑Python fallback.
- **Crypto tools (local, not TLS)**  
  - `cryptography` + `PyNaCl` for file/clipboard utilities, scrypt‑based key derivation, and EXIF stripping for media.
  - Optional **post‑quantum** primitives via **Open Quantum Safe (`oqs`)** — used for local key exchange and demo JS bridge (see below).
- **Privacy helpers**
  - Download manager with hash display, clipboard scrub, EXIF/metadata remover for images/PDFs.
- **UI**
  - Minimal dark theme with green accents, multi‑tab interface, basic shortcuts: **Ctrl+T** (new tab), **Ctrl+W** (close), **Ctrl+R** (reload), **Ctrl+H** (history).

> Note: exact features differ slightly between the two PyQt5 files; TL Edition includes extra TLS/DNS tooling and a live certificate monitor (`get_cert_hash`, `start_tls_monitor`).

---

## Quick start

### 1) Requirements

- Python **3.11+** recommended
- **PyQt5** and **PyQtWebEngine**
- Other runtime deps used by the PyQt5 variants:

```bash
pip install PyQt5 PyQtWebEngine cryptography pynacl pillow piexif \
            httpx pysocks beautifulsoup4 PyPDF2
```

**Optional (enables extra features):**

```bash
pip install oqs  # Open Quantum Safe bindings (post‑quantum demos)
pip install tls-client  # TLS client fingerprint tooling (TL Edition)
```

> On Apple Silicon you may need `brew install openssl` and set `CPPFLAGS`/`LDFLAGS` when compiling crypto packages.

### 2) Run

```bash
# Vault Browser (hardened default)
python "Darkelf Vault Browser.py"

# Vault TL Edition (adds TLS/DNS tools)
python "Darkelf Vault TL Edition.py"
```

The address bar defaults to a private search (DuckDuckGo). You can paste any URL or use the search box on the start page.

---

## Configuration

### Environment flags (already set in code)
Both PyQt5 variants set **Chromium** flags through `QTWEBENGINE_CHROMIUM_FLAGS`, e.g.:

- `--disable-webrtc --disable-webgl --disable-3d-apis`
- `--force-webrtc-ip-handling-policy=disable_non_proxied_udp`
- GPU/Canvas hardening and other privacy toggles

Adjust these in the `main()` function if you need different defaults.

### Proxies and Tor
In **Settings → Security** (or via code hooks), you can switch the app to use a **SOCKS5** proxy. Typical Tor endpoints are `127.0.0.1:9050` or `127.0.0.1:9052`. PAC files are also supported.

### DNS (DoH/DoT)
TL Edition ships with workers for **DNS‑over‑HTTPS** (`https://cloudflare-dns.com/dns-query`) and **DNS‑over‑TLS** (`1.1.1.1:853`). You can change these hosts in the source if you prefer other resolvers.

### Certificate monitor (TL Edition)
The title bar/status shows a live **SHA‑256** of the site’s leaf certificate. You can use this for quick pinning or debugging bad chains during testing.

### Post‑quantum demo bridge
Both PyQt5 builds expose a small **QWebChannel** object (`darkelfCrypto`) that lets a page call local crypto helpers (e.g., **ML‑KEM/Kyber** keypair + hybrid encrypt). This is **local JS ↔ Python** crypto for demonstrations; it is **not** a TLS replacement.

---

## Theming & macOS note

If you see a **light strip** behind the tabs on macOS, make sure the pane is painted:

```python
self.tab_widget.setStyleSheet(\"\"\"
    QTabWidget { background: #0b0f14; }
    QTabWidget::pane { background: #0b0f14; border: 0; }
    QTabBar { background: #0b0f14; }
\"\"\")
bar = self.tab_widget.tabBar()
if hasattr(bar, "setDrawBase"):
    bar.setDrawBase(False)
bar.setAttribute(Qt.WA_StyledBackground, True)
```

---

## Security model & caveats

Darkelf aims to **reduce attack surface** and **limit tracking**, but it is **not a drop‑in replacement for Tor Browser or a dedicated hardened OS**. Features like request blocking/fingerprinting protection are best‑effort and may not defeat determined, targeted tracking.

- PQC features are **experimental** and for **research/demo** use only.
- Always review the code and compile your own binaries if distributing.
- See the **export compliance** notice in the source headers.

---

## FAQ

**Q: Why PyQt5 and not PySide6?**  
The Vault variants target **PyQt5** for maximum compatibility across Linux/Windows/macOS. The separate **ML‑KEM 768 Edition** uses **PySide6** to exercise newer Qt APIs.

**Q: Where is browsing data stored?**  
Each build uses a custom `QWebEngineProfile` directory (cookies/cache/history). Clear it from **History** or remove the profile folder if you need a cold start.

**Q: Can I change the default search?**  
Yes—edit the start‑page template or the URL formatter in the main window class.

---

## Contributing

Pull requests are welcome. Please keep changes focused and include a short description and testing notes. For larger changes (new mitigations, proxy modes, platform quirks), open an issue first.

---

## License

The source is licensed under **LGPL‑3.0‑or‑later** (see file headers). The repository may include third‑party components under their respective licenses.

---

## Related: PySide6 ML‑KEM 768 Edition

The repository also contains a **PySide6** variant that demonstrates local **ML‑KEM‑768** key exchange and a JS crypto bridge. If you’re only interested in PyQt5, you can ignore it; otherwise see `Darkelf ML-KEM 768 Edition*.py` for the experimental build.
