# dynamic-methods/v3

提供方：knowbench。`catalog.schema.json` 由 `method_catalog.render(capabilities=True)` 生成。

新增冻结的 `public_capabilities`、私有 `planning.capability_bindings`、逐槽 `input_slots`，以及作者显式声明的 `when_input_present`。引用的独立方法允许 definition/v1 或 v2；v1 输入全部必需，旧摘要不改写。v3 使用 compiler/v2，任务使用 method-plan/v2。

计划绑定所有必需输入；可选输入只能因上游未选而省略，已选上游必须进入最终成果。条件验收不得控制 JSON、必填输出、安全检查。要求覆盖必须对照服务端持有的任务要求，不能以规划模型自报的覆盖作为权威。

本次完成契约、作者编译、计划校验及现有执行器投影。真实入口、需求收据和作者财务资产仍在后续开发；不得仅凭合成测试启用生产动态演示。旧 v1/v2 契约文件保持不变。
