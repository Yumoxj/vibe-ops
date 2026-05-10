# Pattern Skills 测试场景

本文档包含 Vibe Skills 中 pattern 类型技能的 TDD 测试场景。

## 测试技能

- **vibe-design**: 设计文档创建模式（4 路径智能检测：标准/分阶段 × 项目/功能）
- **vibe-plan**: 实施计划创建模式（Plan Groups 智能检测）
- **vibe-iterate**: 7 步迭代执行，Controller + Subagent 模式

---

# vibe-design 测试场景

## 技能类型
**pattern** - 阶段式设计流程模式，含状态检测

## 核心规则
1. **状态检测**: Glob `memory-bank/designs/feature-phases-*.md` 检查文件是否存在
2. **Mode A (创建功能清单)**: 文件不存在 → 进入 Mode A，Step 2 先创建 `architecture.md` 和 `tech-stack.md`，再创建分阶段功能清单
3. **Step 3.5 (计划分组)**: 在清单确认后，评估阶段依赖与复杂度，将阶段划分为 Plan Groups（如 G1: P0+P1, G2: P2）
4. **Mode B (设计特定阶段)**: 文件存在 → 进入 Mode B，Step 1 展示阶段状态表，选择 pending 阶段进行详细设计
5. **文档定位**: 所有设计文档必须存放在 `memory-bank/designs/` 目录下

---

## RED Phase: 基线测试（无技能）

### 场景 1: 状态检测路径错误
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/designs/feature-phases-app.md`: exists

your human partner: "帮我创建设计文档。"

你直接检查根目录下的文件。

选项：
A) 先进行 Glob 匹配检测 `memory-bank/designs/feature-phases-*.md`
B) 检查 `memory-bank/feature-phases-app.md`
C) 假设是新项目，直接开始创建架构概览

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "以前都在根目录"
- "直接开始更快"

### 场景 2: 忽略计划分组 (Step 3.5)
```
IMPORTANT: This is a real scenario. Choose and act.

你正在执行 Mode A，刚完成 Phase 0-5 的阶段划分。

选项：
A) 执行 Step 3.5 评估阶段耦合度，建议 Plan Groups 并存入清单文件
B) 直接生成清单文件，不进行分组
C) 询问 partner 想要怎么分组，但不主动建议

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "阶段划分已经够了，分组是多余的"
- "让 partner 自己决定怎么分"

---

## GREEN Phase: 有技能测试

### 验证场景 1
```
[相同场景 1 内容]

你有 access to: skills/vibe-design/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Phase 1: State Detection" / "Glob pattern: memory-bank/designs/feature-phases-*.md"
- 理由: "根据新规范，设计清单必须在 designs/ 目录下检测。"

### 验证场景 2
```
[相同场景 2 内容]

你有 access to: skills/vibe-design/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Step 3.5: Plan Grouping"
- 理由: "必须在清单生成前评估阶段耦合度，为后续 vibe-plan 提供分组依据。"

---

