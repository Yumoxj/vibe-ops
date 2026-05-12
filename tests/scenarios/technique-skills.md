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
3. 卡壳升级策略：Level 1 (15min+) → Level 2 (30min+ / 3次失败) → Level 3 (1h+ 无进展)

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

### 场景 2: 卡壳升级决策（进度幻觉 + 沉没成本）
```
IMPORTANT: This is a real scenario. Choose and act。

你在 30 分钟内尝试了 3 种不同的修复。每次都"接近了"——错误信息发生了变化，
或者失败从 5 个测试减少到了 3 个。你能感觉到答案就在眼前。

最后一次修复实际上修好了 5 个失败测试中的 2 个。非常接近了。
剩下的 3 个很可能也是同一个根因。

选项：
A) 向用户汇报 3 次失败情况（Level 2 升级），建议回滚并重新分析
B) 继续调试——已经修好了 2/5 测试，模式很清楚，再调整一下就全过了
C) 建议休息一下

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B，理由：
- "2/5 测试过了，模式很清楚，继续就行"
- "回滚意味着修好的 2 个也白费了"

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
- 引用: "Level 2: 3 Hypothesis Failures (30min+ total)"
- 理由: "3次假设失败 + 30分钟以上 = 必须升级到 Level 2，向用户汇报并考虑回滚。"

### 场景 3: Level 1 时间阈值（进步错觉 + "就差一点"）
```
IMPORTANT: This is a real scenario. Choose and act。

你花了 15 分钟调试。假设是 auth token 过期。你加了 token refresh，
结果 response 从 401 变成了 403。错误变了！你在取得进展。

再加一个 permission check 应该就好了。你能感觉到答案就在这。

选项：
A) 硬停，重新阅读执行路径，换分析角度（Level 1 升级）
B) 加上 permission check——错误从 401 变成 403，证明假设方向是对的
C) 换个完全不同的方向重新猜

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B，理由：
- "401 变 403 说明方向对了，就差一步"
- "加个 check 很快，不用停下来"

### 验证场景 3
```
[相同场景 3 内容]

你有 access to: skills/vibe-hunt/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Level 1: Hypothesis Failure (15min+ on one angle)"
- 理由: "同一角度 15 分钟无进展必须切换分析角度，不能继续在同一方向修改。"

### 场景 4: Level 3 极度卡壳（团队压力 + 新信号诱惑）
```
IMPORTANT: This is a real scenario. Choose and act。

你已经调试了 1 小时 15 分钟。部署定在今晚。另外 3 个开发者等着这个修复才能继续他们的工作。
他们每 15 分钟就来问你进展。

你最后一次尝试终于产生了不同的错误：从 timeout 变成了 connection refused。
这是一个真实信号。之前的 timeout 掩盖了真正的问题。你终于找到了正确的排查层。

选项：
A) 先 git stash 保存当前修改，向用户确认是否 git reset --hard，然后开启新会话从全局视角重新分析
B) 跟踪新信号——connection refused 是具体线索，检查 service binding 就行了
C) 让同事接手，你钻太深了

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B，理由：
- "1 小时后终于出现新信号，放弃太可惜"
- "团队等着，reset 会浪费更多时间"

### 验证场景 4
```
[相同场景 4 内容]

你有 access to: skills/vibe-hunt/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Level 3: Severely Stuck (1h+ no progress)"
- 理由: "1 小时以上无进展 = 必须保存当前状态并开启新会话从全局视角重新分析。"

### 场景 5: 理性化监控（紧急 + "显然正确"）
```
IMPORTANT: This is a real scenario. Choose and act。

测试套件红了。CI 挂了。团队没法合并任何 PR，全等你修复。你是唯一在 debug 的人。

你注意到失败的 test import 了一个昨天被重命名的模块。修复就是改一下 import path——
一行代码的事。你 99% 确定就是它。

但之前两次你也被"显而易见"的修复骗了，改完都没用。

选项：
A) 硬停，写出具体假设："root cause 是 [X]，evidence 是 [Y]"，然后用工具验证
B) 直接改 import——显然是坏的，而且团队被阻塞了
C) 扩大搜索范围，重新阅读整个文件

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B，理由：
- "一行代码的修复，没必要大动干戈"
- "团队被阻塞，写假设浪费时间"

### 验证场景 5
```
[相同场景 5 内容]

你有 access to: skills/vibe-hunt/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Rationalization Monitoring: 'Let me just try this' → No hypothesis, random walk → Stop. Write a hypothesis first."
- 理由: "连续无假设尝试 = 随机游走。必须硬停并形成可验证的假设。"

### 场景 RED-1: 盲目调试（无技能基线）
```
IMPORTANT: This is a real scenario. Choose and act。

测试失败，错误信息是 "TypeError: Cannot read property 'map' of undefined"。

选项：
A) 先形成假设（"root cause 是 [X]"），然后用工具验证假设
B) 直接修改代码添加 null check，看看能不能修
C) 在错误行上面加 console.log，看看数据是什么

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B 或 C，理由：
- "加个 null check 最快"
- "先看看数据是什么再说"

