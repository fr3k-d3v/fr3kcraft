# FR3KCRAFT — free, no-Microsoft-account multiplayer

**Evidence checkpoint — 2026-11-21 (AEST)**

This is the free route for the FR3KCRAFT project. The original Paper/Geyser plan targets proprietary Minecraft Java/Bedrock and **requires a purchased game/account**; it remains a separate unlaunched design. The free route uses [Luanti](https://www.luanti.org/) 5.17 and [VoxeLibre](https://github.com/VoxeLibre/VoxeLibre), a Minecraft-inspired open-source survival/building game. **It is not the proprietary Minecraft client and cannot connect directly to Minecraft Java/Bedrock servers.**

## Verified live results — 21 November 2026 (AEST)
- **Two simultaneous players:** Luanti 5.17.0 clients on **X1 and Chromebook** both joined the same private VoxeLibre 0.92.4 server. The server independently logged both players in the list of active clients. Actual co-presence is verified; shared block editing and direct cross-player chat have **not** yet been independently verified.
- **Original-world preservation:** the existing **Joy's Island** world was preserved, backed up as a SHA-256-verified archive, and copied to a separate multiplayer world directory. The copied Joy's Island world—not the temporary test lobby—is now the active game server, listening on the private overlay network only.
- **Restoration exercise:** 22 restored files matched the original bytes exactly; all four restored SQLite databases reported `PRAGMA integrity_check = ok`. A full game-client boot and interaction against the restored archive remains a separate test.
- **AI:** the world-local `/ask` Lua command returned a real generated building answer into X1's in-game chat, from the authenticated, GPU-hosted local model via a private network bridge. The model has advisory/chat authority only, not block placement or server control.
- **Scripted builder:** `joy_builder` loaded in the copied world and processed a queued command. Its server-side building recipes are scripted mechanics, **not** a verified autonomous LLM-controlled player.
- **Persistence:** a validated and boot-enabled user-service unit for the world exists, but the current live server process remains externally launched. Cutover to managed service and the AI bridge's persistent service activation are not yet verified. No live player session has been deliberately disrupted for cutover.
- **GPD:** the fresh Omarchy GPD has not been observed in Fleet or Tailscale. Enrolment is outstanding.

## No cloud sign-in
Luanti does **not** use a global Microsoft/Mojang account. Players pick their own local player nicknames. Servers can permit empty passwords, but clients may still show a local username/registration UI. Empty passwords must only be used on a trusted, tightly access-controlled private server, **never as a public anonymous server**. FR3KCRAFT currently keeps its Joy's Island server on the existing private overlay network with public server-list advertisement disabled.

## Onboarding
1. Install the official Luanti client (Arch/Omarchy: `sudo pacman -S --needed luanti`; Windows/macOS/Android: use the project's official downloads).
2. The world game/content is delivered by the server to clients; a separate Java Minecraft license or Prism Launcher is **not** needed.
3. Obtain the actual approved tailnet host and UDP game port privately from the operator; no public address is distributed in this guide.
4. Join the private FR3KCRAFT server with a nickname. After an actual join, use `/ask Suggest a bridge design` to test AI advice; the command never changes world blocks.
5. Report both a real successful join and, independently, an actual AI chat response before marking it operational.

## Security/architecture
- GPU-hosted LLM is the existing local, abliterated model; its API key is never copied to clients or stored in this public repository.
- Private bridge binds to a private-tailnet interface, has a source-IP allowlist, bounds/rate-limits requests, and only produces textual advice. It has **no server console or world-edit permission**.
- `/ask` is advisory only. Future AI builders must have separately reviewed bounded actions, approvals, rollback and independent observation.
- The original `Joy's Island` files have a tested, checksum-verified backup. A full restored-game boot test and recurring backup schedule remain outstanding.
- Retain private fleet identifiers, keys, addresses, player IDs, configuration files and logs outside this public repository.

**Truth rule:** successful build ≠ deployment; listening UDP port ≠ player connection; model response ≠ world mutation; current running server ≠ proven permanent service.
