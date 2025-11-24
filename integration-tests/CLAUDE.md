[根目录](../CLAUDE.md) > **integration-tests**

# 集成测试模块

## 模块职责

集成测试模块负责验证gemini-cli的端到端功能，确保各个组件之间的正确交互和完整的使用场景能够正常工作。这是项目质量保证的核心部分。

## 入口与启动

### 测试配置
- **主配置文件**: `vitest.config.ts` - 配置测试环境、超时、并行性
- **全局设置**: `globalSetup.ts` - 环境初始化、ripgrep下载、临时目录管理
- **测试助手**: `test-helper.ts` - 核心测试工具类和辅助函数

### 测试环境特点
- 5分钟超时设置，支持长时间运行的AI交互测试
- 8-16线程并行执行，提高测试效率
- 自动环境隔离，使用临时目录避免冲突
- 支持沙盒模式和容器化测试

## 对外接口

### TestRig 测试框架
核心测试工具类，提供完整的CLI交互和验证能力：

```typescript
class TestRig {
  // 基础测试方法
  run(prompt: string, ...args: string[]): Promise<string>
  runCommand(args: string[]): Promise<string>
  runInteractive(...args: string[]): Promise<InteractiveRun>

  // 文件操作
  createFile(fileName: string, content: string): string
  readFile(fileName: string): string
  mkdir(dir: string): void

  // 验证和调试
  waitForToolCall(toolName: string): Promise<boolean>
  waitForTelemetryEvent(eventName: string): Promise<boolean>
  readToolLogs(): ToolLog[]
}
```

### InteractiveRun 交互式测试
支持实时交互的测试环境，模拟用户实际使用场景：

```typescript
class InteractiveRun {
  expectText(text: string, timeout?: number): Promise<void>
  type(text: string): Promise<void>
  sendText(text: string): Promise<void>
  sendKeys(text: string): Promise<void>
  kill(): Promise<void>
}
```

## 关键依赖与配置

### 测试框架
- **Vitest**: 现代化测试框架，支持TypeScript和并行执行
- **Node PTY**: 伪终端支持，实现真实的终端交互测试
- **Express**: 用于MCP服务器测试的HTTP服务
- **MCP SDK**: Model Context Protocol集成测试

### 环境配置
- `INTEGRATION_TEST_FILE_DIR`: 测试文件存储目录
- `GEMINI_CLI_INTEGRATION_TEST`: 集成测试标识
- `GEMINI_FORCE_FILE_STORAGE`: 强制文件存储，避免密钥环提示
- `TELEMETRY_LOG_FILE`: 遥测日志文件路径

## 测试覆盖领域

### 1. 文件系统操作测试
- **文件读写验证**: 测试基本文件操作的正确性
- **路径处理**: 验证包含空格和特殊字符的文件路径
- **多步骤操作**: 测试读取-修改-写入的完整流程
- **错误处理**: 验证不存在文件的安全处理

### 2. 扩展系统测试
- **本地扩展安装**: 验证扩展的安装、列表、更新流程
- **扩展生命周期**: 测试扩展的完整生命周期管理
- **多版本管理**: 验证扩展版本升级和回滚

### 3. MCP协议测试
- **服务器连接**: 测试MCP服务器的连接和通信
- **工具发现**: 验证MCP工具的自动发现和注册
- **循环模式**: 测试复杂JSON Schema的循环引用处理
- **错误恢复**: 验证MCP连接失败时的恢复机制

### 4. 遥测系统测试
- **事件发射**: 验证用户交互事件的正确记录
- **指标收集**: 测试性能和使用指标的准确性
- **数据格式**: 确保遥测数据的格式正确性

### 5. 交互式功能测试
- **实时响应**: 测试AI响应的实时显示
- **中断处理**: 验证Ctrl+C等中断信号的处理
- **上下文压缩**: 测试长对话的智能压缩机制
- **模式切换**: 验证不同交互模式间的切换

### 6. 高级场景测试
- **JSON输出**: 测试结构化输出的正确性
- **Shell集成**: 验证Shell命令的安全执行
- **Web搜索**: 测试网络搜索功能的集成
- **多语言编码**: 验证UTF-8 BOM等编码的处理

## 测试数据管理

### Mock响应系统
- **响应录制**: 支持录制真实API响应用于测试
- **响应回放**: 使用预录制响应进行快速测试
- **动态响应**: 根据请求生成相应的模拟响应

### 测试环境隔离
- **临时目录**: 每个测试使用独立的临时工作目录
- **配置隔离**: 测试间不共享配置和状态
- **自动清理**: 测试完成后自动清理临时文件

## 性能与调试

### 性能监控
- **超时控制**: 不同环境下的自适应超时设置
- **并发测试**: 支持多线程并行执行提高效率
- **资源监控**: 监控内存和CPU使用情况

### 调试支持
- **详细日志**: 支持VERBOSE模式获取详细执行信息
- **输出保留**: KEEP_OUTPUT模式保留测试输出用于调试
- **工具调用跟踪**: 详细记录所有工具调用和结果

## 持续集成

### CI优化
- **平台适配**: 支持Linux、macOS、Windows多平台
- **容器化**: 支持Docker和Podman容器环境
- **并行执行**: 最大化利用CI资源进行并行测试

### 质量保证
- **重试机制**: 自动重试不稳定的测试
- **错误报告**: 详细的错误信息和调试上下文
- **覆盖率跟踪**: 监控测试覆盖率的持续改进

## 测试统计

- **总测试文件**: 45个测试文件
- **测试覆盖**: 端到端功能全覆盖
- **执行时间**: 完整测试套件约15-30分钟
- **并行度**: 支持8-16线程并行执行

## 变更记录 (Changelog)

**2025-11-24**: 集成测试完整分析完成
- 完成45个集成测试文件的全面扫描
- 详细分析TestRig测试框架和InteractiveRun交互系统
- 识别6大测试领域：文件系统、扩展系统、MCP协议、遥测、交互功能、高级场景
- 理解完整的测试环境配置和CI优化策略