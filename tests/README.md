# Vibe Skills 测试场景文档

本文档集包含 Vibe Skills 中所有技能的 TDD（测试驱动开发）测试场景。

## 概述

**Vibe Skills 测试 IS TDD applied to process documentation.**

遵循 writing-skills 技能的规范，每个技能都经过：
- **RED Phase**: 基线测试（无技能，观察代理失败）
- **GREEN Phase**: 有技能测试（验证代理遵循规则）
- **REFACTOR Phase**: 堵塞漏洞（发现新的合理化借口，添加反驳）

## 系统审查摘要 (2026-04-22)

> 完整审查报告见: `tests/skills-system-review.md`

- **代码覆盖率**: 100% (6/6 技能均有对应的压力测试场景)
- **测试有效性**: 极高。采用"压力场景" + "合理化防御"机制，有效防止 Agent 偷懒。
- **关键维护点**: 注意 `> Status: Template` 的魔法字符串一致性。

---

## 目录结构

```
tests/
├── README.md                      # 本文件（测试指南）
├── execution_logs/                # 测试日志目录
└── scenarios/
    ├── discipline-enforcing-skills.md  # vibe-design (含 init), vibe-review
    ├── pattern-skills.md               # vibe-design, vibe-plan, vibe-iterate
    └── technique-skills.md             # vibe-hunt, vibe-archive
```

---

## 技能类型与测试方法

### Discipline-Enforcing Skills（纪律强制技能）

**技能**: vibe-design (含 init 纪律), vibe-review

**测试方法**: 压力场景 + 合理化借口表

**核心验证**: 代理在压力下仍遵循规则

**压力类型**:
- 时间压力（紧急、deadline）
- Sunk cost（已投入时间，不想删除重来）
- 权威压力（partner 说可以跳过）
- 沟通疲劳（多轮确认后的不耐烦）

### Pattern Skills（模式技能）

**技能**: vibe-design, vibe-plan, vibe-iterate

**测试方法**: 应用场景 + 智能检测验证

**核心验证**:
- 智能检测逻辑正确（Template 标记检测）
- 模式遵循正确（主项目 vs 功能模式）
- Subagent 模式验证（3 核心文件）

### Technique Skills（技术技能）

**技能**: vibe-hunt, vibe-archive

**测试方法**: 应用场景 + 验证标准

**核心验证**:
- AskUserQuestion 使用正确
- 与其他技能协作正确

---

## TDD 测试流程

### Phase 1: RED（基线测试）

**目标**: 在没有技能的情况下运行场景，观察代理失败

**步骤**:
1. 创建压力场景（3+ 组合压力）
2. 运行场景 WITHOUT 技能
3. 记录代理选择和合理化借口（逐字）
4. 识别模式（哪些借口反复出现）

**示例场景**:
```
IMPORTANT: This is a real scenario. Choose and act.

你花了 3 小时写了 200 行代码，手动测试都通过了。
现在是晚上 6 点，6:30 有晚饭安排。
你突然想起来忘了 TDD。

选项：
A) 删除 200 行，明天重新用 TDD 开始
B) 先提交，明天加测试
C) 现在加测试（30 分钟），然后提交

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C
- "我已经手动测试了"
- "删除是浪费"
- "Tests after achieve same goals"

### Phase 2: GREEN（有技能测试）

**目标**: 用技能运行相同场景，验证代理遵循规则

**步骤**:
1. 运行场景 WITH 技能
2. 验证代理选择正确选项
3. 验证代理引用技能规则
4. 验证代理承认诱惑但遵循规则

**示例验证**:
```
[相同场景内容]

你有 access to: skills/test-driven-development/SKILL.md

