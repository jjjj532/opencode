# OpenCode铁三角：OpenSpec、Superpowers与OMO的深度解析

## 一、铁三角概述

在AI辅助编程领域，OpenCode生态系统中存在三个核心工具，它们相互协作，共同构建了一套完整的“规范→规划→执行→验证→归档”的开发工作流。这三个工具分别是：OpenSpec负责规范定义，Superpowers负责流程强制，Oh-My-OpenCode（OMO）负责底层工具支持。它们并非相互替代的关系，而是各司其职、协同工作的有机整体。

这种设计理念源于对AI编程痛点的深刻洞察。许多开发者在使用AI编程助手时都经历过类似的困扰：当你告诉AI“添加一个用户登录功能”时，它确实会写出一段代码，但可能遗漏了密码加密；当你补充“需要加密”时，它可能又忘记了登录失败次数限制；当你再次补充限制条件时，之前已经确认的邮箱验证功能又被破坏了。这种来回折腾的经历表明，AI缺乏对整体需求的系统性理解和长期记忆能力。

铁三角的设计正是为了解决这一问题。OpenSpec通过强制性的规范文档确保需求被完整记录，Superpowers通过严格的工作流强制AI按步骤执行，OMO则提供强大的多智能体协作能力提升执行效率。三者配合使用时，能够将“随意编码”转变为“规范开发”，大幅提升代码质量和项目可维护性。

## 二、各组件详解

### 2.1 OpenSpec：规范层的设计理念

OpenSpec是一个轻量级的规范驱动开发框架，其核心理念可以概括为“在写代码之前，先让AI和你达成共识”。官方文档对此的描述是：“Agree before you build — human and AI align on specs before any code gets written.”用更直白的话说，就是先定规矩再写代码。

OpenSpec解决的核心问题是AI经常“写跑偏”。当你让它实现一个功能时，它可能加入许多不必要的代码，或者遗漏关键的业务逻辑。OpenSpec通过强制开发者先用文档将需求固定下来，让AI严格按照文档执行，从而避免自由发挥导致的偏差。

该框架最巧妙的设计在于将“当前状态”和“变更提案”分离管理。规范文件存放在specs目录中，代表系统的当前真相来源，类似Git的main分支；而变更文件存放在changes目录中，包含所有提议中的、进行中的以及已归档的变更，类似Git的功能分支。这种设计确保AI在修改现有功能时不会破坏已有规范，同时也便于回滚操作。

安装OpenSpec需要使用npm命令全局安装@ fission-ai/openspec包。安装完成后，可以通过openspec init命令在项目中初始化，会自动创建openspec文件夹并配置相关的斜杠命令。在Windows系统上，如果PowerShell报错无法加载文件，需要以管理员身份运行Set-ExecutionPolicy RemoteSigned -Scope CurrentUser来允许执行本地脚本。

OpenSpec 1.0版本之后默认启用core workflow profile，包含四个核心命令：/opsx:explore用于探索模式，仅讨论不产出文档；/opsx:propose用于一次性生成所有规划文档；/opsx:apply用于按tasks.md逐项实施代码；/opsx:archive用于归档完成的变更。如果需要启用扩展命令如/opsx:verify等，需要手动切换到custom profile。

### 2.2 Superpowers：能力层的工程纪律

Superpowers不是用来写代码的工具，而是一套强制性的工作流体系。官方文档对其的定义为：“Superpowers is a complete software development workflow for your coding agents, built on top of a set of composable skills and some initial instructions that make sure your agent uses them.”简而言之，它将软件工程的最佳实践转化为AI必须遵守的强制规则。

这个工具解决的核心问题是AI太“聪明”导致总想走捷径。AI模型倾向于跳过测试、不经审查就直接写代码，而Superpowers通过强制性的步骤约束，确保AI按照标准软件工程流程行事。该项目在GitHub上获得了11.5万颗星，在官方插件市场中拥有23万次安装，是Claude Code第三方插件中排名首位的工具。

Superpowers内置了一套完整的七步工作流，AI会自动按顺序执行：第一步brainstorming进行头脑风暴，通过苏格拉底式提问彻底问清楚需求；第二步using-git-worktrees创建隔离的Git工作空间；第三步writing-plans将任务拆解成2至5分钟的小任务；第四步subagent-driven-development派子智能体执行，采用两阶段审查；第五步test-driven-development强制执行TDD，不写测试就删代码；第六步requesting-code-review进行代码审查，关键问题会阻塞进度；第七步finishing-a-development-branch完成分支并提供合并选项。这些技能是自动触发的，开发者只需说出需求，AI就会自动调用相应技能。

Superpowers对测试的态度极为硬核，采用先写测试再写代码的策略，如果在测试之前写了代码会被直接删除。这种机制确保AI不会跳过测试环节，严格遵循RED-GREEN-REFACTOR循环：先写一个预期会失败的测试，运行确认失败后写最少的代码让它通过，最后进行重构。开发者反馈使用Superpowers后，测试覆盖率从不到30%提升到了85%至95%。

