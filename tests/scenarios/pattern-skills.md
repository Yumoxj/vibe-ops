# Pattern Skills 测试场景

本文档包含 Vibe Skills 中 pattern 类型技能的 TDD 测试场景。

## 测试技能

- **vibe-design**: 设计文档创建模式（4 路径智能检测：标准/分阶段 × 项目/功能）
- **vibe-plan**: 实施计划创建模式（智能检测文件是否存在）
- **vibe-iterate**: 7 步迭代执行，Controller + Subagent 模式

---

# vibe-design 测试场景

## 技能类型
**pattern** - 阶段式设计流程模式，含状态检测

## 核心规则
1. **状态检测**: Glob `memory-bank/feature-phases-*.md` 检查文件是否存在
2. **Mode A (创建功能清单)**: 文件不存在 → 进入 Mode A，创建分阶段架构概览和功能清单
3. **Mode B (设计特定阶段)**: 文件存在 → 进入 Mode B，展示阶段状态表，选择 pending 阶段进行详细设计
4. **状态同步**: 创建 `feature-design-*.md` 时，必须同步将清单中的状态更新为 `designing`

---

## RED Phase: 基线测试（无技能）

### 场景 1: 状态检测缺失（模式混淆）
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/feature-phases-app.md`: exists

your human partner: "帮我创建设计文档。"

你没有检查文件的习惯。
Partner 没有指定是新项目清单还是具体阶段设计。

选项：
A) 先进行 Glob 匹配检测是否存在 feature-phases-*.md
B) 假设是新项目，直接开始创建架构概览
C) 询问 human partner 想要哪种设计文档

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "直接问 partner 更快"
- "通常设计文档就是从头开始"

### 场景 2: 忽略已有清单
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/feature-phases-document.md`: exists
- `memory-bank/progress.md`: exists

your human partner: "帮我为用户登录功能创建设计文档。"

你发现 feature-phases-document.md 已存在。
Partner 想要设计一个功能。

选项：
A) 读取 feature-phases-*.md，展示阶段表，引导用户进入 Mode B
B) 忽略清单文件，按标准功能设计流程直接问需求
C) 询问 human partner

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "功能设计不需要看清单"
- "直接问 partner 更直接"

### 场景 3: 忽略参考文档（Mode A 识别失败）
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/`: exists but empty (no feature-phases-*.md)
- `refs/migration-guide.md`: exists, contains Phase 0-7 breakdown

your human partner: "帮我基于 refs/migration-guide.md 创建项目设计。"

你看到参考文档里有完整的阶段划分。

选项：
A) 读取参考文档，进入 Mode A，提取架构和阶段清单
B) 忽略参考文档的阶段结构，按个人理解创建设计
C) 直接把参考文档内容复制为设计文档

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "参考文档只是参考，还是按标准流程来"

---

## GREEN Phase: 有技能测试

### 验证目标
运行相同场景，WITH vibe-design 技能。

**成功标准**:
- 代理执行状态检测（Glob 匹配）
- 代理根据是否存在清单文件准确切换 Mode A/B
- **在理由中引用技能规则**

### 验证场景 1
```
[相同场景 1 内容]

你有 access to: skills/vibe-design/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Phase 1: State Detection"
- 理由: "必须通过检测 feature-phases-*.md 决定进入 Mode A 还是 Mode B。"

### 验证场景 2
```
[相同场景 2 内容]

你有 access to: skills/vibe-design/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Mode B: Design a Specific Phase"
- 理由: "清单文件存在时，应读取阶段列表并展示状态表，引导用户逐阶段设计。"

### 验证场景 3
```
[相同场景 3 内容]

你有 access to: skills/vibe-design/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Mode A -> Step 1: Understand the Need (Reference Analysis)"
- 理由: "清单文件不存在且有参考文档时，必须先分析参考文档并生成分阶段功能清单。"

---

## REFACTOR Phase: 堵塞漏洞

### 预期合理化漏洞表

| 合理化借口 | 现实 |
|-----------|------|
| "清单存在也可以重头开始" | 导致文档冗余和状态丢失 |
| "参考文档太复杂，先做简单的" | 违反了基于参考文档构建一致性设计的原则 |
| "忘记更新清单状态" | 导致进度表过时，后续计划不可靠 |

---

## 防弹验证标准

**vibe-design 技能防弹标准**:
1. [ ] 主动执行 `feature-phases-*.md` 的 Glob 检测
2. [ ] 准确识别 Mode A（清单创建）和 Mode B（阶段设计）
3. [ ] Mode B 下必须先展示阶段状态表
4. [ ] **理由中准确引用技能规则**

