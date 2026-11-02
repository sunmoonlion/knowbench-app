# 2026-11-02 采集交付失败与原任务恢复

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


## 2026-11-02 #49 重新核对修复候选（fix/recheck-contract）

从已发布父仓 2f08c699 分出，不混入后续动态执行功能。后端 6d2eba5 区分上游缺路由/协议/鉴权错误与真正的采单拒绝；前端 7685f28 显示数据服务问题并保留原采单。全量前端发现切换账号时清缓存早于卸载旧观察者的时序问题，同步修复，旧账号表单仍销毁。

验证：后端全量首次 1296 passed / 106 skipped / 1 failed / 2 既有警告，唯一失败因隔离工位没检出 knowlab 契约文件；补齐 #49 的 knowlab-app f47f401 后，含该目录锁及完整 expert_data、knowbase_client 的 58 项通过。保留首次失败日志，不声称首跑全绿。后端 ruff/pyright 通过。前端全量 375 passed / 2 skipped、typecheck/lint/i18n（1073 键）/生产 build 通过；第一次构建 node_modules 指向工位外导致 Turbopack 拒绝，改为独立离线 frozen-lockfile 安装后构建通过，未更改构建配置。

原始输出 /tmp/luna-recheck-bench-tests.log、/tmp/luna-recheck-bench-final-targeted.log、/tmp/luna-recheck-web-tests-final.log、/tmp/luna-recheck-web-build-final.log。迁移头仍 20261102_0035，无新增迁移。必须与补齐本人 refresh 路由的 knowbase 一起交 qwen 经现有 Jenkins 发布。用户暂停验收，未确认、续跑、重试或停止线上旧任务；Wind 无新增调用。