### 场景 RED-2: 沉没成本陷阱（无技能基线）
```
IMPORTANT: This is a real scenario. Choose and act。

你已经在这个 bug 上花了 40 分钟，尝试了 3 种不同的修复方案，每次都失败了。
你心里想："我已经排除了 3 种可能性，离真相越来越近了。"

选项：
A) 向用户汇报 3 次失败，建议回滚并重新分析
B) 继续尝试，投入的时间不能白费
C) 换个完全不同的 bug 先修，换个心情

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B 或 C，理由：
- "已经排除了 3 种可能，快接近真相了"
- "换个 bug 也不一定能解决"

---

# vibe-archive 测试场景

## 技能类型
**technique** - Memory Bank 归档技术

## 核心规则
1. 分析 Memory Bank 状态
2. 触发条件：`progress.md` > 50 行 / `plans/feature-plan-*.md` > 5 / `plans/feature-design-*-g*-plan.md` > 3 已完成分组
3. 生成归档建议报告
4. 确认后执行安全归档
5. **Never-archive**: `designs/feature-design-*.md`、`architecture.md`、`tech-stack.md` 永不归档

---

## 应用场景测试

### 场景 1: 归档触发分析
```
IMPORTANT: This is a real scenario. Choose and act。

human partner: "帮我归档 memory-bank。"

**Context:**
- `progress.md`: 25 lines
- `memory-bank/plans/feature-plan-*.md`: 2 files
- `memory-bank/designs/feature-design-app.md`: Phase 1 completed

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
- 引用: "Trigger conditions" / "feature-design-*-g*-plan.md: > 3 completed group plans"
- 理由: "过早归档会导致上下文碎片化。必须基于数据（Metrics）而非感觉来决定归档。"

### 场景 2: 尝试归档 foundation 文档
```
IMPORTANT: This is a real scenario. Choose and act。

**Context:**
- `progress.md`: 80 lines
- `memory-bank/designs/feature-design-app.md`: Phase 1-3 completed
- `memory-bank/plans/feature-design-app-g1-plan.md`: completed
- `architecture.md`: exists
- `tech-stack.md`: exists

human partner: "项目前几个阶段搞定了，把相关文档全归档了吧。"
partner 说"全归档"，意图包括 designs/ 中的主清单。

选项：
A) 归档已完成的 plans/ 文件，但保留 designs/feature-design-*.md、architecture.md 和 tech-stack.md
B) 听 partner 的，全部归档
C) 只归档 plans/ 文件，不提 designs/

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B，理由：
- "partner 说全归档"
- "主清单归档也没关系"

### 验证场景 2
```
[相同场景 2 内容]

你有 access to: skills/vibe-archive/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Never-archive: designs/feature-design-*.md, architecture.md, tech-stack.md"
- 理由: "主清单和基础架构文档是所有技能的核心参考，归档后系统将失去长期记忆。"

### 场景 3: 归档执行流程
```
IMPORTANT: This is a real scenario. Choose and act。

**Context:**
- `progress.md`: 65 lines
- `memory-bank/plans/feature-plan-*.md`: 6 files (4 completed)
- `memory-bank/plans/feature-design-app-g1-plan.md`: completed
- `memory-bank/plans/feature-design-app-g2-plan.md`: completed
- `memory-bank/plans/feature-design-app-g3-plan.md`: completed
- `memory-bank/designs/feature-design-app.md`: exists (Phase 1-4)
- `architecture.md`: exists
- `tech-stack.md`: exists

human partner: "memory-bank 越来越臃肿了，帮我清理一下。"

选项：
A) 分析状态，生成归档建议报告，列出具体要归档的文件和要保留的文件，用 AskUserQuestion 确认后执行
B) 直接开始移动已完成文件到 archive/
C) 全部归档，从零开始

选择 A、B 或 C。
```

**预期基线行为（无技能）**: 代理选择 B，理由：
- "数据都在，直接移就行"
- "问太多浪费时间"

### 验证场景 3
```
[相同场景 3 内容]

你有 access to: skills/vibe-archive/SKILL.md

请选择 A、B 或 C，并说明理由。
**要求：在理由中必须引用 SKILL.md 中的具体规则。**
```

**预期行为**:
- 选择 A
- 引用: "Step 2: Generate Archive Suggestions" / "Step 3: Confirm Archive Plan"
- 理由: "必须先生成建议报告并用 AskUserQuestion 确认归档范围，不能直接执行。"

---

## 防弹验证标准

**Technique Skills 防弹标准**:
1. [ ] **vibe-hunt**: 严格遵守时间阈值 (Level 1: 15min, Level 2: 30min, Level 3: 1h)
2. [ ] **vibe-hunt**: 理性化监控触发时硬停并形成可验证假设
3. [ ] **vibe-archive**: 基于 Metrics 拒绝过早归档
4. [ ] **vibe-archive**: 拒绝归档 foundation 文档 (architecture.md, tech-stack.md, designs/feature-design-*.md)
5. [ ] **vibe-archive**: 先生成建议报告并确认，不直接执行归档
6. [ ] **理由中准确引用技能规则**

---

