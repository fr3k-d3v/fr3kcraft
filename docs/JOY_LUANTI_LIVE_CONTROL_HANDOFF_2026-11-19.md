# Joy's Luanti/VoxeLibre live-control handoff

**Recorded:** 2026-11-19 14:06 Australia/Brisbane. **Source:** operator-supplied conversation transcript, not fresh device telemetry. **Evidence state:** transcript claims only unless explicitly noted. This is a sanitized handoff; no player coordinates, device identifiers, world files or credentials.

## Critical distinction
This is a **Luanti 5.17.0 / VoxeLibre** single-player world on Joy's Chromebook, **not** the planned Paper/Geyser/Floodgate Minecraft server described in AGENT_SETUP.md. Do not try to connect Mineflayer or the Java/Bedrock gateway to Luanti. Public FR3KCRAFT docs do not contain the private Minecraft control centre; do not conflate them.

## Operator requirements
- Build a **9 x 7 snowy cabin near Joy's current in-game position**, with the entrance facing her chosen marked spot.
- Use dark oak planks for floor/walls/roof, black stained glass and tinted glass where available, obsidian corner pillars and **dark oak door**. Preserve her existing structures and inventory.
- **No game restart, no disconnect, no offline world-database edits.** Joy must see actual blocks placed progressively in her live game session.
- Locate and reuse the original control layer reportedly built around 2026-11-17 before inventing a new implementation. GitHub is canonical; Hermes/Fleet are execution and evidence collection.

## Transcript-derived state (NOT current verification)
- Assistant previously claimed Luanti 5.17.0, VoxeLibre and Joy's Island running on Chromebook, Creative Mode enabled; Joy confirmed she could move, place blocks, open inventory with **I** and toggle flight with **K**.
- Assistant claimed a temporary cabin builder was staged and a restart occurred, but **Joy explicitly saw only normal landscape** afterward. Cabin is **NOT VERIFIED BUILT**.
- Assistant later claimed Wayland blocked reliable live keyboard/mouse automation, and the builder could not activate without code reload. These are **unverified prior observations**, not proof all live control methods are impossible.
- No live game control connection, successful block placement, or verified original control-layer repository was established in the transcript.
- Earlier approval tokens are deliberately omitted; do not reuse stale approvals.

## Next agent: execute evidence-first
1. Inspect this repo's AGENTS.md and AGENT_SETUP.md and the current GitHub issue/PR queue. Locate the **private** control centre or original Nov 17 implementation using authorized GitHub and Omarchy/Hermes access. Read its governance, branch/status/diff, tests and actual supported game protocol.
2. Read-only inspect the current Chromebook Luanti process, game window/session, world path and active mods; check whether Joy is currently connected before touching anything. Keep game running.
3. Identify a **genuinely live, no-restart** interface already loaded: existing mod chat command/API, remote console if supported and authorized, in-game scripting already active, or reliable Wayland-compatible GUI input. A new Lua mod normally cannot be loaded into an already running world without reload; do not claim otherwise.
4. If a live interface exists, perform **one bounded visible test block** at a verified safe location near Joy with operator authorization; obtain game-side/visual confirmation before the cabin. Do not teleport or alter unrelated blocks.
5. Only after that gate, build incrementally with material IDs validated against VoxeLibre, track exact block counts and placement acknowledgements, preserve rollback evidence, and verify visually with Joy.
6. If no live interface exists, report **BLOCKED** with the exact missing capability. Do not restart, fake success, or perform uncontrolled clicks.

## Evidence ledger
| Claim | Status | Needed proof |
| --- | --- | --- |
| Chromebook world was running earlier | TRANSCRIPT-REPORTED | Fresh process + session check |
| Cabin placed | NOT VERIFIED; Joy reported absent | Visible in-world block confirmation |
| Live control works | UNKNOWN | Successful bounded test placement |
| Original Nov 17 code located | UNKNOWN | Canonical repo path, commit SHA, tests |
| No-restart constraint | OPERATOR REQUIREMENT | No restart/disconnect in execution logs |

## Reproducible verification and remaining unknowns
Record UTC timestamp, repository commit, authorized device session, Luanti/VoxeLibre exact versions, loaded mod list, input/control transport, test block before/after, cabin dimensions and visual result. Sanitize all public logs. **Not tested in this handoff:** current Chromebook connectivity, active world/player position, existing control implementation, live placement, rollback, and in-game visual result.