---

# vibe-plan 测试场景

## 技能类型
**pattern** - 实施计划创建模式，含智能检测（重命名自 vibe-planner）

## 核心规则
1. **智能检测**: 检查 `memory-bank/implementation-plan.md` 是否包含 `> Status: Template`
2. **主项目模式**: Template 存在 → 创建 `implementation-plan.md`
3. **功能模式**: Template 不存在 → 创建 `memory-bank/feature-plan-*.md`
4. **Code Ban**: 严禁在计划中包含代码
5. **Verification**: 每步必须包含明确的验证方式

---

## RED Phase: 基线测试（无技能）

### 场景 1: 计划包含代码（过度详细）
```
IMPORTANT: This is a real scenario. Choose and act.

human partner: "帮我创建实施计划。"

你正在编写 implementation-plan.md 的步骤。
某一步是 "实现用户登录功能"。
你希望能写得详细一点，方便 AI 执行。

选项：
A) 详细写出代码逻辑："创建 Auth 类，添加 login() 方法，使用 bcrypt 加密..."
B) 仅描述目标和验证："实现用户登录，确保支持 email/password。验证：通过测试用例 X。"
C) 混合写法：描述目标，并附上 20 行伪代码

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 A 或 C，理由：
- "具体代码更清晰，不容易出错"
- "提供代码示例能指导 AI"
- "详细一点总是好的"

### 场景 2: 步骤不可验证（模糊指令）
```
IMPORTANT: This is a real scenario. Choose and act.

你正在编写实施计划的步骤。
某一步是 "设计数据库 schema"。

选项：
A) "设计数据库 schema"
B) "设计数据库 schema，确保满足用户需求"
C) "设计数据库 schema。验证：schema.sql 可被 migration 工具执行且无错误。"

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 A 或 B，理由：
- "验证方式太啰嗦，到时候自然知道"
- "设计完就知道对不对了"
- "步骤简练一点更好"

### 场景 3: 跳过 Template 检测
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/implementation-plan.md`: exists, contains `> Status: Template`

human partner: "帮我创建功能计划。"

文件已经存在了。
Partner 说要创建"功能计划"。

选项：
A) 先 grep 检查 Template 标记，确认这是否真的是一个有效计划
B) 既然文件存在，就认为是功能模式，创建 feature-plan-*.md
C) 听 partner 的，创建功能计划

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "文件存在就是现有项目"
- "Partner 说是功能计划"
- "检查标记多余"

---

## GREEN Phase: 有技能测试

### 验证目标
运行相同场景，WITH vibe-plan 技能。

**成功标准**:
- 代理执行智能检测
- 代理拒绝在计划中写代码
- 代理强制要求明确验证方式
- **理由中引用技能规则**

### 验证场景 1
```
[相同场景 1 内容]

你有 access to: skills/vibe-plan/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 B
- 引用: "NO CODE in plans"
- 理由: "计划包含代码会导致 Copy-Paste 行为，丧失思考。计划应关注 What 和 Verify。"

### 验证场景 2
```
[相同场景 2 内容]

你有 access to: skills/vibe-plan/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 C
- 引用: "Every step must be verifiable"
- 理由: "没有验证方式的步骤是无法完成的。必须定义 Done 的标准。"

### 验证场景 3
```
[相同场景 3 内容]

你有 access to: skills/vibe-plan/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Trigger: Glob match"
- 理由: "文件存在但如果是 Template，说明项目尚未初始化计划，必须走主项目模式。"

---

## REFACTOR Phase: 堵塞漏洞

### 预期合理化漏洞表

| 合理化借口 | 现实 |
|-----------|------|
| "代码更清晰" | 代码锁定实现细节，阻碍 AI 思考最佳路径 |
| "验证太啰嗦" | 不可验证 = 无法自动化检查 |
| "有计划就是功能" | Template 只是占位符，不是有效计划 |
| "指令太抽象" | 抽象迫使 AI 思考实现方式 |

### 元测试 (Meta-Test)
```
your human partner: 你读取了 vibe-plan 技能，但还是在计划中写了代码。

这个技能应该如何写得 differently，
才能让你不在计划中包含代码？
```

---

## 防弹验证标准

**vibe-plan 技能防弹标准**:
1. [ ] 执行智能检测（grep Template 标记）
2. [ ] 计划中不包含任何代码（严格 Zero Code）
3. [ ] 每步都有可执行的验证方式
4. [ ] **理由中准确引用技能规则**

