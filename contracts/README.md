# Research Consumer Contract Locks

Research App does not own the Knowledge Retrieval/Citation schemas. The only
editable source of truth is `knowbase-app/contracts/retrieval/`.

`knowbase-retrieval-provider-lock.json` pins the compatible major and SHA-256
of every provider schema consumed by Research. Contract CI must download or
check out the Knowledge provider artifact, set
`KNOWBASE_RETRIEVAL_CONTRACT_DIR`, and run the Research consumer tests.
Updating this lock without provider and consumer tests is not a valid contract
upgrade. Generated or copied schemas must not become a second source of truth.

## Knowbot manifest (knowmart)

`knowmart-knowbot-manifest-v2-lock.json` pins the outer knowbot manifest
`knowbot-manifest/v2` that the workbench summons from knowmart (SDD 0027 P4,
SDD 0043; service identity `knowmart:summon`). The source of truth is
`knowmart-app/contracts/knowbot/v2/`. The backend test
`tests/test_knowbot_contract_lock.py` checks the lock against a sibling
`knowmart-app` checkout (or `KNOWMART_KNOWBOT_MANIFEST_SCHEMA_PATH`) and
validates the workbench's sample manifest against the provider schema. The
inner `pack` is the workbench's own expert-pack shape and is validated by the
backend (`app/domain/workbench/knowbots.py`). Only v2 is accepted (decisions ⑰,
2026-10-28: no compatibility with v1).

## Data request (knowbase)

`knowbase-data-request-lock.json` pins `data-request/v1`, the description of one
item of data that step 0 and `request_data` propose (SDD 0043, decisions ⑭).
The source of truth is `knowbase-app/contracts/data-request/v1/`; the backend
test `tests/test_knowbase_data_request_lock.py` checks the lock.

## Knowbot test runs (knowlab)

`knowlab-test-run-lock.json` pins the `knowbot-test-run/v1` contract that the
workbench implements as a runtime (SDD 0027 §3.4, service identity
`knowbench:testrun`, routes under `/api/internal/v1/knowbot-runtime`). The
source of truth is `knowlab-app/contracts/knowbot-test-run/v1/`. The backend
test `tests/test_knowbot_test_run_lock.py` checks every pinned digest against a
sibling `knowlab-app` checkout and validates a sample request and the results
the workbench produces against the provider schemas. Since 2026-10-30 the
contract also has `POST …/test-runs/{run_id}/cancel` (same schemas; answers
`RunResultV1`): knowlab calls it when an author withdraws a delivery or a
platform review fails. The workbench stops every running case task (the step on
the author's computer is interrupted), hands back the grants those tasks pinned,
records the run as `cancelled`, and answers the same on a repeat call.

## Knowbot incubation (knowlab)

`knowlab-incubation-lock.json` pins the `knowbot-incubation/v1` contract that
the workbench implements as a runtime for one AI round of incubation (drafting
or revising an author's workdraft; 专家孵化方案 §8, decisions ⑮⑯; service
identity `knowbench:incubate`, callers limited to knowlab, routes
`/api/internal/v1/knowbot-runtime/incubations`: submit, read, cancel). The
source of truth is `knowlab-app/contracts/knowbot-incubation/v1/`. The round
runs on the author's account, computer and model key, in the fixed project
「孵化」 under the chosen workspace root, with the built-in incubation method
(`packs.INCUBATE`, not listed among the experts). The backend test
`tests/test_knowbot_incubation_lock.py` checks every pinned digest against a
sibling `knowlab-app` checkout and validates a sample request and the results
the workbench produces against the provider schemas. Since knowlab `625500a`
(2026-10-29) each data format also carries `media_type`, the knowbase
`description` dimensions and the ready `readings`, and a revise may carry the
author's latest `test_report` (report only: no draft, outputs or test data);
both go to the model as part of the round's material.

## Demand status (knowfeed)

`knowfeed-supply-status-lock.json` pins `knowfeed-supply-status/v1`, the
notification knowfeed sends when a demand the workbench raised changes status
(SDD 0045 §2.5). The source of truth is
`knowfeed-app/contracts/supply-status/v1/`. The receiver is
`POST /api/internal/v1/knowfeed/demand-status` (service relation
`supply-status`, scope `knowbench:supply-status`, caller limited to knowfeed).
A finished demand makes the waiting task re-check that demand at once and go
on; the check still trusts what the demand reads by its id, and polling stays as
the fallback. The backend test `tests/test_knowfeed_supply_status.py` checks the
lock against a sibling `knowfeed-app` checkout (or
`KNOWFEED_SUPPLY_STATUS_CONTRACT_DIR`) and that the backend model takes exactly
the fields the schema lists.

## Supply prematch (knowfeed)

`knowfeed-supply-prematch-lock.json` pins `knowfeed-supply-prematch/v1`, the
question the workbench asks knowfeed before the user confirms the data of a
delegation (SDD 0043 §3.3 step 1, SDD 0045 §4): for each item to be fetched,
can it be fetched, is the site still under review, which license tier the first
channel gives, and how long the queue estimates. The source of truth is
`knowfeed-app/contracts/supply-prematch/v1/`. The caller is the backend with the
service relation `supply-demand` (scope `knowfeed:supply-demand`, the same as
demands), `POST /api/internal/v1/supply/prematch`. The answer is shown on the
confirmation panel (task page and board card; the whole ask is capped at 5
seconds), kept with the task, and reused when the user confirms: each answer,
with the description key that was asked, goes together with the workbench's
acceptable conditions into the knowbase confirmation as
`supply = {"conditions": {...}, "prematch": <answer>}` (no second ask at
confirmation). When knowfeed is not configured, the supply switch is off or the
call fails or is slow, the list is shown without estimates. The backend test `tests/test_knowfeed_supply_prematch.py`
checks the lock against a sibling `knowfeed-app` checkout (or
`KNOWFEED_SUPPLY_PREMATCH_CONTRACT_DIR`), that the backend models carry exactly
the fields the schemas list, and that the request it builds validates against
the provider schema (when `jsonschema` is installed).

## Preferences written back to knowbase

When the user swaps the copy knowbase picked for another candidate on the
confirmation panel (SDD 0043 §3.5), the workbench writes the preference back
with the user's own token (`PUT /api/internal/v2/user-data/preferences`, body
`{consumer, key, material_ids}`). Convention: `consumer` is the expert without
its version, `expert:<id>` (the resolution's consumer with `@<version>`
dropped), so a preference keeps working when the expert publishes a new
version. knowbase keys preferences by user × this consumer × description.

## Removed on 2026-10-28 (decisions ⑰)

- `knowlab-knowbot-data-lock.json` (`knowbot-data/v1`: reading the user's data
  through knowlab with the service identity `knowlab:read`). The user's data is
  read from knowbase `user-data/v2` with the task's run grant.
- `knowmart-knowbot-manifest-lock.json` (manifest v1).
- The request of `knowbot-test-run/v1` no longer carries the v1 test grant
  `data_grant`; a case brings its test set in `data`.
