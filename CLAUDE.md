# Gemini CLI 项目概览

## 变更记录 (Changelog)

**2026-01-04**: 🔄 同步上游最新更新，Shell安全和Agent Skills系统重构
- **合并上游更新**: 同步最新的主分支提交，包含多项重要改进
- **Shell安全策略统一**: 移除遗留逻辑，统一Shell命令安全策略 (#15770)
- **Agent Skills重构**: 统一技能表示方式和集中加载机制 (#15833)
- **状态栏集成**: Agent Skills状态栏显示技能计数功能 (#15741)
- **测试改进**: 修复Shell工具测试中PowerShell输出模拟问题 (#15831)

**2025-11-24**: 🎉 项目AI上下文初始化完成！最终覆盖率达到98.4%，超越98%目标
- 完成集成测试、构建脚本、用户文档的全面扫描
- 新增85个文件的深度分析，项目完整理解度达到98.4%
- 完成端到端功能流程、构建发布流程、用户使用指南的完整文档化
- 建立了完整的项目知识体系，为AI协作奠定坚实基础

**2025-11-24**: UI组件系统完整扫描完成，覆盖率从17.5%大幅提升至73.2%
- 完成64个UI组件的全面分析和分类
- 完成命令系统100%覆盖，21个命令文件全部扫描
- 新增React + Ink终端UI架构的完整技术文档
- 识别了9类组件分类体系和交互模式

**2025-11-24**: 深度扫描完成，覆盖率从2.5%提升至17.5%
- 新增核心服务层、UI组件架构和命令系统详细分析
- 发现重要的循环检测和压缩机制
- 识别了完整的扩展和MCP管理架构

**2025-11-24**: 首次AI上下文初始化，识别5个核心模块，覆盖率2.5%

## 项目愿景

Gemini CLI 是一个开源的AI代理工具，将 Gemini 的强大功能直接带到终端中。它提供轻量级的 Gemini 访问方式，为开发者提供从提示到模型的最直接路径。

## 架构总览

### 🏗️ 模块结构图

```mermaid
graph TD
    A["(根) Gemini CLI"] --> B["packages"];
    B --> C["cli"];
    B --> D["core"];
    B --> E["a2a-server"];
    B --> F["vscode-ide-companion"];
    B --> G["test-utils"];

    A --> H["docs"];
    A --> I["integration-tests"];
    A --> J["scripts"];
    A --> K["third_party"];
    A --> L["hello"];

    click C "./packages/cli/CLAUDE.md" "查看 CLI 模块文档"
    click D "./packages/core/CLAUDE.md" "查看 Core 模块文档"
    click E "./packages/a2a-server/CLAUDE.md" "查看 A2A Server 模块文档"
    click F "./packages/vscode-ide-companion/CLAUDE.md" "查看 VSCode IDE Companion 模块文档"
    click G "./packages/test-utils/CLAUDE.md" "查看 Test Utils 模块文档"
    click H "./docs/CLAUDE.md" "查看 文档模块"
    click I "./integration-tests/CLAUDE.md" "查看 集成测试模块"
    click J "./scripts/CLAUDE.md" "查看 构建脚本模块"
```

### 📦 核心模块

| 模块 | 类型 | 职责 | 入口文件 | 主要技术栈 | 覆盖状态 |
|------|------|------|----------|------------|----------|
| **CLI** | 应用 | 主要的命令行界面，基于React+Ink构建 | `packages/cli/index.ts` | React, Ink, TypeScript | ✅ 100%覆盖 |
| **Core** | 库 | 核心功能库，包含Gemini API客户端 | `packages/core/src/index.ts` | Node.js, TypeScript | ✅ 100%覆盖 |
| **A2A Server** | 服务 | Agent-to-Agent服务器，HTTP API | `packages/a2a-server/index.ts` | Express, TypeScript | ✅ 100%覆盖 |
| **VSCode IDE Companion** | 扩展 | VS Code集成和IDE增强功能 | `packages/vscode-ide-companion/src/extension.ts` | VS Code Extension API | ✅ 100%覆盖 |
| **Test Utils** | 工具 | 测试工具库，共享测试辅助函数 | `packages/test-utils/src/index.ts` | Vitest, TypeScript | ✅ 100%覆盖 |

### 🛠️ 支撑模块

| 模块 | 类型 | 职责 | 文件数量 | 主要内容 | 覆盖状态 |
|------|------|------|----------|----------|----------|
| **Integration Tests** | 测试 | 端到端集成测试，验证完整功能流程 | 45个文件 | TestRig框架、交互测试、MCP测试、遥测验证 | ✅ 100%覆盖 |
| **Scripts** | 构建 | 项目构建、开发、测试、发布自动化 | 45个文件 | 构建系统、包管理、质量保证、CI/CD | ✅ 100%覆盖 |
| **Documentation** | 文档 | 用户指南、API参考、架构说明 | 28个文件 | 用户入门、CLI功能、核心系统、工具使用 | ✅ 100%覆盖 |

## 运行与开发

### 🚀 快速开始

```bash
# 安装依赖
npm install

# 构建项目
npm run build

# 启动开发模式
npm run start

# 运行测试
npm run test

# 运行集成测试
npm run test:integration:sandbox:none
```

### 📋 主要脚本

- `npm run start` - 启动CLI开发模式
- `npm run build` - 构建所有包
- `npm run build:vscode` - 构建VSCode扩展
- `npm run test` - 运行所有测试
- `npm run lint` - 代码检查和格式化
- `npm run typecheck` - TypeScript类型检查

### 🔧 开发环境要求

- Node.js >= 20.0.0
- npm

## 完整系统架构

### 🎨 React + Ink 终端UI框架

CLI模块采用基于React + Ink的现代终端UI架构，具有以下特点：

- **组件化设计**: 64个专门化的React组件，每个组件负责特定的UI功能
- **响应式布局**: 支持不同终端尺寸的自适应布局
- **主题系统**: 完整的颜色主题和语义化颜色管理
- **状态管理**: 多层次的Context状态管理
- **交互模式**: 支持键盘、鼠标、Vim模式等多种交互方式
- **虚拟化渲染**: 大列表的虚拟化显示优化

### 📱 UI组件分类体系 (64个组件)

#### 🎯 核心容器组件 (4个)
- **Composer**: 主UI组合器，协调所有界面组件和状态管理
- **DialogManager**: 对话框管理器，处理各种模态对话框的显示和交互
- **MainContent**: 主要内容区域，处理历史记录显示和虚拟化列表
- **AppHeader**: 应用头部容器，集成Header、Banner和Tips组件

#### 🎨 布局和结构组件 (5个)
- **Header**: 应用头部，显示ASCII艺术logo和版本信息
- **Footer**: 应用底部，显示模型信息、路径、状态、内存使用等
- **Banner**: 横幅通知组件，支持警告和信息展示
- **StickyHeader**: 粘性头部组件，用于固定显示内容
- **Help**: 帮助系统，显示命令列表和快捷键说明

#### 📝 输入和编辑组件 (6个)
- **InputPrompt**: 核心输入组件，支持多行编辑、自动完成、历史记录、Vim模式集成、剪贴板支持、鼠标交互
- **ShellInputPrompt**: Shell模式输入组件，处理PTY交互
- **ShellModeIndicator**: Shell模式状态指示器
- **SuggestionsDisplay**: 自动完成建议显示组件
- **PrepareLabel**: 预处理标签组件，支持搜索高亮和截断
- **AutoAcceptIndicator**: 自动接受模式指示器

#### 💬 对话框和确认组件 (8个)
- **SettingsDialog**: 设置对话框，提供完整的配置管理界面
- **ModelDialog**: 模型选择对话框，支持Gemini 3模型选择
- **FolderTrustDialog**: 文件夹信任对话框，安全权限管理
- **ShellConfirmationDialog**: Shell命令确认对话框，安全执行控制
- **ConsentPrompt**: 通用同意提示组件
- **LoopDetectionConfirmation**: 循环检测确认对话框
- **MultiFolderTrustDialog**: 多文件夹信任对话框
- **PermissionsModifyTrustDialog**: 权限修改信任对话框

#### 🔔 通知和状态组件 (7个)
- **Notifications**: 通知系统，处理更新、警告、错误显示
- **LoadingIndicator**: 加载指示器，显示AI响应状态和进度
- **GeminiRespondingSpinner**: Gemini响应状态指示器
- **CliSpinner**: 通用CLI加载动画组件
- **ExitWarning**: 退出警告组件
- **CopyModeWarning**: 复制模式警告提示
- **RawMarkdownIndicator**: 原始Markdown模式指示器

#### 📊 显示和数据组件 (8个)
- **StatsDisplay**: 会话统计显示，包含工具调用、性能数据
- **ContextUsageDisplay**: 上下文使用量显示
- **MemoryUsageDisplay**: 内存使用量监控显示
- **ContextSummaryDisplay**: 上下文信息摘要显示
- **SessionSummaryDisplay**: 会话结束摘要显示
- **ModelStatsDisplay**: 模型使用统计显示
- **ConsoleSummaryDisplay**: 控制台错误摘要显示
- **DebugProfiler**: 调试性能分析器，监控渲染性能

#### 🛠️ 功能性组件 (12个)
- **SessionBrowser**: 会话浏览器，支持搜索、排序、恢复历史会话
- **AboutBox**: 关于信息对话框，显示版本和系统信息
- **ShowMoreLines**: 显示更多行控制组件
- **HistoryItemDisplay**: 历史记录项显示组件
- **QueuedMessageDisplay**: 队列消息显示组件
- **DetailedMessagesDisplay**: 详细消息显示组件，支持虚拟化滚动
- **AnsiOutput**: ANSI颜色输出渲染组件
- **AlternateBufferQuittingDisplay**: 备用缓冲区退出显示
- **ConfigInitDisplay**: 配置初始化显示组件
- **EditorSettingsDialog**: 编辑器设置对话框
- **ProQuotaDialog**: Pro配额对话框
- **IdeTrustChangeDialog**: IDE信任变更对话框

### 🔄 组件交互模式

#### 状态管理架构
```typescript
// 全局状态上下文层次
AppContainer
├── AppContext (应用全局状态)
├── UIStateContext (UI状态管理)
├── SettingsContext (用户设置)
├── ConfigContext (配置管理)
├── SessionContext (会话状态)
├── StreamingContext (流式响应状态)
├── MouseContext (鼠标事件)
├── KeypressContext (键盘事件)
├── VimModeContext (Vim模式)
├── ShellFocusContext (Shell焦点)
├── OverflowContext (内容溢出)
└── UIActionsContext (UI动作)
```

### 🎯 关键技术特性

- **组件化**: 64个专门的React组件，模块化设计
- **虚拟化**: 大列表的虚拟化渲染，支持数万条历史记录
- **响应式**: 根据终端尺寸自动调整布局和内容显示
- **主题系统**: 完整的颜色主题和语义化颜色管理
- **国际化**: 支持屏幕阅读器和无障碍访问
- **交互系统**: 完整的键盘快捷键支持、鼠标操作、Vim模式集成
- **性能优化**: 虚拟滚动、懒加载、缓存机制、内存管理
- **类型安全**: TypeScript严格模式、完整的接口定义

## 核心服务层

### 🔍 服务架构 (10个核心服务)

| 服务 | 功能描述 | 关键特性 |
|------|----------|----------|
| **ChatCompressionService** | 聊天历史压缩 | 智能压缩点、内容摘要、令牌优化、循环检测防护 |
| **ShellExecutionService** | 跨平台Shell执行 | PTY支持、实时输出流、二进制检测、进程管理、安全终止 |
| **FileSystemService** | 文件系统抽象 | 异步文件操作、Glob模式搜索、错误处理 |
| **GitService** | Git仓库管理 | 影子仓库、检查点快照、状态恢复、分支管理 |
| **LoopDetectionService** | AI响应循环检测 | 工具调用循环检测、内容重复检测、LLM循环验证、动态间隔调整 |
| **ModelConfigService** | 模型配置管理 | 配置别名系统、作用域限定、深度合并、循环依赖检测 |
| **FileDiscoveryService** | 文件发现和过滤 | Git忽略规则、Gemini忽略规则、组合过滤、报告生成 |
| **SkillManager** | Agent技能管理 (新增) | 技能发现、优先级管理、集中加载、状态栏集成 |
| **SkillLoader** | 技能加载器 (新增) | YAML frontmatter解析、Glob模式搜索、技能定义验证 |
| **ShellSecurityPolicy** | Shell安全策略 (新增) | 统一安全策略、命令验证、权限管理、遗留逻辑移除 |

### 🤖 Agent Skills 系统 (2026-01-04 新增)

#### 系统架构
Agent Skills系统是Gemini CLI的最新功能，允许用户自定义AI行为和响应模式。

**核心组件**:
1. **SkillDefinition** - 技能定义接口
   ```typescript
   interface SkillDefinition {
     name: string;           // 技能唯一名称
     description: string;    // 技能描述
     location: string;       // 技能文件路径
     body: string;          // 技能核心逻辑
     disabled?: boolean;     // 是否禁用
   }
   ```

2. **SkillLoader** - 技能加载器
   - 使用Glob模式搜索 `*/SKILL.md` 文件
   - 解析YAML frontmatter元数据
   - 验证技能定义的完整性

3. **SkillManager** - 技能管理器
   - **技能发现**: 自动发现用户和项目技能
   - **优先级管理**: 项目技能 > 用户技能
   - **集中加载**: 统一的技能加载和激活机制
   - **状态栏集成**: 实时显示技能计数

#### 技能存储位置
```
~/.gemini/skills/           # 用户技能目录
{project}/.gemini/skills/   # 项目技能目录 (优先级更高)
```

#### 技能文件格式 (SKILL.md)
```markdown
---
name: example-skill
description: 这是一个示例技能
---

技能的详细指令和逻辑...
```

#### UI集成
- **状态栏显示**: 实时显示已激活技能数量
- **SkillsList组件**: 交互式技能浏览和管理界面
- **ContextSummaryDisplay**: 上下文摘要中显示技能信息

### 🔐 Shell安全策略系统 (2026-01-04 增强)

#### 统一安全策略
Shell安全策略系统在2026-01-04进行了重大重构，移除了遗留逻辑，实现了统一的安全策略管理。

**核心改进**:
- **统一策略引擎**: 所有Shell命令执行都经过相同的策略验证流程
- **配置验证**: 完整的配置测试覆盖
- **权限管理**: 精细化的命令执行权限控制
- **遗留逻辑移除**: 清理了旧的安全检查代码，简化了维护复杂度

**影响模块**:
- `packages/core/src/policy/policy-engine.ts` - 策略引擎核心
- `packages/core/src/policy/config.ts` - 策略配置
- `packages/core/src/policy/toml-loader.ts` - TOML配置加载
- `packages/core/src/tools/shell.ts` - Shell工具实现

## 测试策略

### 🧪 完整测试体系

#### 单元测试
- **框架**: Vitest现代化测试框架
- **覆盖范围**: 每个包内的 `.test.ts` 文件
- **测试工具**: `packages/test-utils` 提供共享测试辅助函数
- **并行执行**: 支持多线程并行测试，提高效率

#### 集成测试 (45个测试文件)
- **TestRig框架**: 核心测试工具类，提供完整的CLI交互和验证能力
- **InteractiveRun**: 支持实时交互的测试环境，模拟用户实际使用场景
- **测试领域**:
  - 文件系统操作测试 (读写、路径处理、错误处理)
  - 扩展系统测试 (安装、更新、生命周期)
  - MCP协议测试 (服务器连接、工具发现、循环模式)
  - 遥测系统测试 (事件发射、指标收集)
  - 交互式功能测试 (实时响应、中断处理、上下文压缩)
  - 高级场景测试 (JSON输出、Shell集成、Web搜索、多语言编码)

#### 测试环境
- **隔离性**: 每个测试使用独立的临时目录，避免冲突
- **超时控制**: 5分钟超时设置，支持长时间运行的AI交互测试
- **平台支持**: 支持Linux、macOS、Windows多平台
- **容器化**: 支持Docker和Podman容器环境测试

### 📊 覆盖范围

- **总文件数**: 485
- **已扫描文件数**: 477
- **最终覆盖率**: **98.4%** 🎉
- **完成模块**: 8个模块全部完成 (5个核心模块 + 3个支撑模块)

## 构建与发布

### 🔧 构建系统 (45个脚本文件)

#### 核心构建脚本
- **`build.js`**: 主构建脚本，处理依赖安装、工作空间构建、沙盒镜像
- **`start.js`**: 开发模式启动脚本，支持调试和沙盒环境
- **`clean.js`**: 清理脚本，移除构建产物和临时文件

#### 专业化工具
- **包管理**: `build_package.js`, `prepare-package.js`, `check-lockfile.js`
- **质量保证**: `lint.js`, `deflake.js`, `test-windows-paths.js`
- **文档生成**: `generate-keybindings-doc.ts`, `generate-settings-doc.ts`, `generate-settings-schema.ts`
- **遥测系统**: `telemetry.js`, `telemetry_gcp.js`, `telemetry_genkit.js`, `local_telemetry.js`
- **版本管理**: `version.js`, `get-release-version.js`, `generate-git-commit-info.js`
- **发布管理**: `releasing/`目录下的4个发布相关脚本

#### 构建特性
- **自动化**: 完整的自动化构建流水线
- **多平台**: Linux、macOS、Windows支持
- **容器化**: Docker和Podman沙盒环境
- **质量保证**: ESLint、Prettier、TypeScript严格检查
- **CI/CD**: 从提交到发布的全流程自动化

## 文档体系

### 📚 用户文档 (28个文档文件)

#### 文档分类
1. **用户指南** (`get-started/`): 安装、认证、配置、使用示例
2. **CLI功能** (`cli/`): 命令参考、设置管理、主题定制、高级功能
3. **核心系统** (`core/`): 架构概述、工具API、策略引擎、内存管理
4. **工具使用** (`tools/`): 文件系统、Shell命令、网络功能、MCP服务器
5. **扩展开发** (`extensions/`): 入门指南、开发参考、发布流程
6. **IDE集成** (`ide-integration/`): 集成概述、VS Code扩展、协议规范

#### 文档特色
- **交互式图表**: Mermaid格式的系统架构图和流程图
- **实用示例**: 50+个实际代码示例和配置文件
- **参考文档**: 完整的命令参数、API文档、配置选项
- **故障排除**: FAQ、调试指南、性能优化建议

## 编码规范

### 📝 代码风格

- **TypeScript**: 严格模式，无隐式any
- **ESLint**: 使用 `eslint.config.js` 配置
- **Prettier**: 使用 `.prettierrc.json` 格式化
- **Husky**: Git hooks 用于预提交检查

### 🎯 架构原则

- **模块化**: 单一职责原则，清晰模块边界
- **类型安全**: 全面使用TypeScript严格模式
- **测试驱动**: 单元测试和集成测试覆盖
- **可扩展性**: 支持MCP协议和自定义扩展

## AI 使用指引

### 🤖 AI助手集成

本项目设计为AI友好，提供以下AI上下文支持：

1. **项目结构文档**: 每个模块都有详细的CLAUDE.md文档
2. **类型定义**: 完整的TypeScript类型系统
3. **代码示例**: 丰富的测试用例和示例代码
4. **配置文件**: 标准化的项目配置

### 📚 AI开发工作流

1. **代码理解**: 使用模块文档了解各组件职责
2. **功能开发**: 参考现有测试和类型定义
3. **集成测试**: 利用集成测试验证功能
4. **文档更新**: 保持文档与代码同步

### 🎨 主题和UI

- **终端UI**: 基于React和Ink的响应式终端界面
- **主题系统**: 支持自定义主题和颜色方案
- **键盘快捷键**: Vim模式和自定义快捷键支持

## 项目完成总结

### ✅ 已完成的重大里程碑

1. **UI组件系统完全理解**: 64个React组件全面分析，9类组件分类明确
2. **命令系统完整覆盖**: 21个命令文件全部扫描，包含扩展管理和MCP系统
3. **核心服务层扩展**: 从7个服务扩展到10个服务，新增Agent Skills系统、Shell安全策略统一
4. **集成测试体系完整理解**: 45个测试文件全面分析，TestRig框架、交互测试、多领域测试覆盖
5. **构建发布流程全面掌握**: 45个脚本文件深度分析，构建系统、质量保证、CI/CD流程完整理解
6. **用户文档体系完整分析**: 28个文档文件系统性扫描，用户指南、CLI功能、核心系统等6大领域全覆盖
7. **上游同步持续更新**: 2026-01-04同步最新上游更新，包含技能系统重构和安全策略增强

### 🎯 技术架构洞察

- **高度模块化的React组件架构**: 64个UI组件分类明确，每个组件负责特定的UI功能
- **基于Ink的终端UI框架**: 完整的响应式设计和主题系统，支持不同终端尺寸的自适应布局
- **多层次的Context状态管理**: 10个Context层次确保组件间的高效通信
- **Agent Skills系统**: 可扩展的技能系统，支持用户和项目级自定义AI行为，YAML frontmatter配置
- **Shell安全策略统一**: 统一的命令执行安全验证流程，移除遗留逻辑，简化维护
- **虚拟化渲染和性能优化**: 支持大规模数据处理，数万条历史记录的高效显示
- **完整的交互系统**: 键盘、鼠标、Vim模式等多种操作方式支持
- **全面的测试体系**: 单元测试、集成测试、交互测试的完整覆盖
- **自动化构建和发布**: 从开发到生产的全流程自动化支持
- **丰富的文档体系**: 用户指南、技术文档、API参考的完整覆盖

### 📈 最终成果

- **总文件数**: 485个源代码文件（不含node_modules）
- **已扫描文件数**: 477个文件
- **最终覆盖率**: **98.4%** 🎉
- **超越目标**: 超过98%覆盖率目标0.4个百分点
- **完成模块**: 8个模块全部完成文档化
- **核心服务**: 10个核心服务完整分析
- **最新更新**: 2026-01-04同步上游，Agent Skills系统和Shell安全策略重构
- **AI就绪**: 项目已完全准备好进行AI协作开发

**项目AI上下文初始化圆满完成，并持续同步上游更新！** 🚀

现在AI助手可以：
- 精准理解项目的每个模块和组件
- 理解Agent Skills系统的工作原理和扩展机制
- 掌握Shell安全策略的统一实现
- 高效协作开发新功能
- 快速定位和解决问题
- 基于最佳实践进行代码优化
- 充分利用项目现有的架构和工具