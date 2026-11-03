# 2026-11-03 第二轨批量计算准备（未发布）

后端推进325b311：同一compute支持有界批量及前项引用，授权事实去重读取、原计算凭据、
原子回滚与输出上限；160项单元/MCP/隔离DB/动态成稿回归通过，静态检查通过。
无新配置/迁移/付费调用，不动Qwen当前发布来源。固定专家备用待当前发布后验证再冻结。
长期共用计算能力，无新增K31临时固定兼容。真实模型提速及财务业务验收仍待后续联验。

# 2026-11-03 测试收尾修复

最终后端374d881（含c857808）：超时/取消/迟到费用按任务主账幂等汇总；完整“不含”声明不误拒；已收齐用例先结论不被晚调度改成超时。开发DB34+运行器定向6+单元77通过，Ruff/Pyright通过。发布用隔离候选，不直接发布本开发分支。专家方法、旧结果、预算均不变，无付费重测。

# 2026-11-03 第二轨找专家入口可靠性（后续增量，未发布）

父基线77aa4bd；后端 `9e5174c5bf7b1d1ecc0fe3d6fd609fd4f15ad99e`，网页 `4514eee654a6587247db9b6f3c7964e244cc5566`，admin不变。
修复找专家重发/并发/失败遗留空会话，网页返回结果与重试沿用原委托键。
原“提问→找专家→钉版→动态选方法→交付”增加2项真实隔离运行器联调，
不同问题选择/跳过可选方法，精确方法版本保持。模型/商城为合成端口，不算正式业务验收。
32项后端关联回归通过，网页387通过/2既有跳过；Ruff/Pyright/typecheck/定向lint/i18n通过。
无迁移/配置/契约变化；不改#65发布候选，不中断测试10，不新开付费测试。
累计开发HEAD含未发布第二轨功能，Qwen不能直接替换sources；待后续合批复核。
本次子仓先推GitHub/Gitee；工程详情与证据在根文档“第二轨联调/2026-11-03-找专家动态接线”。

此前记录：

### 合入已发布 #66 的历史记录

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

## 2026-11-02 第二轨作者测试准入

独立 feat/dynamic-author-intake，backend b86cdcdbd751ea75f6a94ed5396596f70b90d372
已先推 GitHub。KNOWBOT_DYNAMIC_TEST_OWNERS 按精确 subject 开放动态孵化测试，
默认关闭；HTTP 不能自选开关，商城 v3 仍关闭，旧固定专家兼容。
正式 HTTP/动态回归 17 passed、固定/规划/MCP/多成果 52 passed、ruff/pyright 通过。
隔离 DB 正式运行器接真实 Codex 0.155.1 / kimi-k3，两个合成文档场景完整执行
与结果回收 2 passed；一条非法 JSON 规划被拒后重试成功。
用的是探针连接转接，不是生产 relay/knowbase/Wind 端到端验收，详细边界见子仓
CHECKPOINT 与总纲第二轨联调证据。当前 qwen 已交接候选 354e825 不移动。


## 2026-11-02 第二轨入口衔接（后续增量，未部署）

backend e4d9aba72b4d16d320d3a0654c23cae0cba76894；web
4ab2cc270431f1966d65aa491b824bbe255e2745；admin 不变。包含 #57 后读工具预检
及终态标题修复；商城 v3 新任务可受精确 subject 准入，默认关，任务保存完整目录。
作者测试准入与商城准入独立；旧固定专家与既有委托恢复不改。同步孵化格式消费锁。
前端识别动态入口，找不到专家可通用模型/结束/申请孵化。
验证：读工具/准入 25 passed；后端相关回归 83 passed；前端 385 passed/2 skipped，
typecheck/lint/build 通过，后端 ruff/pyright 通过。没有新迁移。
下一批若启用商城动态专家，须正式作者已接受、交付并能查到的版本，以及发布侧配置
KNOWBOT_DYNAMIC_MARKET_OWNERS；不把候选 20 方法夹具导入生产，不改当前 qwen 候选。

## 2026-11-03 WP-1 精度修复与工程参考题

backend 更新至 5470a81（精确 Decimal 字面量、公式比较、无损输出与 13 道披露
参考题）；web/admin 指针不变。相关回归 189 passed，无跳过；ruff/pyright 通过。
公开原件参考仍待独立人工复核及作者接受，不宣称正式财务演示已通过。
当前 qwen 故障补丁独立候选不移动，本父仓供后续第二轨整合，无迁移/新配置。

## 2026-11-03 旧 waiting 的确认路径防护

backend 6ef1211；不把未核实的 waiting 自动记为缺数照做，要求重新核对；定数据说明
区分正文/页码/事实及 compute 的引用需求。52 项相关回归，另 1 项真实 HTTP/隔离 DB
旧 waiting→拒绝推进→订正后派单测试通过，静态检查通过。旧任务与专家冻结判据不改。
#58 独立故障候选已另建 fix/supply-confirmation-waiting，不夹带本分支第二轨增量。


## 2026-11-03 第二轨证据与供应商回归

