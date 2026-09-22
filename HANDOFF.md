# Resume on another computer — 2026-09-23

Read this file, then `docs/handoff/STATE.md` and `CONTRIBUTING.md`. No chat history is required.
This snapshot covers the three local TypeType repositories. The visualization workspace was empty.
All paths below describe the source Mac; replace `/Users/dimago` on the destination.

## Repositories and preserved work

| Repository | Clone URL | Resume branch | Baseline before this handoff |
| --- | --- | --- | --- |
| Server | https://github.com/kapdon/TypeType-Server.git | `codex/cross-computer-handoff-2026-09-23` | `8170ba50` detached; source restoration `f3512edd` |
| Frontend | https://github.com/kapdon/TypeType-Frontend.git | `codex/subscription-groups-ux` | `dda7c67` |
| iOS | https://github.com/kapdon/typetype-ios.git | `codex/video-casting` | `3f3b557` |

Server upstream is `https://github.com/TypeType-Video/TypeType-Server`; frontend upstream is
`https://github.com/TypeType-Video/TypeType-Frontend`. Existing source checkouts call these `origin`
and the kapdon repositories `fork`. A fresh clone of kapdon calls kapdon `origin`; adjust push commands.
Do not switch to default branches to resume this work.

Additional preserved branches: iOS `codex/isolated-seek-handoff` at `2ec5327` contains two commits
not merged into casting/main. Review before integration; do not assume casting includes them.
Server `codex/preserved-upstream-dev-2026-09-23` preserves local `6c6fc0f1`, previously misleadingly
named `codex/subscription-group-pagination`. The actual upstream PR #86 head remains `d245874b`
on the fork's `codex/subscription-group-pagination`. No force push or reconciliation was performed.
Other iOS feature branch tips are ancestors of casting and recoverable from its history.
No stashes or additional worktrees were found in these three repositories.

## Ownership and deployment

Dimago is the resuming operator and acceptance owner. No other active implementer was established.
Server owns SQL, API contracts, auth, extraction and deployment compatibility; frontend owns browser
UX; iOS owns native playback, receiver handoffs and signing. Upstream maintainers own PR acceptance.
The central TypeType stack, Token, Downloader and Player are external dependencies, not locally
modified repositories in this snapshot. Their deployment versions have not been inventoried.

Known staging URL: `https://typetype.stage4a.com` (combined API under `/api/`). A fresh unauthenticated
`GET /api/health` returned HTTP 200 on 2026-09-23. This establishes reachability only. Exact deployed
server/frontend Git SHAs, image digests, host, SSH user, stack directory, backups and rollout method
remain UNVERIFIED. No deployment was made by this handoff. Obtain these from the instance operator
before replacing anything; record running image digests and a database backup before a later rollout.
iOS build 1.0 (2) installation and working TV casting were previously reported; see the iOS state doc.

## Material intentionally outside Git

Never place secret values, account cookies, playback-session URLs, private keys or database dumps
in Git, issues, screenshots or command output. Use an encrypted password-manager item or authenticated
encrypted file transfer; verify the recipient and remove temporary decrypted copies. Prefer fresh
login/new per-device credentials over copying tokens. Git history is not a credential vault.