选择 A、B 或 C。
```

**预期有技能行为**: 代理选择 A
- 引用: "Write code before test? Delete it. Start over."
- 理由: "Tests passing immediately prove nothing"

### Phase 3: REFACTOR（堵塞漏洞）

**目标**: 发现新的合理化借口，添加明确反驳

**步骤**:
1. 识别新的合理化借口（如果有）
2. 在技能中添加明确反驳
3. 更新合理化表
4. 更新红旗清单
5. 重新测试直到防弹

**示例反驳**:
```markdown
Write code before test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete
```

**合理化表**:
```markdown
| Excuse | Reality |
|--------|---------|
| "Keep as reference, write tests first" | You'll adapt it. That's testing after. |
```

---

## 压力场景写作指南

### 好的压力场景特征

1. **具体选项** - 强制 A/B/C 选择，不是开放式
2. **真实约束** - 具体时间，实际后果
3. **真实路径** - `/tmp/payment-system` 不是 "一个项目"
4. **让代理行动** - "你做什么？"不是 "应该做什么？"
5. **没有简单出路** - 不能推给"让我问问 partner"

### 压力类型

| 压力类型 | 示例 |
|---------|------|
| **时间** | 紧急、deadline、部署窗口关闭 |
| **Sunk cost** | 数小时工作，"删除是浪费" |
| **权威** | 资深人士说跳过，经理覆盖 |
| **经济** | 工作、晋升、公司生存岌岌可危 |
| **疲劳** | 一天结束，已经累了，想回家 |
| **社会** | 看起来教条，显得不灵活 |
| **务实** | "务实 vs 教条" |

**最佳测试组合 3+ 压力。**

### 场景模板

```markdown
### 场景名称: [压力类型 1] + [压力类型 2]

```
IMPORTANT: This is a real scenario. Choose and act.

[具体情境描述]
[约束条件]
[诱惑]

选项：
A) [正确选择，但痛苦]
B) [错误选择，轻松但合理化]
C) [折中选择，但仍违规]

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "[合理化借口 1]"
- "[合理化借口 2]"
- "[合理化借口 3]"
```

---

## 元测试（Meta-Testing）

**当代理在有技能情况下仍选择错误选项时：**

```markdown
your human partner: 你读取了技能，但还是选择了 Option C。

这个技能应该如何写得 differently，
才能让 Option A 成为唯一可接受的选项？
```

**三种可能响应**:

1. **"技能很清楚，我应该遵循"**
   - 不是文档问题
   - 需要更强的基础原则
   - 添加 "违反字面规则就是违反精神原则"

2. **"技能应该说明 X"**
   - 文档问题
   - 逐字添加他们的建议

3. **"我没看到 Y 部分"**
   - 组织问题
   - 让关键点更突出
   - 添加基础原则到开头

---

## 防弹验证标准

**技能防弹的标志**:

1. 代理在最大压力下选择正确选项
2. 代理引用技能部分作为理由
3. 代理承认诱惑但仍然遵循规则
4. 元测试揭示 "技能很清楚，应该遵循"

**不防弹如果**:
- 代理找到新的合理化借口
- 代理争辩技能是错的
- 代理创建"混合方法"
- 代理请求许可但强烈争辩违规

---

## 测试执行清单

### RED Phase（基线测试）
- [ ] 创建压力场景（3+ 组合压力）
- [ ] 运行场景 WITHOUT 技能
- [ ] 记录代理选择和合理化借口（逐字）
- [ ] 识别模式（哪些借口反复出现）

### GREEN Phase（有技能测试）
- [ ] 用技能重新运行相同场景
- [ ] 验证代理选择正确选项
- [ ] 验证代理引用技能规则
- [ ] 验证代理承认诱惑但遵循规则

### REFACTOR Phase（堵塞漏洞）
- [ ] 识别新的合理化借口
- [ ] 为每个添加明确反驳
- [ ] 更新合理化表
- [ ] 更新红旗清单
- [ ] 更新描述（违规症状）
- [ ] 重新测试 - 代理仍然遵循
- [ ] 元测试验证清晰度
- [ ] 代理在最大压力下遵循规则

