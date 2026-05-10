# AGENTS.MD

本文件适用于整个 `slay-the-hs` 仓库。所有自动化助手、编码代理和协作者在本项目中工作时都应遵守这里的约定。

## 本机环境约束

- 如果需要安装新软件，安装目录必须放在 `E:\basicsoft\develop`。
- 禁止批量删除文件或目录。
- 如果确实需要删除文件，只能一次删除一个明确路径的文件。
- 如果任务看起来需要批量删除，应停止操作并请求用户手动删除。
- 不要执行 `git reset --hard`、批量清理、强制覆盖等会丢失用户改动的操作，除非用户明确要求。

## 项目概览

- 这是一个 Godot 4.5 C# 项目，主场景配置在 `project.godot` 中。
- Godot 项目名为 `slay the hs`，主场景是 `Scenes/MainMenu.tscn`。
- C# 解决方案是 `slay the hs.sln`。
- 主要游戏逻辑位于 `Scripts/`。
- 数据文件位于 `Data/`。
- 测试项目位于 `Tests/CombatLogicTests/`。
- AI 训练、rollout、评估和 agent 工具位于 `Tools/`。
- 训练和运行说明优先参考：
  - `Docs/TrainingGuide.md`
  - `Tools/python/README.md`
  - `Tools/AGENT_CONTROL.md`

## 常用命令

在仓库根目录执行：

```powershell
powershell -ExecutionPolicy Bypass -File .\build.ps1
```

运行战斗逻辑测试：

```powershell
dotnet run --project .\Tests\CombatLogicTests\CombatLogicTests.csproj
```

运行 rollout：

```powershell
powershell -ExecutionPolicy Bypass -File .\Tools\run-rollout.ps1
```

训练：

```powershell
powershell -ExecutionPolicy Bypass -File .\Tools\run-training-stage1.ps1 -Replay
```

评估：

```powershell
powershell -ExecutionPolicy Bypass -File .\Tools\run-eval.ps1 -Policy tabular
```

## 开发注意事项

- 修改 Godot 场景、资源、`.csproj` 或 `.sln` 前，先检查现有格式和 Godot 生成内容，尽量保持最小改动。
- 不要随意删除或重建 Godot 自动生成的 `.uid` 文件。
- 修改 `Data/` 下 JSON 数据时，保持 UTF-8 编码，并优先使用项目已有脚本或结构化 JSON 读写方式。
- 训练、评估、bot 控制前，通常需要先启动 Godot 游戏；Python 脚本会连接本地 bridge `127.0.0.1:47077`。
- 如果新 PowerShell 中 `python --version` 不可用，可设置：

```powershell
$env:SLAY_THE_HS_PYTHON = 'C:\Path\To\python.exe'
```

## 代码风格

- C# 代码遵循仓库现有命名、目录和类组织方式。
- 优先扩展已有系统，例如 `Scripts/Systems/`、`Scripts/Data/`、`Scripts/UI/`，不要轻易引入新的架构层。
- 对共享战斗逻辑、卡牌效果、敌人意图、牌库流转等改动，应补充或更新 `Tests/CombatLogicTests/` 中的测试。
- 保持改动聚焦；不要在功能修改中混入无关格式化、重命名或大范围重构。

## Git 工作流

- 开始修改前先查看 `git status --short --branch`。
- 不要回滚用户已有改动。
- 提交前尽量运行相关构建或测试命令，并在回复中说明执行结果。
- 如果无法运行某个验证步骤，应明确说明原因。
