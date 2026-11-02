# 精确引用方法目录 v2（WP-10 开发契约）

真源为 `knowbench-backend/app/app/domain/workbench/method_catalog.py` 的 `ReferencedMethodCatalog`，`render(referenced=True)` 生成 schema。v1 保留原样。v2 显式钉 `method_compiler=knowlab/workdraft-compiler/v1`；未知编译器拒绝。仍无固定 workflow，不包含 Task 计划。

`method_sources` 以专家内别名为键，含方法 UUID、正整数版本、定义摘要、命名槽连接及只读作者定义。定义真源与编译器属于 knowlab；消费锁为 `knowlab-author-method-lock.json`。消费者核摘要、版本、别名、输入连接与调用边界，整个专家清单摘要同时冻结编译结果。不能取最新方法替换历史，也不能将 v2 runtime 降标 v1 后丢来源。

动态执行仍未开放。该目录仅供编译、交付与 Task 计划工程验证，不能作为动态专家上线验收。
