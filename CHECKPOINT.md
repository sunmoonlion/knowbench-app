# 2026-11-03 test10修复候选（接#65）

最终后端8527273（含64aacf3），基线657e456；仅后端API/runner升级，web/admin不变，无迁移或配置。修复完整“不含”非建议声明误拒、整轮超时漏算已用费用、迟到费用幂等汇总，以及用例已收齐后晚调度误判超时。配套knowlab前端4669c4a将整轮时限交作者设置。候选初轮真实DB69项（含runner）及单元77项通过；费用只读投影补充后重跑真实DB34项，Ruff/Pyright通过。不得从累计开发分支替代本gitlink。Qwen/Jenkins发布，所有活动任务检查后维护窗口执行。

# 2026-11-02 #57 后专家数据工具与终态修复候选

基线 354e8252f040e83c4ea61fc163a55e43cabaeeb4（#57）。只推进 backend/web；admin 与 contracts 保持 #57。后端执行前核线程真实数据工具，缺失不发模型；网页失败/停止不统一显示“专家交回了”。当前旧任务实际 FAILED，弱验收没有被偷偷改成新规则。根仓统一 Wind/年报交接第九节给出入口声明、九工具、旧线程刷新和本人验收要求；发版仍交 qwen/Jenkins。无迁移/新配置。不夹带 b86cdcdb 作者准入或未提交的商城动态受理。

# 2026-11-02 第二轨 Wind 通用查询与年报证据候选

基线 `f3534e7708ec184466417fa1e79c92857806bda0`，子仓先推 GitHub，再钉本仓。

- knowbench-backend: `cb597459ef3ce0a9507657e2cf55609c0f924f8e`
- knowbench-web-frontend: `cf96b723ed69df9eb8b4c093c104ae58463df470`

可选 source_query 契约/消费锁同步，既有固定专家和巨潮原件路径保留。财务、公告年报等能力按官方七服务域发现；供应商文本/结构化回执不冒充 PDF 或页码。测试、初始失败和剩余门见子仓 CHECKPOINT 与 sunmoonun 的 Wind-W0评估续记。当前未生产启用，发布仍由 qwen 走 Jenkins。

#52 已发布修复在 knowbase/knowbench 子仓成为祖先；源码与已验证候选相同。实际第二轨演示还待正式服务配置、真实作者/方法与数据读工具链验证，不把取数探针当成业务通过。

以下是之前历史：

# 2026-11-02 采集交付失败与原任务恢复

## 最新开发增量：补采并发提交（未发布）

feat/method-execution-view 后端已推
`2b894280ce2cb3f6882c5b954e58725b09735030`，前端保持 `06f9026`。
提供方配套 knowbase-backend `971747286c2b2329c8738f5735e8d76321651cc9`：
解析过期基版冲突明确返回，本地提采重读追加，拒重复编号和取消后迟到回包，
作者测试原子编号，数据收尾锁序对齐。无新增协议字段/迁移；provider README 已说明
409 语义。全量 **1411 passed / 92 skipped / 2 既有 warnings**、ruff/pyright 通过，
日志 `/tmp/luna-proposal-race-full.txt`。详情和初次测试夹具修正记录见后端 CHECKPOINT。

未覆盖远程回包丢失的自动恢复与本人确认/修改和新增请求的完整并发合并，不称全链路
幂等通过。没有发布、改 sources 或动线上采单；qwen 的 release/recovery-methods
仍为 2f08c6999feb03f3005393c81f430513ea197ad7。

基线：已发布 #46，父仓 20427cc415c3e42e9d2bc1bcd82c14d95d72b41b。候选分支 fix/supply-delivery；发布交由 qwen，luna 继续开发，不触发 Jenkins。

任务 9319ae09-c8b7-4c3b-844e-27306e9e98d8 已确认后遇到 knowfeed→knowbase 交付 422。后端已有 DataWaiter 查询兜底，缺的是准确展示与失败后的正常恢复入口：供应进度快照只保留公开描述/状态/原因/核对时间，不带 receipt；WAITING(DATA) 区分等数据服务和等本人处理；missing 可重新核对并转 pending，保持原任务与 needs，新轮确认使用独立幂等键，重复核对不换键，重复确认不重复派单。

前端按主体/报告种类/读取形式分组、合并期间；显示失败原因、最近核对时间、原任务重新核对入口。没有对话事件时，现有 query 每 10 秒读取等待中的采单快照与 DATA 等待原因；采单办结停止该快照轮询。后台仍按现有退避查询 knowfeed，最长 5 分钟；不把前端刷新频率当成供应端推送时延。