安装Superpowers有两种方式：可以通过git clone到指定目录然后创建符号链接，也可以在opencode.json中添加插件配置。由于网络访问限制，推荐使用第二种方式直接在配置文件中添加插件引用。

### 2.3 OMO：基础设施层的多智能体协作

Oh-My-OpenCode（简称OMO，现已更名为Oh My OpenAgent）是OpenCode的增强插件，核心能力是多智能体协作和并行执行。它不是流程工具也不是规范工具，而是将OpenCode的底层能力发挥到极致。

OMO解决的核心问题是单个AI模型能力有限，复杂任务处理速度慢。通过引入多智能体协作，多个专业AI可以同时工作：一位担任架构师角色，一位担任代码库专家角色，一位担任前端工程师角色，并行工作且互不干扰。

OMO内置了一支完整的AI团队：Sisyphus作为团队领袖负责规划和调度；Oracle作为架构师负责分析架构、评估风险、做出技术决策；Librarian作为代码库专家负责查阅文档和搜索代码；Explore作为侦察兵负责极速扫描代码库寻找特定模式；Frontend作为前端工程师负责UI实现和样式调整；Document-Writer作为技术作家负责编写README和API文档。在对话中可以直接使用@oracle这样的语法调用某个专家。

OMO最常用的功能是“魔法词ulw”（UltraWork）。在提示词前加上ulw，OMO会自动激活全部增强功能：多代理并行、深度工具链、子任务智能调度。例如输入“ulw帮我实现用户登录功能，支持JWT和刷新令牌”，Sisyphus会先进行规划，Oracle评审方案，Librarian搜索现有代码，Frontend和Backend分别实现，全部并行执行。

此外，OMO还支持后台并行任务，可以同时运行多个子智能体而互不阻塞。你可以让Oracle检查后端安全，Librarian搜索API文档，Frontend编写UI代码，三个任务同时进行，结果自动汇总。

OMO的另一个重要功能是/init-deep深度初始化。在项目目录下首次启动OpenCode后执行该命令，系统会探索整个工程，生成分层的AGENTS.md文件，后续AI执行任务时通过读取这些文件快速了解项目结构。

## 三、协作流程与实际应用

铁三角的完整协作流程可以分为五个阶段。在提案创建阶段，OpenSpec主导，用户输入/opsx:propose命令，系统解析命令并创建变更目录，生成proposal.md、design.md、specs目录和tasks.md四个工件。此时OMO提供底层的文件系统操作，Superpowers提供文档生成代理。

在需求澄清阶段，Plan Agent（Superpowers的能力）会动态介入。当发现规范中的技术栈与实际需求不符时，Plan Agent会重新制定计划，将任务分解为多个Wave，每个Wave内任务并行执行，并明确具体任务和依赖关系以及测试策略。这种动态调整机制确保规范不会被机械执行。

在并行实现阶段，Superpowers开始执行TODO列表。实际执行中会多次调用task，每次创建一个子代理，每个子代理内部使用OMO工具进行文件创建、读取、代码诊断验证和测试运行。

在测试验证阶段，运行测试命令验证代码正确性，OMO和Superpowers协同提供测试环境和结果解析。

在归档阶段，用户输入/opsx:archive命令，OpenSpec完成闭环，保留规范文档和实际代码的分离，既保证可追溯性又不限制灵活调整。

这种协作方式在规范性、灵活性、效率、可追溯性和可复用性五个维度都表现出色。OpenSpec确保了完整的文档链，Plan Agent支持动态调整技术栈，并行task执行高效完成任务，每个工件都有清晰的版本历史，归档的规范可供未来项目参考。

## 四、使用场景选择

根据不同的开发场景，应选择不同的工具组合。快速原型验证场景适合只使用OMO，追求速度不需纠结质量；核心业务代码开发场景适合使用Superpowers，强制TDD保证质量；团队协作且需要文档的场景适合使用OpenSpec，规范统一且可追溯；生产级项目开发则需要三者协同，规范、流程和效率缺一不可；对于仅修改一行代码的bugfix，使用原生OpenCode即可，杀鸡不用牛刀。

关于三个工具之间的依赖关系，它们没有硬性依赖可以独立使用，但配合使用效果更佳。实际操作中，OpenSpec负责“定方向”，Superpowers负责“定计划”，OMO负责“干实事”。

## 五、总结

OpenCode铁三角代表了一套完整的AI驱动开发范式。OpenSpec通过规范驱动确保需求被完整记录和追踪，Superpowers通过强制工作流确保流程被严格执行，OMO通过多智能体协作确保执行效率最大化。三者组合起来，构成了一套从“随意编码”到“规范开发”的完整进化路径。

理解这三个工具的定位和协作方式，是真正掌握OpenCode的关键。不是工具越多越好，而是要知道什么时候用什么。希望本文能够帮助开发者理清思路，让OpenCode真正成为提升生产力的利器。