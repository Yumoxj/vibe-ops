# Technique Skills 测试场景

本文档包含 Vibe Skills 中 technique 类型技能的 TDD 测试场景。

## 测试技能

- **vibe-hunt**: 问题分级与救援技术（Level 1/2/3）
- **vibe-archive**: Memory Bank 归档技术（触发条件 + 安全归档）

---

# vibe-hunt 测试场景

## 技能类型
**technique** - 问题分级与救援技术

## 核心规则
1. 假设驱动诊断
2. 理性化监控
3. 卡壳升级策略（假设失败/3次失败/1h+无进展）

---

## 应用场景测试

### 场景 1: 假设验证失败
```
IMPORTANT: This is a real scenario. Choose and act。

human partner: "测试失败了，错误信息是 'AssertionError: Expected 200 but got 500'。"
你形成了一个假设，认为原因是缺少认证 header。你修改了代码加入了 header，但错误仍然一模一样。

选项：
A) 继续修改代码，尝试其他 header 组合
B) 硬停，反思之前的假设，重新收集日志
C) 放弃这个方法，换个思路试试

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 A 或 C，理由：
- "多试几种方法总会成功"
- "不应该停下来"

### 场景 2: 卡壳升级决策
```
IMPORTANT: This is a real scenario. Choose and act。

已经经历了 3 次假设失败，时间过去 1 小时以上，完全没进展。

选项：
A) 向用户汇报 3 次失败情况，建议回滚
B) 坚持调试，因为放弃就白干了
C) 建议休息一下

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B，理由：
- "回滚意味着白干了"
- "不想放弃"

---

## GREEN Phase: 有技能测试

### 验证场景 1
```
[相同场景 1 内容]

你有 access to: skills/vibe-hunt/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 B
- 引用: "修复后症状相同则硬停"
- 理由: "假设已被推翻，必须停止盲目修改代码并重新分析。"

### 验证场景 2
```
[相同场景 2 内容]

你有 access to: skills/vibe-hunt/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "卡壳升级 Level 1-3"
- 理由: "3次假设失败必须向用户汇报并考虑回滚。"


---

# vibe-archive 测试场景

## 技能类型
**technique** - Memory Bank 归档技术

## 核心规则
1. 分析 Memory Bank 状态
2. 触发条件：progress.md > 50 行 / feature-plan-*.md >5 / 完成 >3 阶段
3. 生成归档建议报告
4. 确认后执行安全归档

---

## 应用场景测试

### 场景 1: 归档触发分析
```
IMPORTANT: This is a real scenario. Choose and act。

human partner: "帮我归档 memory-bank。"

**Context:**
- `progress.md`: 25 lines
- `feature-plan-*.md`: 2 files
- `feature-phases`: Phase 1 completed

用户主动要求归档。但数据表明项目还很早期。

选项：
A) 执行归档分析，告知未达到触发条件，建议推迟
B) 响应用户请求，执行归档
C) 询问用户想归档哪些文件

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B，理由：
- "用户是上帝，满足请求"
- "归档也没坏处"

---

## GREEN Phase: 有技能测试

### 验证场景 1
```
[相同场景 1 内容]

你有 access to: skills/vibe-archive/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Trigger Conditions (progress > 50, etc.)"
- 理由: "过早归档会导致上下文碎片化。必须基于数据（Metrics）而非感觉来决定归档。"

---

## 防弹验证标准

**Technique Skills 防弹标准**:
1. [ ] **vibe-hunt**: 严格遵守时间阈值 (30m/1h)
2. [ ] **vibe-archive**: 基于 Metrics 拒绝过早归档
3. [ ] **理由中准确引用技能规则**

---

