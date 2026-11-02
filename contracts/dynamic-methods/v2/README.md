# 精确引用方法目录 v2（WP-10 开发契约）

真源为 `knowbench-backend/app/app/domain/workbench/method_catalog.py` 的 `ReferencedMethodCatalog`，`render(referenced=True)` 生成 schema。v1 保留原样。v2 显式钉 `method_compiler=knowlab/workdraft-compiler/v1`；未知编译器拒绝。仍无固定 workflow，不包含 Task 计划。

`method_sources` 以专家内别名为键，含方法 UUID、正整数版本、定义摘要、命名槽连接及只读作者定义。定义真源与编译器属于 knowlab；消费锁为 `knowlab-author-method-lock.json`。消费者核摘要、版本、别名、输入连接与调用边界，整个专家清单摘要同时冻结编译结果。不能取最新方法替换历史，也不能将 v2 runtime 降标 v1 后丢来源。

动态执行仍未开放。该目录仅供编译、交付与 Task 计划工程验证，不能作为动态专家上线验收。


### 可选验收约束（2026-11-02）

`computed.expected_value` 指定有限数值目标；计算记录与交回的 value 均须在 tolerance
内。`computed.input_keys` 列出行内数值字段，必须与计算记录中同名、已解析操作数相符，
防止借用无关的正确结果为改过的行背书。只匹配单个数值，不支持列表操作数。
`each_has.non_empty_keys` 拒绝 null、空串/空白串、空列表/对象；0 和 false 不算空。
`each_has.unique_by` 约束行的联合标识，不允许空/非标量/非有限数值；1 与 1.0 相同，
布尔和数值不同。列表非空和最低覆盖仍需作者配置 list_min。

这些字段可选，旧规则序列化不变。旧运行器会拒绝包含未知字段的新目录，不会悄悄忽略。
发布使用这些字段的新方法前须升级 knowbench，并同步 knowlab 编译器、UI 与契约锁。
规则只核作者明确声明的条件，不证明完整业务语义，也不代替真实数据验收。
