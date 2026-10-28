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
the workbench produces against the provider schemas.

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
the workbench produces against the provider schemas.

## Removed on 2026-10-28 (decisions ⑰)

- `knowlab-knowbot-data-lock.json` (`knowbot-data/v1`: reading the user's data
  through knowlab with the service identity `knowlab:read`). The user's data is
  read from knowbase `user-data/v2` with the task's run grant.
- `knowmart-knowbot-manifest-lock.json` (manifest v1).
- The request of `knowbot-test-run/v1` no longer carries the v1 test grant
  `data_grant`; a case brings its test set in `data`.