后端最终：1206 passed、92 skipped、2 个既有 collection warnings；ruff/pyright 通过。证据 /tmp/luna-workbench-supply-final.txt、/tmp/luna-wait-pyright-final.txt。
前端最终 pnpm check：359 passed、2 skipped，类型/lint/i18n/生产构建通过。证据 /tmp/luna-supply-ui-check-final4.txt。浏览器 1366×768、1024×640 步骤栏可见且可点；截图 /tmp/luna-supply-progress.png 是组件场景，不是线上财务闭环验收。

线上验收由 qwen 发版后执行：原任务刷新 → 本人重新核对采单 → 本人确认重试 → 核验新需求关联、交付响应、数据钉住与专家续跑。不得代用户签确认、改 DB 或重建任务掩盖历史。旧单保持记录；保底仍走巨潮，预测口径按用户最新决定为 2026Q4、2027Q1。完整财务报告尚未验收。

## 第二轨独立分支：WP-10 单元 1

上述发布候选保持原分支；从候选另建 feat/dynamic-methods，后端 cbb6cd40e93a3819cca2fb5839caa1df09d77441。新增方法目录 provider schema（dynamic-methods/v1），外层 knowmart v3 锁及孵化 v1 锁。方法目录独立于 ExpertPack.workflow，完整校验方法/依赖/必需成果/次数/权限；动态执行入口明确关闭，避免误走固定调度。前端未变。

完整回归 1244 passed / 92 skipped，其后最终契约补充重跑受影响六文件 81 passed；最终全仓 ruff/pyright 通过，详细时点/命令见后端 CHECKPOINT.md。下一单元：版本化任务计划、确定性校验与事务接受；再接 Attempt/有效计划视图/UI，最后重新孵化作者确认的财务专家。无 Wind 调用、无部署、未修改保底专家或 qwen 发布分支。


## 第二轨 WP-10 单元 2：计划保存与校验

后端钉 6e54339d946d53b83f863d3863edf42c3d9221be，已先推 GitHub feat/dynamic-methods。本父仓只推进后端引用和检查点，前端与 provider schemas 未变；未更新任何发布源。

新增任务计划模型、确定性校验及既有 Ledger 内的规划 Attempt/预算/事务接受。计划存 Artifact 版本，Task 指向确切已接受版，作者目录快照不变；并发接受幂等、取消不复活、输入验收版本核对、已开始前缀保护、失败计费/计轮次与文件冲突均有测试。迁移头 20261102_0035，仅新增可空正整数指针；有接受历史时禁止破坏性 downgrade。回退应用须保留扩展 schema。

后端全量 1289 passed / 92 skipped，之后补测试的相关三文件最终 54 passed；ruff/pyright 通过。详情、命令和原始输出位置见后端 CHECKPOINT.md。动态专家仍不能运行，尚无新增前端入口。

所有者确认方法独立版本、专家精确引用、任务动态组合，已记 sunmoonun b01e7f18 的 methods.md/decisions.md 与双轨 WP-10。新增单元 1a：先在 knowlab 做作者方法主档、输入槽与专家连接分离、版本引用/升级、三方契约及快照校验；再接动态 advisor/权限/视图/验收。当前目录只属于专家包，不可称为跨专家方法库。发版交 qwen，保底巨潮链和 Wind 3/5 限额不变。


## 第二轨 WP-10 单元 3：独立开发分支进度

`feat/method-execution-view` 后端钉 `b6784a7493956d026e89377f8f91200f68349263`，已先推 GitHub。包含 `0e0917a` 的任务执行视图、输入/成果绑定与取消竞态修复，以及本段有界规划服务、父版本重试守卫和动态 covers 固定输入验收。前端、provider schema、发布源未改；`release/recovery-methods` 仍由 qwen 按合并单发布，本开发不增加发版单。

完整后端 1344 passed / 92 skipped / 2 既有 collection warnings；全量后最后补的规划重试守卫，五文件最终 139 项通过；最终 ruff/pyright 通过。记录与复跑说明见后端 CHECKPOINT.md 和 docs/method-execution.md。无新增迁移、Wind 调用、线上任务/采单变更。

规划复用 TurnDriver 端口，但 Advisor/Runner 真实入口及工具鉴权、页面、最终交付尚未整体接通；测试 Driver 是替身，v3 仍拒绝准入。尤其须补可信的每轮调用身份，覆盖 MCP conversation/market/NO_SCOPE 路径，不能把提示词允许工具当强制权限。下一段继续接线与完整生命周期测试。


## 第二轨 WP-10 单元 3：执行循环、读取面与工具鉴权

独立 feat/method-execution-view：后端 ee3c40c09d2cffa6ce37773411c5f610fe2a2138（包含第三段 fa31f31、第四段 4b94cdc），前端 06f90264ae060bf2de239211cbe4d5f0916631ef，子仓均已先推 GitHub。本父仓只钉开发成果，不动 qwen 的 release/recovery-methods、发布源或 Jenkins。

