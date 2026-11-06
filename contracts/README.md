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
# WP-10 动态方法契约（开发中，未发布）

`dynamic-methods/v1/catalog.schema.json` 由后端 `domain/workbench/method_catalog.py` 生成，模型是运行目录的真源；在后端 `app` 执行 `python -m app.domain.workbench.method_catalog`，再更新 schema SHA 与 knowlab 消费锁。`tests/test_dynamic_method_catalog.py` 验证文件生成、语义约束和旧固定入口的拒绝行为。`knowmart-knowbot-manifest-v3-lock.json` 锁外层 v3。

目录只存作者冻结的方法，不能存任务 workflow；实例和成果版本的绑定由后续计划模块承担。当前 `catalog_from_manifest` 只做校验，`pack_from_manifest` 明确拒绝 v3；固定 v2 不变。孵化 v1 消费锁同步新增可选工作稿字段及 planning/selection 建议路径，工作稿原样传递，规划建议仍须由作者接受。

WP-10 方法引用（开发契约，未发布）：新增 `dynamic-methods/v2/catalog.schema.json`；v1 schema 字节不变。v2 的 `method_sources` 含精确方法引用与作者定义证据，`knowlab-author-method-lock.json` 钉定义真源。运行方校验摘要、绑定、版本与选择界限，不维护另一份作者编辑模型或编译器。任务计划保留冻结来源，执行入口仍拒绝 v3。

## Expert selection v4 (2026-11-04, development only)

The current web client explicitly requests `knowbench/expert-match/v4` on the
existing find-expert route. The provider is backend `domain/workbench/expert_match.py`;
`packs.find_pack` supplies its output schema, and the web Zod contract consumes it.
V4 picks contain `fit`, real public `declarations`, and `limitations`. They carry no
method proposals, coverage table, or joint solution proof. Existing v1-v3 tasks are
parsed under their original versions. Backend and web must deploy together.

Server task requirements retain the original question and mark `selection_basis`:
`declaration` for v4, `manual` for direct selection and new author tests; the old
coverage basis remains the omitted default. Manual requests have no match task or
artifact ID. Capability mappings describe the author's allowed range, not proven
coverage. Existing runner input/tool/data/approval/result checks still apply.
Incubation remains explicit and uses the locked runtime-incubation/v2 provider;
v4 sends only quoted user requirements, empty proposals and empty coverage gaps.

## 各版在用情况与退役条件（SDD 0052 第三节，2026-11-06 第一次整理）

只读整理，没有删任何一版。**退役条件**统一两句：① 本仓代码不再读写这一版；② 线上没有还钉着这一版的进行中数据（委托、测试、确认快照）。**退役顺序**：使用方先删锁和读写代码并发版，提供方最后删 schema 与入口。提供方出新版时，在同一笔提交里把旧版这一行的退役条件写上。

| 契约与版本 | 提供方 | 本仓谁在用 | 现在谁还会出这一版 | 退役条件（第 ② 句查哪里） | 状态 |
| --- | --- | --- | --- | --- | --- |
| `knowbot-manifest/v2` | knowmart | `domain/workbench/knowbots.py` `pack_from_manifest` | 固定专家（财务分析与预测等） | 所有者定固定专家全部改成别的清单形态之前不退；查 `workbench_tasks.pack_snapshot` 不带 `runtime_contract` 的进行中委托 | 在用（固定专家保留，11-03） |
| `knowbot-manifest/v3` | knowmart | `knowbots.py` `catalog_from_manifest` | 动态专家 | 现行版 | 在用 |
| `dynamic-methods/v1` | 本仓（knowlab 持锁） | `domain/workbench/method_catalog.py` | 工作稿只有规划说明、没引方法库也没写能力声明 | 新的动态专家一律要求写能力声明（v3）之后；查 `pack_snapshot->>'runtime_contract' = 'knowbench/dynamic-methods/v1'` 的进行中委托与 knowmart 在售版本 | 在用，**可退候选**（要所有者定是否强制 v3） |
| `dynamic-methods/v2` | 本仓 | 同上 | 工作稿引了方法库的方法、没写能力声明 | 同上，`…/v2` | 在用，**可退候选**（同上） |
| `dynamic-methods/v3` | 本仓 | 同上 | 写了能力声明的动态专家 | 现行版 | 在用 |
| `author-method/v1`（`method-definition/v1`） | knowlab | `domain/workbench/method_catalog.py` 读冻结的方法来源 | 方法库里没有可选输入的方法 | 冻结在已发布专家里的引用都换成新版之后；查 knowmart 在售版本的 `method_sources` | 在用 |
| `author-method/v2`（`method-definition/v2`） | knowlab | 同上 | 方法库里有可选输入的方法 | 同上 | 在用 |
| `author-method/v3`（`method-definition/v3`） | knowlab | 同上 | 原子方法 | 现行版 | 在用 |
| `knowbot-test-run/v1` | knowlab | `application/workbench/test_runs.py` | 固定专家与 dynamic-methods v1、v2 的测试 | 固定专家测试取值也按回执走（SDD 0050 第三节 #6）、且 v1/v2 动态专家退役之后；查 `workbench_test_runs` 里 `request->>'contract'` 为 v1 的进行中测试 | 在用 |
| `knowbot-test-run/v2` | knowlab | 同上 | dynamic-methods v3 的测试 | 现行版 | 在用 |
| `knowbot-incubation/v1`、`runtime-incubation/v2`、`knowbot-test-execution/v1` | knowlab | 孵化运行方、测试执行 | 现行 | 现行版 | 在用 |
| `expert-directory/v2` | knowmart | `domain/workbench/expert_match.py` | 现行 | 现行版；锁测试含逐字段比对 | 在用 |
| `data-request/v1`、`retrieval`（knowbase） | knowbase | 第 0 步数据、检索 | 现行 | 现行版 | 在用 |
| `knowfeed-supply-prematch/v1`、`knowfeed-supply-status/v1` | knowfeed | 确认前预切合、需求状态接收 | 现行 | prematch 随 SDD 0051 出 v2 时写 v1 的退役条件 | 在用 |
