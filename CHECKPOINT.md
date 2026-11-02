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
