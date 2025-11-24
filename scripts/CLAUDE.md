[根目录](../CLAUDE.md) > **scripts**

# 构建脚本模块

## 模块职责

构建脚本模块负责gemini-cli项目的构建、开发、测试、部署和发布全流程自动化。它确保项目的一致性、可重复性和高质量交付。

## 入口与启动

### 核心构建脚本
- **`build.js`**: 主构建脚本，处理依赖安装、工作空间构建、沙盒镜像
- **`start.js`**: 开发模式启动脚本，支持调试和沙盒环境
- **`clean.js`**: 清理脚本，移除构建产物和临时文件

### 脚本执行方式
```bash
# 主构建
node scripts/build.js

# 开发模式
node scripts/start.js

# 清理环境
node scripts/clean.js
```

## 对外接口

### 构建系统接口

#### Build Script (build.js)
```javascript
// 自动依赖检查和安装
if (!existsSync('node_modules')) {
  execSync('npm install', { stdio: 'inherit' });
}

// 工作空间构建
execSync('npm run generate', { stdio: 'inherit' });
execSync('npm run build --workspaces', { stdio: 'inherit' });

// 沙盒镜像构建（可选）
if (process.env.BUILD_SANDBOX === 'true') {
  execSync('node scripts/build_sandbox.js -s');
}
```

#### Start Script (start.js)
```javascript
// 开发环境配置
const env = {
  ...process.env,
  CLI_VERSION: pkg.version,
  DEV: 'true',
};

// 调试支持
if (process.env.DEBUG) {
  nodeArgs.push('--inspect-brk');
  env.GEMINI_CLI_NO_RELAUNCH = 'true';
}

// 启动CLI
spawn('node', nodeArgs, { stdio: 'inherit', env });
```

### 包管理系统
- **`build_package.js`**: 单包构建脚本
- **`prepare-package.js`**: 包发布准备
- **`check-lockfile.js`**: 锁文件一致性检查

## 关键依赖与配置

### 构建工具
- **ESBuild**: 快速TypeScript/JavaScript构建
- **TypeScript**: 类型编译和检查
- **NPM Workspaces**: 多包管理
- **Docker**: 容器化构建支持

### 开发工具
- **Vitest**: 测试框架
- **ESLint**: 代码检查
- **Prettier**: 代码格式化
- **Husky**: Git hooks管理

### 环境变量
- `BUILD_SANDBOX`: 控制沙盒镜像构建
- `DEBUG`: 启用调试模式
- `DEBUG_PORT`: 调试端口配置
- `GEMINI_SANDBOX`: 沙盒环境类型

## 核心脚本功能

### 1. 构建管理 (build.js)
- **依赖自动安装**: 检测缺失依赖并自动安装
- **工作空间构建**: 并行构建所有子包
- **代码生成**: 运行代码生成器创建必要文件
- **沙盒集成**: 可选的容器化环境构建

### 2. 开发环境 (start.js)
- **构建状态检查**: 验证项目构建完整性
- **调试支持**: 集成Node.js调试器
- **沙盒检测**: 自动检测和配置沙盒环境
- **环境变量管理**: 设置开发特定配置

### 3. 清理系统 (clean.js)
- **构建产物清理**: 移除dist、bundle等目录
- **依赖清理**: 删除node_modules
- **临时文件清理**: 清理生成文件和临时数据
- **工作空间清理**: 递归清理所有子包

### 4. 包管理 (prepare-package.js)
- **版本同步**: 同步包版本信息
- **依赖验证**: 检查依赖完整性
- **发布准备**: 准备NPM发布所需文件

## 专业化工具脚本

### 测试工具
- **`lint.js`**: 代码检查脚本
- **`deflake.js`**: 测试稳定性检查
- **`test-windows-paths.js`**: Windows路径兼容性测试

### 文档生成
- **`generate-keybindings-doc.ts`**: 快捷键文档生成
- **`generate-settings-doc.ts`**: 设置文档生成
- **`generate-settings-schema.ts`**: 设置模式生成

### 遥测系统
- **`telemetry.js`**: 遥测数据收集主脚本
- **`telemetry_gcp.js`**: Google Cloud遥测
- **`telemetry_genkit.js`**: Genkit遥测集成
- **`local_telemetry.js`**: 本地遥测处理

### 版本管理
- **`version.js`**: 版本号管理
- **`get-release-version.js`**: 发布版本获取
- **`generate-git-commit-info.js`**: Git提交信息生成

### 沙盒支持
- **`sandbox_command.js`**: 沙盒命令管理
- **`build_sandbox.js`**: 沙盒镜像构建

### 发布管理
- **`releasing/`**: 发布相关脚本
  - `create-patch-pr.js`: 创建补丁PR
  - `patch-comment.js`: 补丁评论管理
  - `patch-trigger.js`: 补丁触发器

## 质量保证

### 自动化检查
- **代码质量**: ESLint + Prettier自动化
- **类型安全**: TypeScript严格模式检查
- **测试覆盖**: Vitest测试执行和覆盖率
- **安全扫描**: 依赖漏洞检测

### CI/CD集成
- **多平台支持**: Linux、macOS、Windows
- **容器化**: Docker和Podman支持
- **并行构建**: 最大化构建效率
- **自动化发布**: 从提交到发布的全流程自动化

## 开发工作流

### 开发环境设置
```bash
# 1. 克隆项目
git clone <repository>
cd gemini-cli

# 2. 安装依赖和构建
npm run build

# 3. 启动开发模式
npm run start
```

### 测试和验证
```bash
# 运行所有测试
npm run test

# 代码检查
npm run lint

# 类型检查
npm run typecheck

# 集成测试
npm run test:integration
```

### 发布流程
```bash
# 准备发布
npm run prepare-release

# 发布到NPM
npm run publish

# 创建GitHub Release
npm run create-release
```

## 配置和定制

### 构建配置
- **ESBuild配置**: 快速构建优化
- **TypeScript配置**: 严格类型检查
- **工作空间配置**: 多包管理设置

### 环境配置
- **开发环境**: 本地开发优化配置
- **测试环境**: CI/CD专用配置
- **生产环境**: 发布和部署配置

## 脚本统计

- **总脚本数量**: 45个脚本文件
- **核心构建脚本**: 12个主要脚本
- **测试工具**: 5个测试相关脚本
- **文档生成**: 3个文档生成脚本
- **遥测系统**: 4个遥测相关脚本
- **发布管理**: 4个发布相关脚本

## 变更记录 (Changelog)

**2025-11-24**: 构建脚本模块完整分析完成
- 完成45个脚本文件的全面扫描和分类
- 详细分析构建系统、开发工具、质量管理、发布流程
- 识别7大功能域：构建管理、开发环境、清理系统、包管理、专业化工具、质量保证、CI/CD集成
- 理解完整的自动化构建和发布流水线