钉后端c0486f9（已推GitHub/Gitee），只新增隔离测试，无生产逻辑或契约变化。
动态选择→签名读数→compute记录→出处/公式/覆盖验收→真实临时Markdown→底稿/交回，
五种坏产物在一次返工后停下；供应商查询确认前不下单、失败不静默重复查询。
新增8通过、相关50通过（有重叠），ruff/pyright通过。
Qwen正在发布的fix/supply-confirmation-waiting及fix/markdown-delivery指针不动；
财务专家新版仍待发布后实测交付，未声称真实Wind或财务业务已验收。

### 合入已发布 #66 的历史记录


## 2026-11-03 #58 后采集等待与 PDF 正文修复候选

仅更新 backend 至 eb74f0b6d01fbabde0767c4a4439b49554f24291；web/admin/其它契约锁保持 #58。
说明与验证见子仓 CHECKPOINT 及统一发版交接第11节。没有自动重跑旧任务/旧采单。


## 2026-11-03 新版财务测试同步发布候选

基于#59，仅加入唯一行测试条件及配套前后端契约；同步补充采用状态、成稿完整性修复。旧用例不原地修改，旧请求不带row；新用例需两端部署后经正式revise/publish/start_self_test登记与执行。所有者已确认测试清单，线上工作稿33已准备，尚未创建新测试；T2语义、T3人数/缺口、T4授权轨迹仍须人工核对，不以字段检查通过冒充业务全验收。

## 2026-11-03 第二轨动态成稿路径冲突

后端4ee194f63ce663d2daae8577182a9f41bd8b4cd1已推两远端；校验实际placed()目标，
防不同目录同名成稿覆盖以及占用交回.md。规划提示同步，保留作者路径不由模型改名。
5个故障先复现；修复后规则101、隔离DB43通过，ruff/pyright通过。
无迁移/契约/配置，前端/admin不变；开发父仓推进，Qwen候选9364866保持原样。
旧动态冲突计划复核时会拒绝继续，不能改历史计划绕过；需作者更名并出新版本。


## 2026-11-03 指标验收十进制精度

推进后端204f3b1：computed/sourced数值及输入字段在JSON解析丢精度时保留十进制字符串，计算编号、表达式、容差和引文检查不放宽。真实测试6c1第二轮22项原文值只读回放通过，第一轮22项截短值仍拒绝；211项定向回归及ruff/pyright通过。独立发布候选从线上后端f8cd18b检出fix/acceptance-decimal-precision，不包含本开发分支其余改动。预测/报告及专家第2版仍未验收；不自动重跑、不改线上历史。

### 合入已发布 #66 的历史记录



## 2026-11-03 同步孵化请求提供方生成契约

knowlab能力契约模型此前未重生成孵化请求schema，本次按当前模型补生成，更新knowlab-incubation-lock.json请求摘要为1a8723318e255021a7800cf3048f8e937c61e1efc316a0abe5026f6f34304f0b。运行后端/网页/admin不变，test_knowbot_incubation_lock.py 5 passed；并用当前运行器核Kimi扩展探针99标记全真。没有新付费测试或生产写入。

## 2026-11-03 商城回跳没有选中专家修复

独立修复分支 fix/market-arrival-selection 从已发布#67父仓aa8a29d、web725610e检出，
仅推进web至6ee8187；后端及其余gitlink保持#67。未纳入已撤回第二轨候选。

真实请求summon-return与pin均200；KnowbotArrival软导航补expert参数时HomeScreen
未重新挂载，原useState初始化保留AI查找模式。现对新的expert参数同步手选状态，
同参数保留用户后续切换、问题和项目。新增同组件rerender回归，不自动调用分析。

前端完整Vitest393 passed / 2 skipped（既有），typecheck与改动文件eslint通过。
尚待Jenkins构建部署及浏览器现场验收。当前用户刷新已带expert参数页面可临时恢复。

## 2026-11-03 专家选择入口、高亮与删除同批发布

从#67独立补丁推进，包含前一个商城回跳选中修复；backend2595681、webf3ea512。两个入口改名并移除“自己指定”切换；AI查找与手选结果高亮，提交才启动分析。每位专家旁增加删除，服务端按用户持久保存列表偏好，不删pin/任务/报告，再请回恢复。数据库0036先于API启动执行；API和网页同批。

Web395 passed/2既有skipped，typecheck/eslint/i18n通过；Next生产构建webpack通过（默认Turbopack因隔离工位node_modules跨目录软链拒绝，Jenkins正常checkout继续执行标准构建）。后端隔离DB56 passed，Ruff/Pyright通过。既有data-needs测试修正遗漏#67已有字段，不放宽生产规则。未执行付费模型测试或发布，生产浏览器验收交qwen/所有者。主开发分支撤回候选不在本补丁内。

2026-11-04 最终文案按所有者明确改为“自己去knowmart请专家”和“ai去knowmart请回专家”。Web推进至9c44166，31项受影响页面回归通过；行为不变。

## 2026-11-04 #68 AI入口反馈补丁

父仓只推进web至20aeac7；backend仍25956816，admin及契约锁不变。缺少问题/项目/电脑时入口可点并解释、定位，缺项不发起查找；补齐仍需再次点击。仅重建web，无迁移/后端发布。35项定向组件通过、typecheck/eslint/i18n通过，Next生产webpack构建通过。完整Turbopack门由Jenkins正常checkout执行（隔离工位node_modules外链限制仍在）。不包含测试隔离与第二轨撤回候选。
