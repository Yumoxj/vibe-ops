# Vibe Skills 全面测试计划 (TDD Edition)

> **版本**: 2.0
> **日期**: 2026-05-12
> **目标**: 验证 Vibe Skills 系统的健壮性、抗压能力和模式识别准确性。反映 memory-bank 重构后的结构。

本文档定义了 Vibe Skills 的完整测试执行计划。测试遵循 **Red-Green-Refactor** 的 TDD 流程，确保每个技能不仅能“工作”，而且能在面临真实世界压力（如时间紧迫、权威施压、惰性）时“防弹”。

---

## 1. TDD 测试方法论

在 LLM Agent 技能开发的语境下，TDD 循环定义如下：

1.  🔴 **RED (Baseline / 基线测试)**
    *   **动作**: 在 **不加载/不提供** 目标技能文档的情况下，运行测试场景。
    *   **目的**: 确认在没有技能约束时，Agent 会因为压力、诱惑或缺乏上下文而做出错误选择（通常是选择“捷径”）。这证明了技能存在的必要性。
    *   **预期**: Agent 失败（选择 B 或 C），并给出合理的借口（Rationalization）。

2.  🟢 **GREEN (Verification / 验证测试)**
    *   **动作**: 在 **加载/提供** 目标技能文档的情况下，运行相同的测试场景。
    *   **目的**: 验证技能文档是否提供了足够的上下文、规则和原则，使 Agent 能够克服压力，做出正确选择。
    *   **预期**: Agent 成功（选择 A），并引用技能中的具体规则作为理由。

3.  🔵 **REFACTOR (Optimization / 重构优化)**
    *   **动作**: 如果 GREEN 阶段失败（即有技能仍选错），分析 Agent 的“新借口”。
    *   **执行**: 修改技能文档（`SKILL.md`），明确反驳这个新借口，或增强规则的权重。
    *   **循环**: 重新运行 GREEN 测试，直到通过。

---

## 2. 测试环境准备

### 2.1 文件准备
- 确保 `tests/scenarios/` 下的所有测试场景文件是最新的。
- 创建 `tests/execution_logs/` 目录（如果不存在），用于存放测试日志。

### 2.2 模拟环境
为了有效测试 **Pattern Skills**（如 vibe-design 的智能检测），需要构建模拟的文件系统状态。

*   **Mock State A (New Project)**:
    *   `memory-bank/designs/feature-design-app.md` (不存在)
    *   `memory-bank/architecture.md` (不存在)
    *   `memory-bank/tech-stack.md` (不存在)
    *   `memory-bank/plans/` (空目录)
    *   `memory-bank/progress.md` (不存在)
*   **Mock State B (Existing Project)**:
    *   `memory-bank/designs/feature-design-app.md` (存在)
    *   `memory-bank/architecture.md` (存在)
    *   `memory-bank/tech-stack.md` (存在)
    *   `memory-bank/plans/feature-design-app-g1-plan.md` (存在)
    *   `memory-bank/progress.md` (存在)

---

## 3. 详细执行计划

### 第一阶段：Discipline-Enforcing Skills (纪律强制类)
**核心目标**: 验证 Agent 在高压下的定力。

#### 3.1 vibe-design 测试
*   **场景源**: `tests/scenarios/discipline-enforcing-skills.md` -> "vibe-init 测试场景（已合并至 vibe-design）"
*   **测试步骤**:
    1.  🔴 **RED**: 运行 "场景 1: 跳过初始化 + 时间压力"。提示 Agent 忽略 `skills/vibe-design`。
        *   *预期结果*: 选 B/C ("客户着急，先写代码")。
    2.  🟢 **GREEN**: 明确加载 `skills/vibe-design/SKILL.md`。重运行场景 1。
        *   *预期结果*: 选 A ("必须初始化")。检查是否引用了 "初始化必须在编码之前"。
    3.  🟢 **GREEN**: 运行 "场景 2 (Sunk Cost)" 和 "场景 3 (Authority)"。
    4.  🔵 **REFACTOR**: 如有失败，更新 SKILL.md 中的 "When to Use" 或 "核心原则"。