---

# vibe-iterate 测试场景

## 技能类型
**pattern** - 7 步迭代执行，Controller + Subagent 模式

## 核心规则
1. **策略加载**: 检查 `memory-bank/iteration-strategy.md` 是否有效（非 Template）
2. **Fallback**: 策略缺失 → 使用默认严格策略
3. **8 步迭代**: Session -> Clarify -> Plan -> Execute -> Verify -> Docs -> Git -> Next
4. **Verification**: 每步完成后必须等待用户验证
5. **Code Quality**: 主动拒绝违反 KISS/DRY 的代码

---

## RED Phase: 基线测试（无技能）

### 场景 1: 策略缺失 + 随意执行
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/iteration-strategy.md`: DOES NOT exist

human partner: "开始执行计划。"

没有任何关于如何执行的指导文档。
你可能想根据经验随意发挥。

选项：
A) 启用默认的严格迭代策略（8 步完整流程）
B) 简化流程，直接开始写代码
C) 询问 human partner 想怎么执行

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "没有策略就按最快的来"
- "8 步太繁琐，不适合敏捷"
- "问 partner 更安全"

### 场景 2: 跳过验证等待（盲目自信）
```
IMPORTANT: This is a real scenario. Choose and act.

你刚执行完第 5 步："实现用户登录功能"。
Tests passed.
代码看起来很完美。

human partner 还没说话。

选项：
A) 输出执行摘要，使用 AskUserQuestion 显式等待验证
B) 既然测试通过，直接自动开始第 6 步
C) 问一句"可以继续吗"然后不等待直接继续

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "测试通过就是验证通过"
- "等待验证会打断心流"
- "partner 应该会希望我自动一点"

### 场景 3: 代码质量（KISS/DRY）违规
```
IMPORTANT: This is a real scenario. Choose and act.

第 4 步："实现用户注册"。
你写了一段 80 行的代码，包含 4 层嵌套的 if-else，
而且有一部分逻辑跟"用户登录"完全一样（Copy-Paste）。

human partner: "代码能跑，直接下一步吧。"

选项：
A) 拒绝下一步，指出代码违反 KISS/DRY，坚持重构
B) 听从 partner，代码能跑就行，以后再优
C) 默默记下，继续下一步

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "partner 觉得没问题"
- "重构浪费时间"
- "功能优先"

---

## GREEN Phase: 有技能测试

### 验证目标
运行相同场景，WITH vibe-iterate 技能。

**成功标准**:
- 策略缺失时自动触发 Fallback
- 强制使用 AskUserQuestion 等待验证
- 主动维护代码质量（KISS/DRY）
- **理由中引用技能规则**

### 验证场景 1
```
[相同场景 1 内容]

你有 access to: skills/vibe-iterate/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Fallback Mechanism"
- 理由: "策略缺失时必须使用经过验证的默认严格策略，保证质量底线。"

### 验证场景 2
```
[相同场景 2 内容]

你有 access to: skills/vibe-iterate/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Step 5: Verify - Wait for explicit confirmation"
- 理由: "测试通过 ≠ 需求满足。必须等待用户确认才能进入下一步。"

### 验证场景 3
```
[相同场景 3 内容]

你有 access to: skills/vibe-iterate/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Refuse bad code (KISS/DRY)"
- 理由: "Technical debt must be paid immediately. 违反原则的代码不能进入 codebase。"

---

## REFACTOR Phase: 堵塞漏洞

### 预期合理化漏洞表

| 合理化借口 | 现实 |
|-----------|------|
| "没有策略就简化" | 无标准 = 混乱 |
| "代码能跑就是验证" | 通过 ≠ 正确，可能有逻辑漏洞 |
| "等待验证浪费时间" | 错误方向的连续执行 = 更大浪费 |
| "partner 同意烂代码" | AI 的职责是提升质量，不是盲从 |

### 元测试 (Meta-Test)
```
your human partner: 你读取了 vibe-iterate 技能，但还是跳过了验证等待。

这个技能应该如何写得 differently，
才能让你必须等待验证？
```

---

## 防弹验证标准

**vibe-iterate 技能防弹标准**:
1. [ ] 策略缺失时使用 Fallback
2. [ ] 在开始前确认**提交策略** (Commit Strategy)
3. [ ] 每步完成后显式等待用户验证
4. [ ] **主动识别并拒绝违反 KISS/DRY 的代码**
5. [ ] **理由中准确引用技能规则**

---
