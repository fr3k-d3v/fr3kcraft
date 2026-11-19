# FR3KCRAFT — agent setup and Minecraft join runbook

**Public onboarding document | Updated 2026-11-19 (AEST)**

This is a **standalone public guide** for a human or automation agent tasked with preparing a Minecraft client and joining the FR3KCRAFT family world. It is **not** the game server, an executable bot, a public invite, or permission to operate infrastructure. The Minecraft control centre and deployment configuration are private and are **not** contained in this repository.

## 0. Truth and current gate

| Item | Evidence/status | What it means |
| --- | --- | --- |
| Server plan | **PLANNED:** one Paper Java server with Geyser + Floodgate for Java/Bedrock cross-play | Java and Bedrock are intended to share one world |
| Server deployment | **OBSERVED 2026-11-19:** deployment bundle staged on Contabo and no listeners observed on the planned Minecraft ports; full container/process state could not be verified | **BLOCKED:** no verified joinable server; running state remains **UNKNOWN** |
| EULA | **REQUIRED:** owner must read and explicitly accept the [Minecraft EULA](https://www.minecraft.net/en-us/eula) before any startup | Agent must not accept on behalf of owner without explicit consent |
| Access | **REQUIRED:** individually approved whitelist entries and a privately supplied, verified server address | Never guess a host, open ports or bypass a whitelist |
| In-game verification | **UNKNOWN:** no verified successful Java or Bedrock join, world gameplay, backup restore, or cross-play test | Client preparation is **not** proof of multiplayer readiness |
| Automated Minecraft player/bot | **NOT IMPLEMENTED/NOT VERIFIED** in this public repo | Do not claim that an AI agent can already log in or move in-world |

**Do not attempt to join or announce FR3KCRAFT as live** until the server operator supplies an approved endpoint and confirms the acceptance checklist in section 7. Never copy private connection details into public issues, commits, screenshots or chat logs.

## 1. Agent operating contract

Use **OBSERVE → VERIFY → RECONCILE → IMPROVE → TEST → RECORD**. Label findings **CLAIM**, **OBSERVATION**, **EVIDENCE**, **VERIFIED FACT**, **HYPOTHESIS**, **SIMULATION**, **UNKNOWN** or **BLOCKED**. Preserve raw local evidence privately; publish only sanitized status.

- **OBSERVE (default):** check OS, architecture, existing game/launcher, version metadata and reported network prerequisites. Do not change machine state, connect, open ports, sign in or install anything.
- **APPROVAL:** ask the operator before installing packages, changing launcher settings, logging into a Microsoft account, adding a server, joining, whitelisting, altering networking, deploying or changing a world.
- **AUTO:** permitted only for previously approved, bounded, reversible actions; authorization must be explicit, scoped and recorded.
- **PHYSICAL:** device interactions, account sign-in and owner EULA decisions may require the human at the machine.

**Forbidden shortcuts:** no credential gathering, token/SSH key sharing, private IPs in public material, authentication bypass, spoofing a paid game licence, disabled SSH host-key checks, arbitrary server console/RCON commands, unexpected firewall changes, or model-issued world mutations. A returned process ID, exit code or successful launcher start is not evidence that a player joined a world.

## 2. Inputs the agent needs from the operator (keep private)

1. **Target device and operating system:** one of the four identifiers below, or an explicitly approved additional device.
2. **Edition:** Java Edition or Minecraft for Windows (Bedrock), with a legitimately licensed Microsoft/Xbox account. Never request the password or session tokens.
3. **Authority scope:** inventory only, launcher installation, client configuration, or a single live join test.
4. **Approved server address:** provided by the operator **after** deployment verification, plus the applicable port and server version/compatibility notice.
5. **Whitelist decision:** operator confirms the player's Java username or Xbox gamertag has been approved and added server-side. Do not post either publicly.

If any prerequisite is missing, return **BLOCKED** or **UNKNOWN** with the next required owner action. Do not silently choose an endpoint.

## 3. Client matrix — one real device per identity

| Device identifier | Machine | Primary edition | Notes |
| --- | --- | --- | --- |
| `x1-linux` | X1 Linux | Java | Local launcher check required |
| `chromebook-omarchy` | Chromebook running Linux/Omarchy | Java via Prism Launcher | Do not assume ChromeOS Android Minecraft is equivalent |
| `windows-laptop` | One Windows laptop | Java **or** Bedrock | Edition choices on one device, not two independent client slots |
| `omarchy-linux` | Omarchy Linux desktop | Java | Separate machine from Chromebook |

**Target version baseline, not a live compatibility guarantee:** the private deployment plan selected Paper **26.3, build 253** using **Java 25**, with Geyser-Spigot and Floodgate-Spigot. Its compatibility inventory dated 2026-11-16 listed Java **26.3** and Bedrock **26.40–26.63**. Check the *currently deployed* Paper/Geyser compatibility before installing/selecting versions; these values can become stale. Java and Bedrock use different protocols and different launcher/account flows.

## 4. OBSERVE preflight: inspect the client first

From a normal, unprivileged user account, record OS, CPU architecture, existing launcher and Java/game version **without** exposing accounts or hostnames:

**Linux (Bash):**

```bash
uname -s
uname -m
command -v prismlauncher || true
command -v flatpak || true
command -v minecraft-launcher || true
command -v java || true
```

**Windows (PowerShell):**

```powershell
[Environment]::OSVersion.VersionString
$env:PROCESSOR_ARCHITECTURE
Get-Command 'MinecraftLauncher.exe','java.exe','prismlauncher.exe' -ErrorAction SilentlyContinue |
  Select-Object Name
# Check Windows Settings > Apps > Installed apps for Minecraft for Windows.
# Store apps and Bedrock version may not be discoverable from PATH.
```

The private FR3K Minecraft Control Centre has an optional **read-only** `client-preflight` CLI requiring Python 3.11+ and an **authorized private checkout**. It is **not available** in this public repo. Only if you already have legitimate access to that checkout, from its root:

```bash
# Linux: substitute exactly the identity of the machine executing the command.
PYTHONDONTWRITEBYTECODE=1 PYTHONPATH=src python3 -m fr3k_mc.cli client-preflight --client omarchy-linux
# Other Linux labels: x1-linux | chromebook-omarchy
```

```powershell
# Windows, from the authorized private checkout root:
$env:PYTHONDONTWRITEBYTECODE = '1'
$env:PYTHONPATH = 'src'
py -3 -m fr3k_mc.cli client-preflight --client windows-laptop
```

The optional CLI reads bounded local metadata, **does not contact a server** and emits JSON with `network_tested: false`. Its exit status 0 means a report was produced, **not** that Minecraft is ready. Unknown launcher locations, Store packages or versions remain **UNKNOWN**. Do not ask an unapproved agent to access the private project.

## 5. Client setup — only with explicit installation approval

### A. Linux / Omarchy / Chromebook: Minecraft Java via Prism

If Prism Launcher is already installed, skip installation. Otherwise, after confirming Flatpak is present and receiving installation approval, use its official Flathub package:

```bash
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub org.prismlauncher.PrismLauncher
flatpak run org.prismlauncher.PrismLauncher
```

If Flatpak is missing, record **BLOCKED — package manager approval needed**. Do not run an unreviewed `curl | sh` installer. In Prism Launcher, have the human sign in **locally** to the Microsoft account owning Minecraft Java Edition. Create a vanilla Java instance matching the **server operator's verified compatible release** (the original plan was 26.3), and select a compatible Java runtime (the original plan specifies Java 25). Let the launcher perform its official downloads. Do not publish launcher logs containing account data.

### B. Windows: Java Edition

Install/open the official **Minecraft Launcher** from Minecraft/Microsoft's official distribution, only with owner approval. Have the human sign in locally, select **Minecraft: Java Edition**, and select the server-verified compatible release. The server plan does **not** require client Forge/Fabric mods.

### C. Windows: Minecraft for Windows (Bedrock)

Install/open the legitimately licensed **Minecraft for Windows** via its official launcher/Microsoft Store. Have the human sign in locally with the authorized Microsoft/Xbox account. Confirm the Bedrock client version is within the **currently deployed Geyser-supported range**, not merely the historical range above.

**Cross-play rule:** server plugins and datapacks run on the server; they do not silently install Java client mods. Resource packs may present a normal in-game acceptance/download prompt. A Java-only modpack is not a Bedrock client package.

## 6. Connect and join — only once the server is VERIFIED LIVE

Obtain the operator-approved host/address through a **private channel**. This repository intentionally has no IP or DNS hostname for a game server.

**Java Edition (Windows or Linux):** Multiplayer → Add Server → Name `FR3KCRAFT` → Address `<OPERATOR_APPROVED_HOST>:25565` (or operator's verified custom port) → Done → Join. Default transport: **TCP 25565**.

**Bedrock (Windows):** Play → Servers → Add Server → Name `FR3KCRAFT` → Address `<OPERATOR_APPROVED_HOST>` → Port `19132` (or operator's verified custom port) → Save → Join. Default transport: **UDP 19132**; the staged Geyser/NetherNet plan also requires **TCP 19132** for discovery/signalling.

Both editions must use legitimately owned software, appropriate Microsoft/Xbox authentication, and the server-side whitelist. Floodgate bridges Bedrock players to the Java server; it does **not** grant free game licences or remove authorization requirements. Do not keep retrying rejected logins or circumvent a whitelist.

**Network prerequisites (not a firewall-change instruction):** outbound DNS, authentication/download HTTPS 443 and the edition-specific game port; operator-side inbound rules limited to Java TCP 25565 and Bedrock TCP+UDP 19132 **after** review. Actual destinations, firewall rules, proxy/VPN behavior and reachability are **UNKNOWN** until observed.

## 7. Server operator gate — not steps for an unauthorised client agent

Only the authorized server operator may complete and evidence these:

- [ ] Explicit owner acceptance of the official Minecraft EULA is recorded.
- [ ] Deployment performed through a reviewed, authorized infrastructure workflow, preserving existing non-Minecraft services.
- [ ] Paper starts and the actual build/version is recorded; Geyser + Floodgate load without errors.
- [ ] Public connectivity for Java TCP 25565 and Bedrock UDP/TCP 19132 is verified from outside the server.
- [ ] Enforced whitelist contains only approved Java usernames and Bedrock gamertags.
- [ ] RCON remains private; no Docker or management interface is exposed.
- [ ] A real Java player **and** a real Bedrock player independently join and can interact in the same world.
- [ ] A backup archive is created, its checksum checked **and its restoration tested** in a disposable environment.
- [ ] Actual connection details and supported client versions are privately issued to the approved agent.

No `docker compose up`, root escalation, EULA write, port opening, server installation or whitelist mutation is authorized by this document. The existing private bundle is staged; it has not been shown to be live. **A green checklist box in a report is not proof: attach the underlying operator evidence.**

## 8. Troubleshooting without damaging the environment

| Symptom | First read-only check | Correct result |
| --- | --- | --- |
| Server does not appear / timeout | Ask owner whether live deployment and externally tested game ports exist | **BLOCKED** when server is not verified live |
| "Not whitelisted" | Verify player identity privately with operator | Operator adds approved player; agent does not disable whitelist |
| Client/server version mismatch | Record exact client edition/version; request current Paper/Geyser compatibility | Reconcile versions before changing client |
| Java runtime mismatch | Check launcher-selected Java version and runtime error | Use supported runtime with approval |
| Bedrock sign-in / Floodgate error | Confirm game licence/account sign-in and operator-side plugin health | Do not share account tokens or secret `key.pem` |
| Linux launcher absent | Confirm Flatpak and existing launcher paths | Install only with approval |
| Resource pack rejected | Read the normal game prompt, record error | Respect user's acceptance choice; no forced mod install |
| Game launched but cannot join | Distinguish running client from authenticated and whitelisted server connection | Remains **UNKNOWN/BLOCKED**, not success |

## 9. Automated agents and in-world bots: separate concerns

An **installation/orchestration agent** can run permitted OBSERVE checks and guide authorized client installation. An **in-game agent** needs a separately designed, reviewed bot client, legal account/authentication, dedicated identity, world rules, rate limits, sandbox testing, and explicit operator permission. **FR3KCRAFT currently provides no working bot executable or verified AI Minecraft session.** Do not invent a Mineflayer endpoint, distribute session cookies, bypass premium authentication or claim a simulated NPC is a connected player.

The private control centre also contains **offline-only** Joy apprenticeship and Freik architectural-guild simulations. These are **not** actual spawned NPCs, terrain generation, world edits, or live reward transfers. Future actions that change blocks, grant rewards, teleport, reboot or reload must be human-approved, bounded, evidence-backed and recoverable.

## 10. Required handoff: an evidence-backed report

Return the following template **without secrets or raw identifiers in public**:

```text
FR3KCRAFT SETUP REPORT — <UTC timestamp>
Device label: <x1-linux | chromebook-omarchy | windows-laptop | omarchy-linux>
Authority: OBSERVE | APPROVED_INSTALL | APPROVED_JOIN
OS / architecture: <observed, no hostname or device serial>
Edition / client version: <observed or UNKNOWN>
Launcher / Java runtime: <observed or UNKNOWN / N/A>
Server live gate: VERIFIED with operator evidence ID | BLOCKED | UNKNOWN
Approved endpoint / whitelist: privately verified YES | NO | UNKNOWN
Connectivity checks: NOT RUN | result + private evidence ID
Actual successful login: VERIFIED | FAILED | NOT ATTEMPTED
Cross-play observed: VERIFIED | UNKNOWN
Changes made: <each change and authorizing approval ID; otherwise NONE>
Test/evidence: <private evidence IDs, command exit codes, redacted errors>
Outstanding blockers: <specific needed permission/action>
Final status: READY FOR APPROVED JOIN | VERIFIED JOINED | BLOCKED | UNKNOWN
```

**Verification rules:** keep raw logs, console evidence, relevant server-side confirmation and backup-restore evidence in access-controlled storage. Redact accounts, gamer tags, session data, addresses and tokens. Never infer success from command exit code, service heartbeat, a Git commit or an offline simulation.

## 11. Public contribution process

1. Read this runbook, start in OBSERVE mode, document what you know and do not know.
2. Open a GitHub issue/PR with a sanitized reproduction, expected result and verification plan.
3. Use official Minecraft, Paper, GeyserMC and Prism documentation for current upstream requirements; recheck exact versions before upgrading.
4. Add tests for changed scripts/code; never put production credentials, logs, player identities, real address/ports beyond generic defaults, world files, backups or private deployment paths in this public repository.
5. Report whether a change was **documented**, **committed**, **deployed**, **tested on a real client** and **verified in-game** as separate outcomes.

**Upstream:** [Minecraft](https://www.minecraft.net/) · [PaperMC](https://papermc.io/) · [GeyserMC](https://geysermc.org/) · [Prism Launcher](https://prismlauncher.org/). These links are references, not a substitute for verifying live compatibility.

**Bottom line:** an agent can prepare and inventory the four target clients now. Actual joining remains **BLOCKED** until the server is explicitly EULA-authorized, running, externally tested, whitelisted, and the operator privately provides an approved host.
