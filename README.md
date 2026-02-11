# SchoolAI-MR-JS-Particles

A WebXR mixed-reality particle system for Meta Quest 3 that uses hand tracking to control interactive particle effects in passthrough AR, with a desktop browser fallback.

## Key Features

- **Four particle modes** — Nebula (galaxy-like swirl), Swarm (boid-ish cloud), Burst (explosive randomness), Sculpt (hand-magnet attractor)
- **WebXR hand tracking** — open/close hand to expand/implode particles; pinch (index + thumb) to cycle modes
- **Passthrough mixed reality** — runs as an `immersive-ar` session in Meta Quest Browser with real-world passthrough
- **Desktop fallback** — orbit camera with drag, mouse wheel to control particle tension, click to cycle modes
- **Live UI controls** — color picker, particle count adjustment (+/- buttons), recenter, and mode selection buttons
- **No build step** — single HTML file with inline JS; Three.js loaded from CDN

## Demo / Screenshots

<!-- TODO: Add screenshot or GIF of the particle system running on Quest 3 and/or desktop -->

## Getting Started

### Prerequisites

| Requirement | Version / Notes |
|---|---|
| **Node.js** | v14+ (uses built-in `https`, `fs`, `path` modules) |
| **Modern browser** | Chrome, Edge, or Firefox for desktop fallback |
| **Meta Quest 3** | For mixed-reality mode (Meta Quest Browser) |

No additional npm packages are needed — the server uses only Node.js built-in modules, and the frontend loads [Three.js r128](https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js) from a CDN.

### Installation

```bash
git clone <repo-url>
cd SchoolAI-MR-JS-Particles
```

No `npm install` required.

### Configuration

The project ships with self-signed SSL certificates (`cert.pem` / `key.pem`) for local development. WebXR requires HTTPS, which is why the server uses TLS.

| Variable | Location | Default | Description |
|---|---|---|---|
| `PORT` | `server.js` line 49 | `8443` | HTTPS server port |
| `particleCount` | `index.html` line 166 | `9000` | Initial number of particles |
| `maxParticles` | `index.html` line 167 | `15000` | Upper limit for particle count |
| `minParticles` | `index.html` line 168 | `3000` | Lower limit for particle count |

To regenerate the self-signed certificates:

```bash
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes
```

### Running Locally

```bash
node server.js
```

Open **https://localhost:8443/** in your browser. You will see a security warning because the certificate is self-signed — click **Advanced** then **Proceed to localhost** to continue.

## Usage

### Desktop (any modern browser)

| Action | Effect |
|---|---|
| Drag | Orbit camera |
| Mouse wheel | Expand (scroll up) / implode (scroll down) particles |
| Click | Cycle through modes: Nebula → Swarm → Burst → Sculpt |
| UI buttons | Select mode, adjust color, change particle count |

### Meta Quest 3

1. Ensure **Hand Tracking** is enabled in Quest system settings.
2. Open `https://<your-local-ip>:8443/` in **Meta Quest Browser** (the Quest must be on the same network as the server).
3. Tap **Enter Mixed Reality** to start the `immersive-ar` session with passthrough.

| Gesture | Effect |
|---|---|
| Open hand | Expand particle cloud |
| Close hand | Implode particle cloud |
| Pinch (index + thumb) | Cycle particle mode |
| Point (index finger) in Sculpt mode | Attract particles toward fingertip |

## Project Structure

```
SchoolAI-MR-JS-Particles/
├── index.html   # Main application — Three.js scene, particle system, WebXR, UI (710 lines)
├── server.js    # Node.js HTTPS static file server (55 lines)
├── cert.pem     # Self-signed SSL certificate
├── key.pem      # SSL private key
└── README.md    # This file
```

- **`index.html`** — Contains everything: HTML structure, CSS styles, and all JavaScript (inline `<script>` block). Sections are clearly numbered 1–8 covering configuration, Three.js setup, particle system, UI wiring, WebXR session management, particle dynamics, desktop fallback, and window resize handling.
- **`server.js`** — Minimal HTTPS server that serves static files with proper MIME types. Supports common web assets plus `.glb`/`.gltf` for potential 3D model serving.

## Scripts / Commands

| Command | Description |
|---|---|
| `node server.js` | Start the HTTPS development server on port 8443 |

## Testing

No automated tests found.

## Linting / Formatting

No linting or formatting tools are configured.

## Build / Deployment

There is no build step — the project serves raw HTML/JS directly. For deployment:

- Serve over HTTPS (required for WebXR).
- Ensure the host is accessible from the Quest device on the same network.
- Replace the self-signed certificates with valid ones for production use (e.g., via Let's Encrypt).

No Dockerfile or CI/CD configuration is present.

## Contributing

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/my-change`.
3. Make your changes. Keep the single-file architecture unless there is a strong reason to split.
4. Test on both desktop and Quest 3 if possible.
5. Submit a pull request with a clear description of the changes.

## License

<!-- TODO: Add a LICENSE file to the repository. -->

No license file found. Please add one before distributing.

## Credits / Acknowledgements

- [Three.js](https://threejs.org/) (r128) — 3D rendering engine
- [WebXR Device API](https://immersiveweb.dev/) — Mixed reality and hand tracking
- [WebXR Hand Input API](https://www.w3.org/TR/webxr-hand-input-1/) — Joint-level hand tracking

## Open Questions / TODO

- [ ] **License** — No `LICENSE` file exists. Add one to clarify usage terms.
- [ ] **Screenshots / Demo** — Add a GIF or screenshot showing the particle system on Quest 3 and desktop.
- [ ] **package.json** — Consider adding one for metadata, scripts (`"start": "node server.js"`), and potential future dependencies.
- [ ] **Network access on Quest** — Document how to find the host machine's local IP and any firewall considerations for Quest testing.
- [ ] **Certificate regeneration** — The shipped `cert.pem` / `key.pem` will eventually expire; document the expiry date or automate renewal.