Advisor 已按冻结方法计划执行并原子交付全部最终成果；返工显式 pin 失效走受限重规划。页面展示计划理由/版本及当前有效成果，保留历史但不混用；固定专家 HTTP 结构保持不变。MCP 按真实轮次、Runner 观察的工具事件、任务绑定与工具子集强制核验，覆盖跨任务/对话/市场/no-scope 和取消后绕过；原生审批不扩大作者 deny。规划带有界数据元信息，不带授权凭据/未读正文。

后端全量 1396 passed / 92 skipped / 2 既有 warnings；全量后补充的上下文与 Runner→MCP→数据授权跨层测试，四文件最终 44 passed。前端完整 377 passed / 2 skipped，typecheck/lint/i18n/build 通过。后端 ruff/pyright 通过；固定 0.155.1 隔离协议探针通过，无真实模型/金融数据调用。命令和最初失败记录见子仓 CHECKPOINT 与 docs/method-execution.md。

无新迁移，保留 0025/0035 历史保护；v3 外部准入仍关闭。agy 负责作者方法库内容/UI；下一步准备动态孵化测试的稳定断言和多成果回执契约，然后联合真实沙箱验收。Wind 仍 3/5、W0 未决，保底巨潮不改。


## 2026-11-02 同步 #49 重新核对修复

开发分支同步后端 aeca77f / 前端 fb942ff；上游路由故障不再转成用户采单错误，页面保留采单并提示稍后再试。保留第二轨原有 resolution_changed 与 CAS 逻辑。切换账号的缓存清理改为旧页面卸载后执行。

定向后端 recheck 5 passed，前端 panel/session 18 passed；前端 typecheck、后端 ruff/pyright 通过。发布使用独立 fix/recheck-contract 候选，不使用本开发分支；线上任务未动。


## 2026-11-02 WP-10：接通受控动态作者测试

钉后端 24b6415，先推子仓 GitHub。测试受理服务允许构造期显式注入动态联调能力，
生产 bootstrap 保持默认关闭；受理/Conductor/Ledger/Runner/多成果回收复用现有链。
实际引用目录在隔离数据库及脚本 AppServer 下跑两步/三步计划，未改写任务快照。
新受理/旧固定测试/账房/成果回归 52 passed，ruff/pyright 通过；此前跨端快照相关
111 passed。未重复后端全量，未调用真实模型/金融数据。细节及回退见后端 CHECKPOINT。
此批只做开发，不替 qwen 发版，不影响独立 recheck 修复候选或本人暂停的业务验收。

# 发布祖先核对

父仓亦合入 #52 f69fe1c3，仅历史与记录对齐；子仓保持本轮已验证 HEAD，未覆盖 qwen 的部署分支。

## 2026-11-02 同批纳入 #52 数据工具目录修复

后端 15643e299ab0e0a6a6c9df2ab700bf95d2be7800 已推 GitHub；父仓钉该指针，
前端不变。与 sunmoonun 4e950b3c 的内部 HTTPS /api/mcp/data Exact 路由同批发布。
修复已配置提供方故障被吞为部分工具目录、空目录缓存的问题；保持原用户授权守卫。
相关 124 passed、提供方 8 passed、真实 JWT 目录对接 9 工具通过、ruff/pyright 通过。
证据与未覆盖边界见后端 CHECKPOINT；仍待 qwen 原生发布及旧线程正式 MCP 刷新检查，
不操作旧任务/确认单/等待提问、不新增 Wind 调用、不宣称业务验收完成。


## 2026-11-02：合入 kimi 方法验收修复

合入 04de69d，后端 31e3701；保留 Luna 的 #52 修复及现有前端 gitlink。
通用验收增加目标值、计算输入绑定、行非空及联合唯一约束，provider v1/v2 同步生成。
在 Luna 与最新各 provider 并列后，验收/计算/目录/计划 234 passed（无跳过）；
隔离数据库 method_acceptance 2 passed。新字段在旧运行器 extra=forbid 下明确拒绝，
使用新规则前需要编译器和运行器共同升级。未导入 20 个合成方法为线上资产。


## 2026-11-03 #58 后采集等待与 PDF 正文修复候选

仅更新 backend 至 eb74f0b6d01fbabde0767c4a4439b49554f24291；web/admin/其它契约锁保持 #58。
说明与验证见子仓 CHECKPOINT 及统一发版交接第11节。没有自动重跑旧任务/旧采单。


## 2026-11-03 新版财务测试同步发布候选

基于#59，仅加入唯一行测试条件及配套前后端契约；同步补充采用状态、成稿完整性修复。旧用例不原地修改，旧请求不带row；新用例需两端部署后经正式revise/publish/start_self_test登记与执行。所有者已确认测试清单，线上工作稿33已准备，尚未创建新测试；T2语义、T3人数/缺口、T4授权轨迹仍须人工核对，不以字段检查通过冒充业务全验收。
