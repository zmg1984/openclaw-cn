## 📦 Feishu 独立 NPM 包发布完成

### ✅ 已完成的配置

1. **编译配置**
   - ✅ `tsconfig.json` - TypeScript 编译配置
   - ✅ `src/plugin-sdk.d.ts` - 类型声明文件（支持 npm 用户的 TypeScript）

2. **包配置**
   - ✅ `package.json` - 更新为 npm 发布格式
     - `main`: 指向 `./dist/index.js`（编译产物）
     - `types`: 指向 `./dist/index.d.ts`（类型声明）
     - `exports`: 标准 ESM 导出
     - `files`: 限制包内容（只包含 dist/）

3. **源码修复**
   - ✅ 所有导入改为 `openclaw-cn/plugin-sdk`（workspace 兼容）
   - ✅ 类型名称统一为 `ClawdbotPluginApi`、`ClawdbotConfig`
   - ✅ 编译成功，无错误

4. **文档**
   - ✅ `README.md` - 安装和配置说明
   - ✅ `PUBLISHING.md` - 详细发布步骤
   - ✅ `PUBLISH_CHECKLIST.md` - 快速参考清单
   - ✅ `.npmignore` - npm 包排除规则

### 🚀 发布流程

#### 一次性准备（首次发布）
```bash
# 1. 创建 npm 账户（如果没有）
npm adduser

# 2. 配置 .npmrc（添加 token 或 2FA）
npm config set //registry.npmjs.org/:_authToken "YOUR_TOKEN"
```

#### 每次发布步骤
```bash
# 1. 更新版本号
vim extensions/feishu/package.json
# 改 "version": "2026.1.31" → "2026.2.0"

# 2. 编译
cd extensions/feishu
pnpm clean && pnpm build

# 3. 验证
npm pack  # 查看将要发布的内容

# 4. 发布
npm publish

# 5. 验证发布成功
npm view @openclaw-cn/feishu version  # 应显示新版本号
```

### 📋 双模式支持

现在 feishu 支持两种安装方式：

#### Workspace 模式（开发）
```bash
# 在 openclaw-cn 项目中
pnpm install

# 自动包含 feishu 扩展
# 使用 TypeScript 源码（./index.ts）
```

#### NPM 模式（独立使用）
```bash
# 用户直接安装
npm install @openclaw-cn/feishu

# 或通过 openclaw 命令
openclaw-cn plugins install @openclaw-cn/feishu
```

### 🔧 编译产物说明

`dist/` 目录包含：
```
dist/
├── index.js              # 主入口（编译后）
├── index.d.ts            # TypeScript 类型声明
├── index.js.map          # 源码映射
├── index.d.ts.map        # 类型源码映射
└── src/                  # 编译后的源码
    ├── channel.js
    ├── channel.d.ts
    ├── onboarding.js
    ├── onboarding.d.ts
    ├── runtime.js
    ├── runtime.d.ts
    └── plugin-sdk.d.ts   # 类型声明文件
```

### ⚙️ 运行时兼容性

#### 类型映射层
`src/plugin-sdk.d.ts` 提供：
- workspace 模式：直接导入 `openclaw-cn/plugin-sdk`
- npm 模式：用户需单独安装 `openclaw` 作为 peer dependency

#### 自动解析
运行时通过 jiti 自动：
1. 检测环境（workspace vs npm）
2. 加载对应的 SDK
3. 提供统一的 API

### 📝 后续维护

**版本更新**：
- 修复 bug → 修订版本 (2026.1.31 → 2026.1.32)
- 新功能 → 次版本 (2026.1.31 → 2026.2.0)
- 破坏性变更 → 主版本 (2026.1.31 → 2027.0.0)

**发布流程**：
1. 修改代码
2. 更新 CHANGELOG（项目根目录）
3. 更新 package.json 版本号
4. 运行 `pnpm build`（编译）
5. 运行 `npm publish`（发布）
6. 在 GitHub 创建 Release（可选但推荐）

### 🎯 下一步

选择你想要的方式：

1. **立即发布** → 按上面"每次发布步骤"操作
2. **本地测试** → 使用 `npm pack` 和 `npm install <tarball>`
3. **继续开发** → feishu 在 workspace 中开发，后续需要时再发布

详见 [PUBLISHING.md](./extensions/feishu/PUBLISHING.md)
