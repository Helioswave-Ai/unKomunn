# unKomunn v1.0 — Sovereign Town Square & Identity Citadel

**unKomunn** is a self-hosted, zero-telemetry communication platform and identity architecture built on Matrix (Synapse) and a customized Cinny web client. Engineered by **HeliosWave / ThinkSpace AI Works** ([helioswave.dev](https://helioswave.dev) | [helioswave.com.au](https://helioswave.com.au)), unKomunn transforms standard federated chat into a self-contained **Digital Third Place**—complete with cryptographic multi-handle identity vaults, anti-farming account credit gates, RAM-only ephemeral messaging, and a dynamic **45° Isometric Stronghold** interface that visually reacts to your server's security posture.

---

## Core Capabilities (v1.0)

* **State-Reactive 45° Isometric Stronghold UI:** Both the browser favicon and the application branding dynamically reflect the live state of the server's Gatekeeper:
  * **Open Town Square Mode (Drawbridges Lowered):** Purple-bordered stronghold with lowered drawbridges spanning the moat toward the central hearth. New community members can roll handles and enter the public square automatically.
  * **Private Citadel Mode (Drawbridges Raised):** Red-bordered stronghold with drawbridges raised into locked portcullis gates. Locks down public registration and restricts entry strictly to pre-authorized single-use cryptographic invite codes or existing citizens.
* **1-Account-Per-Server Anti-Farming Gate (`alt_credits`):** Prevents sybil attacks and automated account farming. Each citizen defaults to `0` Alt-Credits (`1` primary identity). Creating an additional persona requires an explicit credit grant from a Founder or Admin via the **Users & Roles** panel, which is automatically consumed upon enrollment.
* **Sovereign Identity Vault & 1-Click Persona Switcher:** Local AES-encrypted identity vault (`PBKDF2-HMAC-SHA256`) allowing users to store, back up, restore, and switch between multiple pseudonymous handles (`adjective-noun-4digit`) without exposing their underlying credentials.
* **Immutable Login Handles vs. Custom Room Nicknames:** Enrollment permanently binds your rolled `adjective-noun-4digit` handle as your immutable Matrix ID and login credential—eliminating login collisions—while allowing you to freely set a casual Display Name / Nickname inside **Settings -> Account** for everyday chat.
* **RAM-Only Ephemeral Direct Messaging:** Supports volatile, memory-only DMs (500-character limit, 15 messages/minute rate limit, 5-minute TTL, zero database storage).
* **Cross-Platform Local Viewer Portal (2-Part Security):** Lightweight, zero-dependency multi-threaded Python gateway (`unkomunn_portal.py`) for **Windows, macOS, and Linux** that executes locally verified UI code with strict `Content-Security-Policy` headers and handles self-signed LAN/Mesh TLS certificates transparently on `http://127.0.0.1:8766`.

---

## Operations & Administration Handbook

### 1. OPSEC & Addressing Protection (Anti-Doxing & Network Modes)
Sovereignty means self-responsibility: **never port-forward a raw home residential IP directly to strangers.**
* **[Option A] Encrypted Mesh (Tailscale / ZeroTier — Recommended):** Zero router port-forwarding. Bind to your `100.x.y.z` mesh IP. Home IP stays 100% hidden.
* **[Option B] Outbound Tunnel / Cloud VPS Relay:** Route a domain through Cloudflare Tunnel or a WireGuard VPS relay. Home IP stays 100% hidden.
* **[Option C] Public Domain / DDNS:** Point a domain or DuckDNS hostname to your router and forward TCP `80, 443, 7881` & UDP `50100-50200` (LiveKit WebRTC). Caddy automatically provisions TLS.
* **[Option D] Local LAN Mode:** Bound to your local LAN IP with internal local CA TLS for private on-premise networks.

### 2. Users & Roles (`Users & Roles` Modal)
Accessible to **Founders (PL 100)** and **Admins (PL 50)** via the top `● unKomunn ▾` status pill:
* **Server Access Gatekeeper:** Switch instantly between **Open Town Square** and **Private Citadel** modes, or generate single-use **Citadel Invite Codes** for prospective members.
* **Role Hierarchy:**
  * `Founder (PL 100)` / `Admin (PL 50)`: Full server authority—Gatekeeper switching, room/space creation and deletion, user provisioning, role promotion, Alt-Credit grants, ModBot room toggles, and Hotlist editing.
  * `Moderator (PL 25–50)`: Room moderation + top-bar access to ModBot & Hotlist controls.
  * `Member (PL 0)`: Full text, voice, video, screen-sharing, and RAM-Only Ephemeral DMs.
* **Alt-Credit Allocation (`+1` / `-1`):** Grant or revoke Alt-Credits for specific users to authorize legitimate secondary accounts while blocking unauthorized alt creation.

### 3. Sovereign Identity Vault & Nickname Rules (`Identity Vault` Modal)
* **Roll New Handle:** Generates a fresh pseudonymous identity (`e.g., cosmic-saber-5698`) on the active server (subject to Gatekeeper mode and available Alt-Credits) and binds it to your local vault.
* **Login Credential vs. Casual Room Nickname:** Your full rolled `adjective-noun-4digit` handle is your permanent login username and underlying Matrix ID (`@adjective-noun-4digit:server`). Once logged in, you can freely change your casual room **Display Name** in **Settings -> Account** (e.g., shortening `cosmic-saber-5698` to `cosmic`). Logging in always requires your full rolled handle, and clicking any user's profile card in chat verifies their permanent underlying identity.
* **1-Click Switch:** Instantly swaps active Matrix sessions and reloads the workspace under the selected persona.
* **Encrypted Export / Import (`.vault`):** Back up your entire multi-account keyring into an encrypted file protected by your master passphrase for portable recovery across workstations.

### 4. Rooms & Spaces (Add, Configure & Delete)
* **Public Town Square (`#unKomunn`):** Every newly enrolled citizen is automatically joined to the primary Town Square space and lobby by the ModBot daemon.
* **Creating Rooms & Private Channels:** Use `+ Create Room` in the left sidebar to establish public topic rooms or encrypted private channels.
* **Deleting / Purging Rooms:** Founders and Admins can permanently purge rooms and clean up room state via **Delete Rooms / Spaces** in the top `● unKomunn ▾` menu.

### 5. Multi-Server Switching (`Servers` Modal)
* Click the top **`● unKomunn ▾`** pill and select **Servers** (or edit `servers.json`) to switch between local LAN strongholds, Tailscale/ZeroTier mesh citadels, or public community nodes from a single Viewer Portal install.

---

## Quick Start

### Client Viewer Portal (`unKomunn-Portal-v1.0.zip` — Windows / macOS / Linux)
Requires **Python 3.9+** (zero external `pip` packages required).
1. Download and extract `unKomunn-Portal-v1.0.zip` from the [Releases](../../releases) page.
2. Edit `servers.json` and replace `YOUR_SERVER_IP` with your server's LAN, Mesh (`100.x.y.z`), or domain address.
3. Launch the portal:
   * **Windows:** Double-click `Start-unKomunn.bat` (or run `python unkomunn_portal.py`)
   * **macOS:** Double-click `Start-unKomunn.command`
   * **Linux:** Run `./start-unkomunn.sh`
4. Open `http://127.0.0.1:8766` in your browser.

### Server Stack Deployment (`unKomunn-Server-v1.0.tar.gz` — Linux + Docker Compose)
1. Download and extract `unKomunn-Server-v1.0.tar.gz` to `/opt/unkomunn` on your Linux host.
2. Replace `YOUR_SERVER_IP` and `YOUR_SYNAPSE_ADMIN_TOKEN` in `modbot_daemon.py`, `config/caddy/Caddyfile`, and `config/cinny/index.html` with your host IP and Synapse admin token.
3. Start the Docker Compose stack (`docker compose up -d`) and enable the `unkomunn-modbot.service` systemd unit. All containers run with `restart: unless-stopped` for automatic power-loss recovery.

---

## Support & Contact

**unKomunn** is independently designed and maintained by **HeliosWave / ThinkSpace AI Works**. If this stack powers your community, workshop, or private network, you can support ongoing development here:

* **Support Development:** [Ko-fi (HeliosWave)](https://ko-fi.com/helioswave)
* **Web:** [helioswave.dev](https://helioswave.dev) | [helioswave.com.au](https://helioswave.com.au)
* **Contact:** `contact@helioswave.dev`

---

## Credits, License & Acceptable Use Notice

* **Upstream Attribution:** unKomunn's web interface is **Powered by [Cinny](https://cinny.in)** (`v4.12.8`), an open-source Matrix client developed by Ajay Bura and contributors under the MIT License. Matrix homeserver powered by [Synapse](https://github.com/element-hq/synapse) (The Matrix.org Foundation C.I.C.).
* **License:** Distributed under the **MIT License** (see `LICENSE` for full warranty disclaimer and limitation of liability).
* **Sovereign Deployment Notice:** unKomunn is provided strictly as a self-hosted software architecture for lawful private, community, and enterprise communication. Operators deploying a homeserver are solely responsible for their own network security, cryptographic key backups, and compliance with applicable local laws.
