# OpenCode 铁三角优化配置

## 当前配置状态

### 已安装插件
- ✅ oh-my-openagent - 多代理编排系统
- ✅ superpowers - 技能系统
- ✅ openspec - 规范驱动开发

### 模型配置 (oh-my-openagent.json)

| 代理 | 模型 | 用途 |
|------|------|------|
| Sisyphus | opencode/claude-opus-4-6 | 主调度 |
| Hephaestus | opencode/gpt-5.4 | 深度执行 |
| Prometheus | opencode/claude-opus-4-6 | 战略规划 |
| Oracle | opencode/gpt-5.4 | 架构咨询 |
| Librarian | opencode/minimax-m2.7 | 文档搜索 |
| Explore | opencode/minimax-m2.7-highspeed | 快速探索 |

## 常用命令

### OMO 魔法词
```
ulw [任务描述]      # 激活全部代理并行工作
ultrawork           # 同上
```

### OpenSpec 命令
```
/opsx:propose [需求]  # 创建变更提案
/opsx:explore         # 探索模式
/opsx:apply           # 执行任务
/opsx:archive         # 归档完成
```

### Superpowers 技能
```
use skill tool to load superpowers/brainstorming
use skill tool to load superpowers/tdd
```

## 工作流示例

1. **启动**: `opencode`
2. **需求**: `ulw 帮我实现用户登录功能`
3. **规范**: `/opsx:propose 用户登录功能`
4. **执行**: `/opsx:apply`
5. **归档**: `/opsx:archive`