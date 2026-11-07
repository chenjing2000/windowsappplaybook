# Windows 应用设计与交付手册

适用于个人维护的 Windows 本地应用。常规 Windows 桌面 GUI 默认优先采用 C# / WPF；默认发布方向为 Windows x64、Framework-dependent、Single-file，并优先采用便携式目录部署。项目可以根据实际技术约束采用合理例外。

## 按任务阅读

| 任务 | 阅读入口 |
|---|---|
| 新建应用、技术选型或设计重要功能 | [设计框架](design-framework.md) |
| 组织源码、测试、文档、工具和发布目录 | [项目结构规范](project-structure.md) |
| 设计窗口、弹窗和控件 | [Windows 11 UI 指南](ui-guidelines.md) |
| 记录项目已确认的约定 | [项目契约卡模板](project-contract-template.md) |
| 检查运行环境和发布产物 | [运行形态与验证](runtime-profiles.md) |
| 复用文件保存、任务取消、发布和数据保护模式 | [可复用模式](patterns.md) |
| 进行代码审查和交付前检查 | [Windows 应用审查 Skill](docs/skills/windows-app-review/SKILL.md) |

接入项目时，在仓库根目录 `AGENTS.md` 中说明本项目的规范入口、契约卡、权威规格、构建命令、验证入口及授权边界。无需将整套手册复制到每个项目。

## 规范优先级与冲突处理

1. 用户当前明确指示及本轮授权。
2. 用户已确认的项目行为、数据格式和技术决定。
3. 项目根目录 `AGENTS.md` 与项目契约卡中的专属约定。
4. 本手册中的通用规则和默认值。
5. LLM 的一般工程建议。

高优先级决定与低优先级规则冲突时，不得静默选择。应指出冲突、影响和建议；在用户已明确选择并授权实施时，按该决定执行。项目旧约定不能自动覆盖用户当前明确的新决定。

## 规则分级

- **必守**：正确性、数据安全、授权边界和交付可靠性要求。
- **默认**：需求未另行约定时采用的首选方案。
- **按需**：仅在项目具有相应能力或场景时应用。
- **例外**：允许有充分工程理由的替代方案；涉及重大技术、数据或分发取舍时先由用户决定。

## 规则职责

- `design-framework.md` 定义通用设计原则、默认技术路线、授权与验收边界。
- `project-structure.md` 定义仓库目录和文件归属。
- `ui-guidelines.md` 定义 Windows 11 风格的界面与交互要求。
- `patterns.md` 提供可复用的实现模式。
- `runtime-profiles.md` 定义运行形态及验证分层。
- `project-contract-template.md` 记录单个项目实际采用的约定和入口。
- `docs/skills/` 存放可执行的审查工作流程。

同一规则只在一个文件中作权威定义。其他文件可以摘要或链接，不应维护相互独立的重复版本。

## 已接入项目

- 日历与课表 · [ClassTimetable](../ClassTimetable/docs/project-contract.md) · [OneSchedule](../MySchedule/docs/project-contract.md)
- 英语学习与工具 · [EnglishAudioPlayer](../audioplayer/docs/project-contract.md) · [ReadArticles](../ReadArticles/docs/project-contract.md) · [ReciteWords](../ReciteWords/docs/project-contract.md) · [MdictViewer](../MdictViewer/docs/project-contract.md)
- Web 界面及内容导出 · [EnglishNotes](../EnglishNotes/docs/project-contract.md) · [Transcript Studio](../TranscriptStudio/docs/project-contract.md)
- 其他 C# 程序与诊断示例 · [CSharp 子项目契约](../CSharp/project-contracts.md)

以上项目链接是手册使用者仓库中的示例入口，不要求这些外部项目随本手册一起存在。按项目实际入口选择[运行与验证形态](runtime-profiles.md)。