| Item | Current storage / evidence | Secure transfer or recreation |
| --- | --- | --- |
| GitHub access | `~/.config/gh/hosts.yml` exists; credential helper/Keychain may also hold credentials; secret storage mode not inspected | Install `gh`, run `gh auth login`, then `gh auth setup-git`; grant the new device repository access. Do not copy tokens into remote URLs. |
| SSH private keys | `SSH_AUTH_SOCK=/Users/dimago/.bitwarden-ssh-agent.sock`; `~/.ssh/agent` exists; no `~/.ssh/config` found; private key inventory not inspected | Install/unlock Bitwarden and enable its SSH agent, or create a new key and authorize it through the host operator. Do not copy the socket. Confirm the exact vault item and host access with Dimago. |
| SSH hosts and trust | `~/.ssh/known_hosts` and `.old`; TypeType deployment host/user/port not established | Obtain host/user/port and fingerprint from the operator through a trusted channel; recreate a minimal SSH config and verify fingerprint. A GitHub key does not prove server access. |
| Staging login / API tokens | Historical staging login supplied in prior work; durable vault location UNCONFIRMED. Active app credentials may be in device/simulator Keychain; browser tokens in browser storage | Have Dimago/instance operator provision or reset a dedicated test account and save it in the password manager. Reauthenticate on the new machine. Do not rely on chat or copy browser profiles. This remains a live-test access blocker until done. |
| Server private settings and service credentials | No local `.env` found; only tracked `.env.example`. Production env/Compose overrides, DB credentials, JWT/session secrets, OIDC, Token/Downloader keys, proxy/YouTube cookies and TLS material may reside on deployment host; exact paths unknown | For a new local instance copy `.env.example` to ignored `.env` and configure isolated development values. For existing live work obtain the operator's secret-store inventory and transfer via encrypted vault; preserve encryption/signing keys needed by existing data. Never use sample values in production. |
| Frontend private config | No local `.env` found; template `apps/web/.env.example` is tracked | Recreate `apps/web/.env` from template. `VITE_*` values are browser-visible build input: never put secrets there. Set API/proxy target for the desired environment. |
| Apple account, signing private keys, provisioning and device pairing | Existing Mac login Keychain/Xcode account and provisioning stores are expected; individual identities not inventoried; local Xcode `xcuserdata` exists | Sign in to Xcode with the authorized Apple account, select team and regenerate development profiles. If an existing identity is required, export its certificate+private key as password-protected p12 via Keychain and send password separately, or issue a new identity. Pair/trust the phone and enable Developer Mode. No p12/profile goes into Git. |
| iOS app user state | UserDefaults, Keychain, simulator/device app containers, offline downloads; not in Git | Sign in again; server-backed data resyncs. Use encrypted device backup or approved export for indispensable local-only downloads/state. Exact local dataset not inventoried. |
| Live data and services | PostgreSQL data, object storage/downloads, reverse-proxy config, Redis/Dragonfly state belong to deployment host/volumes; no local Docker command available | Operator must identify volumes, back up PostgreSQL consistently and export necessary objects over encrypted transport. Restore to an isolated test instance first. Do not assume a source clone restores accounts or media. |
| Build dependencies/caches | Server `.gradle/`, `build/`; frontend `node_modules/`, `apps/web/node_modules/`, `apps/web/dist/`, generated `src/paraglide/`, inlang cache/metadata; iOS `.build/`, `.swiftpm/`, `.vendor/`, `.derivedData/` | Reinstall pinned dependencies and rebuild. Caches are optional, never the authoritative source. Xcode package lockfile, Bun lockfile, Gradle wrapper and Cast SDK checksum/setup script are tracked. Network access to package hosts is required. |
| iOS release binaries | `.derivedData/Exports/2026-09-21/TypeType.ipa`, `TypeType-AltStore.ipa`, `Signed/TypeType.ipa`; older export under `2026-09-08/`; `.derivedData/Release-2026-09-21/` and `Device/` | Copy needed IPA/archive/dSYM privately via encrypted transfer and compare SHA-256 on both machines, or rebuild/sign from casting branch. Existing signing may expire or be device-limited. Preserve archive+dSYM for crash symbolication if needed. |
| QA evidence and local reviews | New `/private/tmp/typetype-handoff-*.log`; old `/private/tmp/typetype-prs-ocr-review.json` was referenced historically but is not currently present; iOS `.impeccable/review/` exists | Fresh results are summarized in state docs. Transfer selected redacted reports privately or rerun. Temporary files may disappear. Historic screenshots/recordings were not exhaustively located and are not required to build. |
| Agent/tool configuration | `~/.codex/`, `~/.agents/`, ignored AGENTS.md rules, graph indexes and OS editor settings; not required at runtime | Reinstall optional tools/plugins or use shell commands here. Structural exploration used codebase-memory; its index is regenerable and has a known partial parse in `HomeRecommendationSignalProfileBuilder.kt`. Code and build files remain authoritative. |

This is a bounded inventory of the three checkouts and conventional credential locations, not an audit
of every file on the Mac, password vault or deployment host. Unconfirmed locations are explicit
blockers for reproducing live access, not proof that the material does not exist.

## Fresh-computer checklist

- [ ] Install Git, GitHub CLI, curl, unzip; authenticate GitHub and verify access to all three forks.
- [ ] Clone each URL with its resume branch (`git clone --branch BRANCH URL DIRECTORY`). Run
  `git fetch origin` and `git status -sb`; inspect the latest handoff commit and ensure a clean tree.
- [ ] Fetch iOS `codex/isolated-seek-handoff` and the preserved server branch before resolving work.
- [ ] Install JDK 25 (`java -version` must work) and Docker Engine/Compose v2 for server/PostgreSQL
  and Testcontainers. Use the tracked Gradle wrapper; do not require a global Gradle install.
- [ ] Install Bun 1.4.2 as pinned in frontend package.json, plus Node (source Mac had Node 24.19.0;
  build scripts invoke `node`). The fresh checks here used Bun 1.4.0; exact 1.4.2 validation is pending.
- [ ] For iOS use macOS with Xcode 27 (observed 27A266a), its command-line tools and an iOS 17+
  simulator/device. Package.swift requires Swift tools 6.1. Select Xcode with `xcode-select`.
- [ ] Recreate ignored env files and authenticate staging through the operator. Confirm SSH/vault,
  Apple signing access and a receiver reachable on the same LAN if continuing live work.
- [ ] Follow repository-specific commands and acceptance gates in `docs/handoff/STATE.md`.
- [ ] Reconfirm staging version and logs before attributing failures to this source. Do not deploy the
  server module migration until its build wiring and clean-clone tests are resolved.
- [ ] Keep exported IPA/archive/dSYM and live database backups privately if needed; Git is insufficient
  to preserve installed app state, credentials, signing identity or production data.

The source can be resumed from these branches. Live work still needs a durable test-account vault
entry, verified deployment/SSH details and secrets, and Apple/device access. Server execution needs
JDK/Docker and clean-clone validation. Pending acceptance checks are not completed by this handoff.