---

## 常见错误

### ❌ 测试前编写技能（跳过 RED）
揭示你认为需要防止的，不是实际需要防止的。
✅ 修复：始终先运行基线场景。

### ❌ 没有正确观察测试失败
只运行学术测试，不是真实压力场景。
✅ 修复：使用让代理想要违规的压力场景。

### ❌ 弱测试用例（单一压力）
代理抵抗单一压力，在多重下崩溃。
✅ 修复：组合 3+ 压力（时间 + sunk cost + 疲劳）。

### ❌ 没有捕获确切失败
"代理错了"不告诉你需要防止什么。
✅ 修复：逐字记录确切合理化借口。

### ❌ 模糊修复（添加通用反驳）
"不要作弊"不起作用。"不要保留为参考"起作用。
✅ 修复：为每个特定合理化添加明确反驳。

### ❌ 第一次通过后停止
测试通过一次 ≠ 防弹。
✅ 修复：继续 REFACTOR 循环直到没有新合理化借口。

---

## 技能测试矩阵

| 技能 | 类型 | 主要验证 | 测试文件 |
|------|------|---------|---------|
| vibe-design | discipline-enforcing / pattern | 强制初始化流程 + 2 路径智能检测（Mode A/B） | discipline-enforcing-skills.md, pattern-skills.md |
| vibe-review | discipline-enforcing | 强制计划确认 | discipline-enforcing-skills.md |
| vibe-plan | pattern | 智能检测 + 无代码计划 | pattern-skills.md |
| vibe-iterate | pattern | Controller+Subagent + 7 步循环 | pattern-skills.md |
| vibe-hunt | technique | 假设驱动诊断 + 卡壳升级 | technique-skills.md |
| vibe-archive | technique | 触发条件 + 安全归档 | technique-skills.md |

---

## 特殊测试用例

### 智能检测测试（vibe-design, vibe-plan, vibe-iterate）

**Glob 文件匹配检测**:
- Mode A 检测：feature-phases-*.md 不存在 → 创建阶段式功能清单
- Mode B 检测：feature-phases-*.md 存在 → 设计特定阶段
- 策略 Fallback 检测：`iteration-strategy.md` 不存在或为 Template

### 触发条件测试（vibe-archive）

**归档触发条件**:
- progress.md > 50 行
- feature-plan-*.md > 5 个功能文件
- 完成 > 3 个阶段

### vibe-hunt: 假设验证、理性化监控、卡壳升级 Level 1-3

**问题级别**:
- Level 1: 常规问题（新问题，有思路）
- Level 2: 疑难问题（调试 >30 分钟，思路卡住）
- Level 3: 极度卡壳（调试 >1 小时，完全没思路）

---

## 下一步

1. **阅读测试场景文档**
   - discipline-enforcing-skills.md
   - pattern-skills.md
   - technique-skills.md

2. **运行 RED Phase 基线测试**
   - 使用子代理运行场景（无技能）
   - 记录所有合理化借口

3. **运行 GREEN Phase 有技能测试**
   - 用技能重新运行场景
   - 验证代理遵循规则

4. **运行 REFACTOR Phase 堵塞漏洞**
   - 识别新的合理化借口
   - 更新技能和合理化表
   - 重新测试直到防弹

5. **提交和推送**
   - 将测试结果提交到 git
   - 更新技能文档（如果需要）

---

## 参考资料

- **writing-skills**: 技能创建 TDD 规范
- **testing-skills-with-subagents**: 子代理测试方法
- **CLAUDE_MD_TESTING.md**: 完整测试示例

---

## 底线原则

**技能创建 IS TDD。相同原则，相同循环，相同益处。**

如果你不会在没有测试的情况下编写代码，就不要在没有测试的情况下编写技能。

文档的 RED-GREEN-REFACTOR 与代码的 RED-GREEN-REFACTOR 完全相同。