# Windows 应用项目结构规范

本规范定义仓库源码与交付目录的组织原则。具体项目可以按规模增减目录；目录名称和职责应清楚，不为满足模板而创建空目录。

## 推荐的源码仓库结构

```text
<AppName>/
├── .github/                    # CI、Issue 模板等（按需）
├── <AppName>/                  # 程序主体源码与项目文件
├── <AppName>.Tests/            # 自动化测试（按需）
├── docs/
│   ├── skills/                 # 项目专用 Skills
│   ├── guides/                 # 用户指南、开发说明、部署与排障说明
│   ├── reference/              # API、数据格式、接口和技术参考
│   ├── specs/                  # 已确认需求和行为规格（按需）
│   ├── architecture/            # 架构决策记录（按需）
│   └── archive/                # 已废弃但需追溯的资料（按需）
├── tools/                      # 辅助脚本和开发维护工具（按需）
├── artifacts/                  # 构建、测试、诊断中间产物（按需）
├── dist/                       # 正式发布产物（按需）
├── .gitignore
├── .editorconfig                # 按需
├── AGENTS.md
├── README.md
├── <AppName>.slnx               # 或 .sln，按 SDK 和工具支持情况选择
└── Directory.Build.props        # 按需
```

## 目录职责

| 路径 | 职责 | 约束 |
|---|---|---|
| `<AppName>/` | 程序主体源码、资源和项目文件 | 不混入无关测试、历史包和发布副本 |
| `<AppName>.Tests/` | 单元、集成及相关测试 | 测试资源与测试入口清楚 |
| `docs/skills/` | 项目专用 Skill | 每个 Skill 有清晰用途和入口 |
| `docs/guides/` | 面向用户或开发者的说明 | 按主题组织，避免混入临时日志 |
| `docs/reference/` | 接口、格式、参数和技术参考 | 与实际程序版本保持一致 |
| `docs/specs/` | 已确认的需求和行为规格 | 仅在项目需要时创建 |
| `docs/architecture/` | 架构决策和重要技术选择 | 仅记录仍有效或需追溯的决定 |
| `docs/archive/` | 已废弃但有历史价值的资料 | 明确标注废弃状态，避免被误认为现行规则 |
| `tools/` | 开发、打包、迁移、诊断等辅助工具 | 不放应用主业务代码 |
| `artifacts/` | 构建、测试和诊断中间产物 | 默认不提交 Git，除非项目明确需要 |
| `dist/` | 准备交付的正式程序 | 不作为随手堆放旧版本的工作区 |

根目录用于存放整个仓库的配置和入口文件。允许存在 `.sln`、`.slnx`、`global.json`、`Directory.Build.props`、`NuGet.Config`、`.editorconfig`、`LICENSE`、`CHANGELOG.md` 等与项目工具链相关的文件。不得为了目录整齐破坏工具的标准发现机制。

## 文档分类

- `guides/` 回答“如何使用、开发、部署和排障”。
- `reference/` 回答“接口、格式、参数和技术事实是什么”。
- `specs/` 记录“程序应该表现为何种已确认行为”。
- `architecture/` 记录“为什么采用某个重要架构决定”。
- `archive/` 仅保存需要追溯的废弃材料。

简单项目可以只使用 `guides/` 和 `reference/`；其他目录按需建立。项目契约卡不是完整规格文档，应链接到权威规格。

## 发布目录与程序主体的区别

源码仓库中的 `<AppName>/` 是源码项目目录；发布目录中的 `<AppName>/` 是与可执行文件同级的应用数据及私有运行时目录。二者处于不同生命周期，不应混淆。

典型便携式发布布局：

```text
dist/
└── <AppName>/
    ├── <AppName>.exe
    ├── <AppName>.dll               # 若发布形态需要
    ├── <AppName>.deps.json         # 若发布形态需要
    ├── <AppName>.runtimeconfig.json # 若发布形态需要
    ├── <AppName>/                  # 可执行文件同级的应用专属目录
    │   ├── userdata/
    │   │   ├── config/
    │   │   ├── data/
    │   │   ├── projects/
    │   │   ├── cache/
    │   │   ├── logs/
    │   │   └── backups/
    │   └── runtimes/
    │       ├── chromium/
    │       └── ffmpeg/
    └── README.txt
```

如果项目需要将多个可执行文件或辅助进程一起交付，应明确主入口和各组件职责。一个软件包默认只有一套权威正式发布产物；按需允许不同架构、安装形式或辅助工具，但必须清楚标识版本和用途。

## Git 与清理

- 默认将 `bin/`、`obj/`、测试临时输出和可再生 `artifacts/` 排除在版本控制之外。
- `dist/` 是否纳入版本控制由项目交付方式决定；默认不把多个旧发布版本长期堆积在仓库。
- 用户数据、真实素材和密钥不得因通用 `.gitignore` 规则被误删或意外提交；项目应明确哪些内容允许进入 Git。
- 清理脚本只能删除明确归属于当前构建或任务的文件，不得无差别删除整个工作目录。
- 移动现有文件前，检查相对路径、MSBuild、脚本、CI、资源加载和运行时路径，并同步更新引用。