#### 3.2 vibe-review 测试
*   **场景源**: `tests/scenarios/discipline-enforcing-skills.md` -> "vibe-review 测试场景（重命名自 vibe-reviewer）"
*   **测试步骤**:
    1.  🔴 **RED**: 运行 "场景 1: 跳过确认 + 时间压力"。
    2.  🟢 **GREEN**: 加载 `skills/vibe-review/SKILL.md`。重运行场景 1。
        *   *关键验证*: Agent 是否正确执行范围评估、深度分级和专家派发？
    3.  🟢 **GREEN**: 运行 "场景 2 (过度自信)"。
        *   *关键验证*: Agent 是否正确进行了范围评估并判断了深度分级？

---

### 第二阶段：Pattern Skills (模式识别类)
**核心目标**: 验证智能检测逻辑 (Smart Detection) 的准确性。

#### 3.3 vibe-design 测试
*   **场景源**: `tests/scenarios/pattern-skills.md` -> "vibe-design 测试场景"
*   **测试步骤**:
    1.  🔴 **RED**: 运行 "场景 1: 跳过检测"。
    2.  🟢 **GREEN (Logic Check 1)**:
        *   *Setup*: 确保 `memory-bank/designs/` 中没有 `feature-design-*.md`。
        *   *Prompt*: "运行 vibe-design"。
        *   *Verify*: Agent 识别为 **Mode A: 创建功能设计文档**。
    3.  🟢 **GREEN (Logic Check 2)**:
        *   *Setup*: 创建 `memory-bank/designs/feature-design-app.md`。
        *   *Prompt*: "运行 vibe-design"。
        *   *Verify*: Agent 识别为 **Mode B: 设计特定阶段**。
    4.  🟢 **GREEN (Architecture Check)**:
        *   *Prompt*: Mode A 场景，用户要求跳过架构文档。
        *   *Verify*: Agent 坚持 Mode A Step 2 流程，先创建 `architecture.md` 和 `tech-stack.md`。
    5.  🟢 **GREEN (Mode B Check)**:
        *   *Setup*: `memory-bank/designs/feature-design-*.md` 存在，但 `architecture.md` 不存在。
        *   *Prompt*: "运行 vibe-design"。
        *   *Verify*: Agent 进入 Mode B 时警告 `architecture.md` 缺失。

#### 3.4 vibe-plan 测试
*   **场景源**: `tests/scenarios/pattern-skills.md` -> "vibe-plan 测试场景（重命名自 vibe-planner）"
*   **测试步骤**:
    1.  🔴 **RED**: 运行 "场景 1: 计划包含代码"。
    2.  🟢 **GREEN**: 加载技能。
        *   *Verify*: 生成的计划是否 **不包含代码**，只有指令？
    3.  🟢 **GREEN**: 运行 "场景 2: 步骤不可验证"。
        *   *Verify*: 每步是否包含明确的验证方式？
    4.  🟢 **GREEN (Architecture Context)**:
        *   *Setup*: `memory-bank/architecture.md` 和 `memory-bank/tech-stack.md` 存在。
        *   *Prompt*: "为 Phase 2 创建实施计划"。
        *   *Verify*: Agent 在创建计划前读取 `architecture.md` 和 `tech-stack.md` 作为上下文。

#### 3.5 vibe-iterate 测试
*   **场景源**: `tests/scenarios/pattern-skills.md` -> "vibe-iterate 测试场景"
*   **测试步骤**:
    1.  🟢 **GREEN (Commit Strategy)**:
        *   *Setup*: 设定一个具体的任务上下文。
        *   *Prompt*: "开始执行计划"。
        *   *Verify*: Agent 在执行前确认提交策略（统一/阶段/步步）。
    2.  🟢 **GREEN (Subagent Dispatch)**:
        *   *Verify*: Agent 能够正确进行 Controller 决策并派发给合适的 Subagent。
    3.  🟢 **GREEN (7-Step Loop)**:
        *   *Verify*: 完成任务后，是否正确执行两阶段审查（spec + quality）并等待用户确认？
    4.  🟢 **GREEN (Architecture Consistency)**:
        *   *Setup*: 完成一个阶段，且变更涉及架构变化。
        *   *Verify*: Agent 对比变更与 architecture.md/tech-stack.md，建议更新受影响部分。