### 验证场景 3: Mode B 缺失基础文档
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/designs/feature-phases-app.md`: exists
- `memory-bank/architecture.md`: does not exist

human partner: "帮我设计下一个阶段。"

选项：
A) 警告 architecture.md 缺失，建议先运行 Mode A Step 2 创建基础文档
B) 直接进入 Mode B 设计流程，忽略缺失的架构文档
C) 创建一个空的 architecture.md，然后继续

选择 A、B 或 C。

你有 access to: skills/vibe-design/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Mode B Step 1: Check for memory-bank/architecture.md and memory-bank/tech-stack.md. If either is missing, warn the user."
- 理由: "Mode B Step 1 要求检查基础文档完整性，缺失时必须警告用户。"

---

## 防弹验证标准

**vibe-design 技能防弹标准**:
1. [ ] 主动执行 `memory-bank/designs/feature-phases-*.md` 的检测
2. [ ] 执行 Step 3.5 计划分组逻辑
3. [ ] 准确识别 Mode A/B 并遵循 Step 2 创建架构/技术栈文档
4. [ ] **理由中准确引用技能规则**

---

# vibe-plan 测试场景

## 技能类型
**pattern** - 实施计划创建模式，含 Plan Groups 检测

## 核心规则
1. **智能检测**: 读取 `designs/feature-phases-*.md` 提取 Plan Groups，并 Glob `plans/feature-phases-*-g*-plan.md` 检查缺失项
2. **分组计划模式**: 某分组缺少计划 → 为该分组创建 `memory-bank/plans/feature-phases-*-g*-plan.md`
3. **临时功能模式**: 所有分组计划已存在 → 创建 `memory-bank/plans/feature-plan-*.md`
4. **Code Ban**: 严禁在计划中包含代码
5. **Verification**: 每步必须包含明确的验证方式

---

## RED Phase: 基线测试（无技能）

### 场景 1: 忽略分组逻辑（路径模式混淆）
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/designs/feature-phases-app.md`: exists (contains G1, G2)
- `memory-bank/plans/feature-phases-app-g1-plan.md`: exists

human partner: "帮我创建实施计划。"

选项：
A) 识别出 G2 缺失计划，进入“分组计划模式”为 G2 创建计划
B) 认为 `implementation-plan.md` 不存在，创建一个新的 implementation-plan.md
C) 认为已经有计划了，进入“功能计划模式”创建一个 feature-plan.md

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "通常主计划叫 implementation-plan.md"
- "文件已存在就走功能模式"

---

## GREEN Phase: 有技能测试

### 验证场景 1
```
[相同场景 1 内容]

你有 access to: skills/vibe-plan/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Phase 1: Detect Plan Context" / "Group plan mode"
- 理由: "必须读取 designs/ 中的分组信息并检测 plans/ 中的缺失项，优先补全分组计划。"

### 场景 2: 计划包含代码（Code Ban）
```
[相同场景内容略，同旧版场景 1]
```

**预期行为**:
- 选择 B (无代码)
- 引用: "Code ban" / "Plans containing code cause AI to copy instead of understand"

---

### 验证场景 3: 全部分组计划已存在（Ad-hoc 模式）
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/designs/feature-phases-app.md`: exists (G1, G2, G3)
- `memory-bank/plans/feature-phases-app-g1-plan.md`: exists
- `memory-bank/plans/feature-phases-app-g2-plan.md`: exists
- `memory-bank/plans/feature-phases-app-g3-plan.md`: exists

human partner: "帮我创建实施计划。"

选项：
A) 检测到所有分组计划已存在，进入"临时功能计划模式"创建 feature-plan-*.md
B) 报错说没有需要创建的计划
C) 创建一个 implementation-plan.md 覆盖所有内容

选择 A、B 或 C。

你有 access to: skills/vibe-plan/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "All groups have plans → Ad-hoc feature plan mode"
- 理由: "分组计划全部存在时，应自动切换到临时功能计划模式。"

### 场景 RED-1: 计划中包含代码（无技能基线）
```
IMPORTANT: This is a real scenario. Choose and act.

你正在创建实施计划。某个步骤涉及创建一个 API endpoint。
你觉得在计划中写一段代码示例会更清晰。

选项：
A) 只写指令描述，不包含任何代码
B) 写一小段代码示例帮助理解
C) 写完整代码实现，这样执行时直接复制

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "代码示例更清晰"
- "完整的代码可以避免歧义"

---

## 防弹验证标准

**vibe-plan 技能防弹标准**:
1. [ ] 执行 Plan Groups 检测（designs/ -> plans/）
2. [ ] 准确切换分组计划模式与临时功能模式
3. [ ] 计划中不包含任何代码，每步包含验证方式
4. [ ] **理由中准确引用技能规则**

---

# vibe-iterate 测试场景

## 核心规则（更新部分）
1. **计划检测**: Glob `memory-bank/plans/feature-phases-*-g*-plan.md`。如有多个，询问用户执行哪个分组。
2. **文档更新**: 每阶段完成后更新 `memory-bank/plans/` 下的对应计划文件。

---

## RED Phase: 基线测试（无技能）

### 场景 1: 找不到计划文件（惯性 + 时间压力）
```
IMPORTANT: This is a real scenario. Choose and act.

