# Isolated laboratory — issue #3

Operator procedure: [Run with Docker](../../docs-website/docs/getting-started/docker.md#isolated-test-laboratory-issue-3).

`lab.py` creates private state under `.data/isolated-test`; no deployment file or
connection setting used by the current server is changed. Commands are scoped to
the generated Compose project. There is deliberately no cleanup command that
deletes volumes and no restore command that overwrites the source.

## Execution evidence — 2026-10-22

The owner authorized beginning T02 in chat, superseding the ticket's earlier
planning-only restriction for this isolated scope. No current-server database,
configuration, client profile or container was changed. No load/population test
or continuous observation was started.

- Image built from local source with `src/Startup/Dockerfile`; build/publish
  succeeded. Build reported 44 existing warnings; no C# source was changed.
- Application image ID:
  `sha256:6b4f0c09d42a56fb8eabf14d75ea37adc1f16f79dd5dee7a1ba20a1170e00c6b`.
  The private identity manifest records the Git revision and resolved image IDs.
- Laboratory project: `openmu-t02-03871313ec3e`; source database `openmu_lab`
  exists only inside its own PostgreSQL instance and volume. Separate restore
  instance/volume uses the same logical database name. Database ports are private.
- Source initialization produced 20 standard fixture accounts, 76 characters,
  4,603 items and one game configuration. These are initialization fixtures,
  not earned progression or an approved phase-one world.
- Clean PostgreSQL restart preserved complete sorted row data, schema and
  sequence values/`is_called`. Private evidence: `persistence.json`.
- Backup: `snapshot-20261022T154003Z` (UTC recovery point identifier).
  SHA-256:
  `356ffc8adff7c6fc825f31f591ec33aaaae76de49b91cd3fc4812c45742ff991`.
- Independent restoration succeeded; all 122 user tables, schema and sequences
  matched. Measured restore-plus-comparison duration: approximately 12.59 seconds,
  excluding image build, container creation/readiness, and application recovery.
  This is an observation for this fixture set, not an RTO guarantee.
- Private evidence: snapshot `manifest.json`, `database.dump`, `restore.log`,
  `verification.json`. Dumps contain credentials/personal data and stay private.
- Host panel responded HTTP 200 on `127.0.0.1:18080` after resuming the
  laboratory. A healthcheck now waits for the login page on future starts.
- Disabled `config.SystemConfiguration.AutoStart` only in the source laboratory
  after the verified snapshot. The original snapshot/restore retains its earlier
  configuration; it does not include this later operational adjustment. The
  initialized default had started game subsystems inside the laboratory before
  this adjustment; no client or bot load was connected.
- Six local safety tests passed: non-loopback rejection, existing-state
  protection, occupied-port rejection before writing state, private credentials
  and isolated resource definitions, corrupt-backup rejection, foreign-project
  backup rejection. Run:
  `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest discover -s deploy/isolated-test -p 'test_*.py'`.

Initial drill final state: all three laboratory containers were stopped and volumes retained.
Both PostgreSQL containers exited 0. The application exited 137 on the final
Compose stop; graceful application shutdown was **not established** by that initial drill.
The follow-up below records the separately authorized diagnosis and fix. The verified database restart/restore
results above do not establish graceful application shutdown.

## Shutdown diagnosis and correction — 2026-10-22

The owner authorized diagnosing exit 137, correcting it in the isolated
laboratory, and verifying clean shutdown and persistence. The existing server
was not stopped, reconfigured or upgraded.

**Cause:** the custom main loop observed only its own cancellation source. The
host's ConsoleLifetime translated SIGTERM into `ApplicationStopping`, but that
token did not release the main loop, so the normal hosted-service cleanup was
never reached before Docker's stop budget elapsed. The old `ProcessExit` callback
was too late to drive this lifecycle. The fix links the loop's token to
`ApplicationStopping`, cancels its delay/console wait, handles expected cancellation
as normal completion, and removes the late callback. The ordinary host cleanup
then runs. The 10-second Docker stop budget was unchanged.

**Regression command:**

```bash
PYTHONDONTWRITEBYTECODE=1 python3 deploy/isolated-test/check_shutdown.py
```

- Before the fix, the command failed with exit 137, `OOMKilled=false`, approximately
  11.26 seconds. Private report: `shutdown-20261022T155955Z/report.json`.
- After the fix, both cycles passed with exit 0 and `OOMKilled=false`; observed
  stop durations were 1.276 and 1.240 seconds, including Compose overhead.
- The complete sorted row contents of all 122 tables, schema, and sequence state
  matched the baseline after each application cycle and after a clean PostgreSQL
  restart. This verifies idle committed-data durability, not in-flight game writes.
- Logs record completed connect/chat shutdown and game-listener shutdown, without
  shutdown exceptions in the inspected cycle logs.
- Private evidence: `shutdown-20261022T160605Z/report.json`, `baseline.json`, and
  `world-1.log` / `world-2.log`. Raw logs remain private.
- The original minimal reproduction was also rerun against the fixed image:
  exit 0, `OOMKilled=false`, approximately 1.208 seconds. Private results:
  `shutdown-original-before.json` and `shutdown-original-after.json`.
- Build/publish succeeded with the same 44 existing build warnings and no warnings
  in the changed Startup source. Eight procedure safety tests passed, including
  refusal to treat exit 137 as a clean stop. The actual Docker regression above
  covers the process lifecycle; mocked safety checks alone do not prove shutdown.
- Fixed image, applied only to the laboratory:
  `sha256:63841e5a7190a4a5066cc143b94701a754bd180584289e4206bb2c0dd9df315f`
  (`openmu-t02:shutdown-fix`). Private `identity.json` records the previous image,
  fixed image, and SHA-256 of the tested `Program.cs`; `shutdown-build.log`
  records the build. The existing production image/reference was not replaced.
- `lab.py stop`, `backup`, `persistence` and `admin-only` now inspect the application
  exit status and reject abnormal/forced exits. Database containers are left
  available for inspection on application-stop failure.

Final verified state: application, source PostgreSQL and restore PostgreSQL are
all stopped with exit 0 and `OOMKilled=false`; volumes and backup evidence are
retained. Private final record: `shutdown-final-state.json`.

Scope limits: this is an idle Linux-container SIGTERM test with no connected
clients or bots. Interactive terminal input/SIGINT confirmation, active-player
logout/save, purchases, reset/MR, market operations in flight, resource pressure,
and shutdown under load remain unverified. Recovery/observation/balance targets
remain hypotheses, and the rest of issue #3 stays open.

## Explicit pending acceptance gates

- Create and document fresh journey accounts and additional advanced fixtures
  through the isolated admin panel; standard initialization fixtures alone do
  not demonstrate an earned journey. Keep these cohorts separate.
- Verify new-character creation, client endpoint advertisements, login, equipment,
  inventory and configuration through the application connected to restored data.
  Database equality is not an end-to-end gameplay recovery test.
- Exercise disconnect/reconnect, application restart with connected clients, crash during paid
  operations, reset/MR and market reconciliation in their implementation tickets.
- Back up/recover admin session keys and external files for a full-host-loss drill;
  this implementation verifies logical PostgreSQL recovery with lab-only ownership.
- Provide a fresh empty recovery target for subsequent drills. Existing recovery
  evidence is retained; automatic overwrite/destructive cleanup is absent.
- Approve hardware-specific observation/stop conditions, off-host backup location,
  retention/disk limits and scheduling before any 24/7 run. Local CPU/RAM/log
  budgets are candidates, not demonstrated population capacity.
- RPO 15 minutes, RTO 30 minutes, seven-day windows, population targets and all
  progression/combat/economy numbers remain hypotheses. No configuration is
  promoted and issue #3 is not closed by this database drill.
