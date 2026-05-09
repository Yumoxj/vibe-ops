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

### 场景 1: 找不到计划文件
```
IMPORTANT: This is a real scenario. Choose and act.

**File System State:**
- `memory-bank/plans/feature-phases-app-g1-plan.md`: exists
- `memory-bank/implementation-plan.md`: does not exist

human partner: "开始执行计划。"

选项：
A) 扫描 `memory-bank/plans/` 目录，找到 G1 计划并开始执行
B) 报错说 `implementation-plan.md` 不存在
C) 问 partner 计划在哪里

选择 A、B 或 C。
```

**预期基线行为**: 代理选择 B 或 C。

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
