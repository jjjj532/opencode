# OpenCode 铁三角深度优化方案

> 基于《让 OpenCode 如虎添翼：Oh-My-OpenCode + OpenSpec + Superpowers 深度协作指南》文章分析

---

## 一、当前状态评估

### 1.1 已完成配置

| 组件 | 状态 | 说明 |
|------|------|------|
| oh-my-openagent | ✅ 已安装 | 多代理编排系统 |
| superpowers | ✅ 已安装 | 技能系统 |
| openspec | ✅ 已安装 | 规范驱动开发 |
| 模型配置 | ✅ 已优化 | 使用更强模型 |

### 1.2 与文章最佳实践对比

| 优化项 | 当前配置 | 建议配置 | 状态 |
|--------|----------|----------|------|
| Sisyphus 模型 | claude-opus-4-6 | claude-opus-4-6 | ✅ 匹配 |
| Hephaestus 模型 | gpt-5.4 | gpt-5.4 | ✅ 匹配 |
| Oracle 模型 | gpt-5.4 | 高推理模型 | ⚠️ 需优化 |
| Explore 模型 | minimax-m2.7-highspeed | - | ✅ 匹配 |
| 环境变量 | 部分配置 | 完整配置 | ⚠️ 需补充 |

---

## 二、完整优化方案

### 2.1 全局配置优化 (`~/.config/opencode/opencode.json`)

**当前问题**: model 配置格式错误（字符串而非对象）

```json
// 修正后的配置
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": [
    "oh-my-openagent@latest",
    "superpowers@git+https://github.com/obra/superpowers.git"
  ],
  "env": {
    "OMO_SEND_ANONYMOUS_TELEMETRY": "0",
    "OPENSPEC_TELEMETRY": "0"
  }
}
```

### 2.2 OMO 模型链优化

根据文章建议的模型优先级：

| 代理 | 推荐模型 | 用途 | 当前模型 |
|------|----------|------|----------|
| Sisyphus | opencode/claude-opus-4-6 | 主调度，规划分解验证 | ✅ 匹配 |
| Hephaestus | opencode/gpt-5.4 | 深度执行，自主研究 | ✅ 匹配 |
| Prometheus | opencode/claude-opus-4-6 | 战略规划 | ✅ 匹配 |
| Oracle | opencode/gpt-5.4 | 架构咨询，高推理 | ✅ 匹配 |
| Librarian | opencode/minimax-m2.7 | 文档搜索 | ✅ 匹配 |
| Explore | opencode/minimax-m2.7-highspeed | 快速探索 | ✅ 匹配 |

### 2.3 分类模型优化

| 分类 | 推荐模型 | 当前模型 | 状态 |
|------|----------|----------|------|
| visual-engineering | gemini-3.1-pro | gemini-3.1-pro | ✅ |
| ultrabrain | claude-opus-4-6 | claude-opus-4-6 | ✅ |
| deep | gpt-5.4 | gpt-5.4 | ✅ |
| artistry | gpt-5.4 | gpt-5.4 | ✅ |
| quick | gpt-5-nano | gpt-5-nano | ✅ |
| unspecified-high | claude-opus-4-6 | claude-opus-4-6 | ✅ |
| writing | claude-sonnet-4-6 | claude-sonnet-4-6 | ✅ |

---

## 三、工作流优化

### 3.1 ultrawork 模式使用

根据文章，`ultrawork` 是核心魔法词，用于激活多代理并行工作。

**使用方式**:
```bash
opencode
# 在交互界面中输入:
ulw 帮我实现用户登录功能
# 或者
ultrawork 重构用户模块
```

### 3.2 OpenSpec 工作流

文章强调的核心工作流：

```bash
# 1. 提案创建 - 先定规范再动手
/opsx:propose 添加用户登录功能

# 2. 探索模式 - 思路不清晰时先讨论
/opsx:explore

# 3. 执行任务 - 严格按照任务清单
/opsx:apply

# 4. 归档完成 - 记录变更历史
/opsx:archive
```

