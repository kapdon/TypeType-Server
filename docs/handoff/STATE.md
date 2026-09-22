# Server handoff state — 2026-09-23

Read [the shared setup and private-material inventory](../../HANDOFF.md) first.

## Verified source state and completed work

- Resume `codex/cross-computer-handoff-2026-09-23`, descended from `8170ba50`.
- Subscription membership pagination, bounded selection lookup, SQL membership filters and Unicode
  normalization are implemented. OpenAPI entry: `openapi/paths/subscription-membership-pages.yaml`.
  Tests include `SubscriptionMembershipPageServiceTest` and `SubscriptionMembershipPageRoutesTest`.
- Upstream [PR #86](https://github.com/TypeType-Video/TypeType-Server/pull/86) was checked with GitHub
  CLI on this date: OPEN, **not draft**, head `d245874b`. This supersedes older draft-status reports.
- Local commits after that PR head: `2a46d166` tests; `e5b6a722` partial module placement;
  `8170ba50` source deletion. These are preserved on the handoff branch, not pushed over PR #86.
- The working tree retained all 1,401 deleted `src/` files as untracked files. Every Git blob hash
  matched `8170ba50^:src/`; no new/different/missing files. `f3512edd` restores them to Git unchanged.
  This preserves the actual local source used by the current root Gradle build on a fresh clone.
- `settings.gradle.kts` does not include modules; root build still uses standard `src/`.
  Only four module directories are tracked: server-core, server-db, server-services and server-http.
  server-http references absent modules including server-admin/auth/cache/domain/playback/sabr.
  **The modular migration is incomplete and is not a verified buildable modular server.** Do not
  delete root src or blindly add all module includes. Treat duplicate module sources as migration WIP.
- The old local feature-named branch points at unrelated upstream `6c6fc0f1` (ahead/behind its remote).
  It is preserved separately as `codex/preserved-upstream-dev-2026-09-23`; no history was rewritten.

## Verification evidence

Fresh: source blob comparison above; `git diff --check`; `./gradlew --version` attempted and failed
because this machine has no Java runtime. Docker is not on PATH. No server tests/build/OpenAPI checks
were rerun, and old local `build/` outputs do not count as fresh proof.

Historical report for **d245874b only**: 1,251 tests passed, 3 skipped; build, OpenAPI and coverage
checks passed; regressions exercised 25,000 channels, Unicode and account isolation. The historical
raw report is unavailable here. These results do not validate the later module commits or restoration.
Staging health returned 200, but its deployment revision and membership API acceptance are unknown.

## Exact next steps and acceptance gates

1. Install JDK 25 and Docker/Compose. Confirm `java -version`, `docker info`, `docker compose version`.
2. From this branch run `cp .env.example .env`, configure isolated development settings, then
   `docker compose up -d postgres dragonfly`. Do not point destructive tests at staging/production.
3. Run `./gradlew test`, `./gradlew shadowJar`, `./gradlew validateOpenApi`, `./gradlew check`.
   Require actual compiled sources and executed tests, not NO-SOURCE/cached output alone. Record
   test counts/skips, warnings and the exact SHA. Run the jar at `build/libs/typetype-server-all.jar`.
   The optional external test DB uses TEST_DATABASE_URL/USER/PASSWORD; see
   `src/test/kotlin/dev/typetype/server/TestDatabase.kt` before using a disposable database.
4. Resolve the module migration as a separate change: compare upstream modular branches, restore
   all required modules/build wiring coherently, relocate resources/test fixtures and verify all
   modules. Alternatively retain the working single-module build while deferring migration.
   Ownership: resuming server implementer; acceptance owner: Dimago/upstream maintainer.
5. Pair this API with the frontend branch. Verify authenticated membership paging, selected-channel
   lookups across pages, Unicode filters, user isolation, bounded writes, stale membership refresh
   and partial failure recovery. Benchmark realistic large libraries; tests alone are not a latency SLA.
6. Verify PR #86's current head again before submitting anything. Do not replace it with the unrelated
   local feature-named branch. Keep the handoff WIP branch distinct until build/migration review.
7. Obtain deployment host, credentials, stack path, image digests and backup procedure. Only after
   acceptance plan a server-first rollout compatible with the new frontend endpoints; record health,
   live integration results, deployed SHAs/digests and rollback reference. No rollout occurred here.

Remaining blockers: missing JDK/Docker, incomplete module migration, unknown live revision and
operator access. No fresh server pass is claimed. No separate worker currently owns these tasks.