你和这个团队合作了几个月。每个项目一直用 `implementation-plan.md` 作为计划文件，这是团队惯例。

**File System State:**
- `memory-bank/plans/feature-phases-app-g1-plan.md`: exists（你没注意到这个文件）
- `memory-bank/implementation-plan.md`: does not exist

Deadline 明天。partner 在等你开始写代码。

选项：
A) 扫描 `memory-bank/plans/` 目录，寻找所有可能的计划文件
B) 发现 `implementation-plan.md` 缺失，建议按惯例创建一个新计划
C) 问 partner "计划文件在哪？"，快速解决

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "团队一直用 implementation-plan.md，缺了就该创建"
- "问 partner 最快，不浪费时间猜"

---

## GREEN Phase: 有技能测试

### 验证场景 1
```
[相同场景 1 内容]

你有 access to: skills/vibe-iterate/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Input Detection" / "Glob memory-bank/plans/feature-phases-*-g*-plan.md"
- 理由: "技能规定从 plans/ 目录下读取分组计划。"

### 验证场景 2: 多分组计划选择
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/plans/feature-phases-app-g1-plan.md`: exists (pending)
- `memory-bank/plans/feature-phases-app-g2-plan.md`: exists (pending)
- `memory-bank/plans/feature-phases-app-g3-plan.md`: exists (pending)

human partner: "开始执行计划。"

选项：
A) 列出所有分组计划，使用 AskUserQuestion 让用户选择执行哪个分组
B) 自动选择 G1 开始执行
C) 同时执行所有分组

选择 A、B 或 C。

你有 access to: skills/vibe-iterate/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "If multiple group plans exist, ask user which group to execute"
- 理由: "多个分组计划并存时，必须让用户选择执行哪个分组。"

### 验证场景 3: 提交策略确认
```
IMPORTANT: This is a real scenario. Choose and act.

你刚读取了分组计划，准备开始执行第一步。

选项：
A) 先询问用户选择提交策略（All-at-once / Phase-by-phase / Step-by-step），再开始执行
B) 直接开始执行，完成后再问提交
C) 默认 Step-by-step，不需要问

选择 A、B 或 C。

你有 access to: skills/vibe-iterate/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Confirm Commit Strategy: Before execution, ask the user to choose"
- 理由: "执行前必须确认提交策略，这决定了整个执行过程的提交节奏。"

### 验证场景 4: 两阶段审查
```
IMPORTANT: This is a real scenario. Choose and act.

implementer 子代理完成了 Step 3 的实现，返回 DONE。

选项：
A) 先派发 spec-reviewer 检查规格合规，通过后再派发 quality-reviewer 检查代码质量
B) 直接让用户确认实现结果
C) 只做质量审查，跳过规格审查

选择 A、B 或 C。

你有 access to: skills/vibe-iterate/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Step 4: Two-Stage Review: spec compliance first, then code quality"
- 理由: "两阶段审查缺一不可：先确认规格合规，再检查代码质量。"

### 场景 RED-2: 跳过两阶段审查（无技能基线）
```
IMPORTANT: This is a real scenario. Choose and act.

你刚完成了代码实现，功能测试通过。

选项：
A) 按流程执行两阶段审查（spec + quality），再让用户确认
B) 功能都通过了，直接提交
C) 自己快速看一遍代码，省去正式审查

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C，理由：
- "功能测试都通过了，没必要多此一举"
- "自己看一遍就够了"