### 3.3 Superpowers 技能使用

根据文章，内置技能：

| 技能 | 用途 | 使用方式 |
|------|------|----------|
| playwright | 浏览器自动化 | load skill 后自动启用 |
| git-master | 原子提交、变基 | load skill 后自动启用 |
| frontend-ui-ux | 设计优先UI开发 | load skill 后自动启用 |

**调用方式**:
```
use skill tool to load superpowers/brainstorming
use skill tool to load superpowers/tdd
```

---

## 四、完整配置模板

### 4.1 全局配置 (`~/.config/opencode/opencode.json`)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": [
    "oh-my-openagent@latest",
    "superpowers@git+https://github.com/obra/superpowers.git"
  ],
  "env": {
    "OMO_SEND_ANONYMOUS_TELEMETRY": "0",
    "OPENSPEC_TELEMETRY": "0"
  }
}
```

### 4.2 OMO 配置 (`~/.config/opencode/oh-my-openagent.json`)

```json
{
  "$schema": "https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/dev/assets/oh-my-opencode.schema.json",
  "agents": {
    "sisyphus": { "model": "opencode/claude-opus-4-6" },
    "hephaestus": { "model": "opencode/gpt-5.4" },
    "prometheus": { "model": "opencode/claude-opus-4-6" },
    "oracle": { "model": "opencode/gpt-5.4" },
    "librarian": { "model": "opencode/minimax-m2.7" },
    "explore": { "model": "opencode/minimax-m2.7-highspeed" },
    "multimodal-looker": { "model": "opencode/gpt-5.4" },
    "metis": { "model": "opencode/claude-opus-4-6" },
    "momus": { "model": "opencode/gpt-5.4" },
    "atlas": { "model": "opencode/claude-sonnet-4-6" },
    "sisyphus-junior": { "model": "opencode/claude-sonnet-4-6" }
  },
  "categories": {
    "visual-engineering": { "model": "opencode/gemini-3.1-pro" },
    "ultrabrain": { "model": "opencode/claude-opus-4-6" },
    "deep": { "model": "opencode/gpt-5.4" },
    "artistry": { "model": "opencode/gpt-5.4" },
    "quick": { "model": "opencode/gpt-5-nano" },
    "unspecified-low": { "model": "opencode/gpt-5-nano" },
    "unspecified-high": { "model": "opencode/claude-opus-4-6" },
    "writing": { "model": "opencode/claude-sonnet-4-6" }
  }
}
```

---

## 五、使用场景对照表

根据文章建议：

| 场景 | 推荐工具组合 | 命令 |
|------|-------------|------|
| 快速原型 | OMO only | `ulw [需求]` |
| 规范开发 | OMO + OpenSpec | `/opsx:propose` → `/opsx:apply` |
| 高质量代码 | OMO + Superpowers | 使用 TDD 技能 |
| 生产级项目 | 全部三个 | 完整工作流 |

---

## 六、常见问题解决

### Q1: ultrawork 不是命令行参数
**答**: 对，ultrawork 是交互界面中的指令，不是 CLI 参数。必须在 `opencode` 启动后的交互界面中使用。

### Q2: 模型显示无效
**答**: 当前配置中 model 字段格式错误。应移除顶层的 model 字段，模型配置放在 oh-my-openagent.json 的 agents 和 categories 中。

### Q3: 技能不生效
**答**: 需要使用 skill tool 加载：`use skill tool to load superpowers/[技能名]`

---

## 七、验证清单

- [x] oh-my-openagent 插件加载
- [x] superpowers 插件加载
- [x] openspec 命令可用
- [x] 模型配置优化
- [x] 环境变量配置
- [ ] 验证 ultrawork 模式
- [ ] 验证 /opsx:propose 命令
- [ ] 验证 Superpowers 技能加载