---

### 第三阶段：Technique Skills (技术执行类)
**核心目标**: 验证最佳实践的执行质量。

#### 3.6 vibe-hunt 测试
*   **测试重点**: 诊断驱动调试场景（假设验证、理性化监控、卡壳升级）。
*   **步骤**:
    *   构造 "Level 3: 极度卡壳" 场景 (3 次假设失败，1 小时以上无进展)。
    *   验证 Agent 是否向用户汇报并建议回滚，而不是继续盲目尝试。

#### 3.7 vibe-archive 测试
*   **测试重点**: 触发条件的数学判断 + never-archive 保护。
*   **步骤**:
    *   提供数据: `progress.md` 60 行。
    *   验证: Agent 建议归档。
    *   提供数据: `progress.md` 20 行。
    *   验证: Agent 建议 **不** 归档。
    *   提供: 用户要求归档 architecture.md/tech-stack.md。
    *   验证: Agent 拒绝归档 foundation 文档。

#### 3.8 vibe-clean 测试
*   **测试重点**: 深度分级 + 子代理派发 + 输出格式。
*   **步骤**:
    *   🟢 **GREEN (Surface)**:
        *   *Setup*: 目标代码 < 50 行。
        *   *Verify*: 仅派发 simplification-analyzer。
    *   🟢 **GREEN (Standard)**:
        *   *Setup*: 目标代码 50–200 行。
        *   *Verify*: 派发 simplification-analyzer + dead-code-analyzer。
    *   🟢 **GREEN (Deep)**:
        *   *Setup*: 目标代码 200+ 行。
        *   *Verify*: 派发全部三个 analyzer（simplification + dead-code + duplicate）。
    *   🟢 **GREEN (Output)**:
        *   *Verify*: 输出 `feature-design-clean-*.md` 到 `memory-bank/designs/`。
    *   🟢 **GREEN (Integration)**:
        *   *Verify*: 输出文档可被 `/vibe-plan` 直接消费。

---

## 4. 执行日志模板

建议为每轮测试建立一个 Markdown 日志文件 (例如 `tests/execution_logs/run-2026-01-22.md`)。

```markdown
# 测试执行日志 - YYYY-MM-DD

## 1. vibe-design
| 阶段 | 场景 | 结果 (Pass/Fail) | 实际行为/备注 |
|---|---|---|---|
| RED | 场景 1 | Pass | 选择了 B，理由是客户着急 (符合预期) |
| GREEN | 场景 1 | Pass | 选择了 A，引用了"结构完整性"规则 |
| GREEN | 场景 2 | Fail | 选择了 B，Agent 认为 2 小时工作量太大不舍得删 -> **需 Refactor** |
| GREEN | 场景 4 (Architecture) | Pass | 选择了 A，引用了"Mode A Step 2"规则 |

## Refactor 记录
- **vibe-design**: 添加了针对 "Sunk Cost" 的明确反驳："错误的开端会导致更大的浪费"。
```

---

## 5. 验收标准

测试计划完成的标志是：

1.  **所有 Discipline Skills** 在 GREEN 阶段均选择 Option A。
2.  **所有 Pattern Skills** 准确识别了 100% 的 Mock 文件状态。
3.  **所有 Technique Skills** 遵循了规定的交互流程（如 Brainstorming, AskUserQuestion）。
4.  **vibe-clean** 正确按深度分级派发子代理并输出到 `memory-bank/designs/`。
5.  **Skills System Review** 文档已更新，反映最新的测试通过率。
