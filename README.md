# Thank Helper

Thank Helper 是一个面向 **Dalamud API 15** 的 FF14 插件。当前版本提供坦克职业的静默自动打断：后台扫描已经进入战斗的敌人，不选择或切换目标，在动作实际可用时直接对对应怪物使用插言或下踢。

## 当前功能

- 仅支持骑士（PLD）、战士（WAR）、暗黑骑士（DRK）和绝枪战士（GNB）。
- PvP 中强制停止，不受设置开关影响。
- 优先使用插言；插言不可执行时才考虑已确认可用的下踢。
- 可选开启未知读条的下踢试探；Boss 不会参与未知试探。
- 超出范围、视线不通或技能暂不可用时不会放弃该读条，会持续检测到读条结束。
- 多目标同时读条时，先过滤当前可执行目标，再选择剩余时间最短的读条。
- 可配置 0～3000 毫秒延迟，默认 300 毫秒。
- 可选在确认打断成功后输出本地聊天提示。
- 下踢学习库支持筛选、删除、清空以及分享码导入/导出。

## 下踢学习与试探风险

默认关闭自动试探。开启后，插件会对普通怪物的未知读条尝试下踢：

- 一次确认成功后，记录为 `Confirmed`，以后自动放行。
- 三个不同怪物实例均明确失败后，记录为 `Rejected`。
- 超距、冷却、动作未成功发送、目标已有眩晕、目标死亡或消失等情况不会计为失败。
- Boss 只允许插言或使用已经确认的下踢记录，不试探未知组合。

下踢成功要求同时观察到本地玩家施加的眩晕以及对应读条提前结束。首版不 Hook 网络包，因此极端同时打断场景仍可能存在归因误差。

## 命令

```text
/thankhelper
/thankhelper interrupt
/thankhelper probe
```

- `/thankhelper`：打开设置窗口。
- `/thankhelper interrupt`：切换自动打断总开关。
- `/thankhelper probe`：切换自动试探开关。

## 分享码

分享码格式为 `THI1:` 开头的压缩文本。导出内容只包含 `Confirmed` 下踢白名单，不包含失败记录、开关、延迟、通知设置或插言黑名单。导入采用合并去重，不会覆盖本地已确认记录。

## 构建要求

- Windows
- .NET SDK 10
- Dalamud API 15
- PowerShell 7（`pwsh`）

如果本机 XIVLauncher 的 Dalamud 开发目录可被 SDK 自动发现，直接构建即可。否则把 `DALAMUD_HOME` 指向包含 `Dalamud.dll` 的 Dalamud 开发目录：

```powershell
$ErrorActionPreference = 'Stop'
$env:DALAMUD_HOME = 'C:\path\to\DalamudDev'
dotnet build '.\ThankHelper\ThankHelper.csproj' -c Release
if ($LASTEXITCODE -ne 0) { throw 'build failed' }
```

构建产物位于 `ThankHelper\bin\Release\`，至少应包含：

- `ThankHelper.dll`
- `ThankHelper.json`
- `ThankHelper.deps.json`

## 本地安装

1. 在 Dalamud 插件设置中启用开发插件支持。
2. 将 `ThankHelper\bin\Release\ThankHelper.dll` 添加到 Dev Plugin Locations。
3. 在开发插件列表中加载 `Thank Helper`。

本仓库当前不包含公开自定义仓库索引。若要通过自定义仓库分发，需要另行托管插件压缩包和仓库清单；插件本体无需为此耦合发布逻辑。

## 验证

运行纯逻辑检查：

```powershell
$ErrorActionPreference = 'Stop'
dotnet run --project '.\ThankHelper.Checks\ThankHelper.Checks.csproj'
if ($LASTEXITCODE -ne 0) { throw 'checks failed' }
```

当前实现已完成纯决策、学习状态机、分享码边界、命令解析、结果观察检查，并通过 .NET 10 / Dalamud API 15 编译验证。

**尚未声称完成 FF14 实机验证。** 自动动作、距离/视线判定、状态读取和游戏版本兼容性仍需在实际 XIVLauncher/Dalamud 环境中测试。建议先在木人或低风险 PvE 场景验证，并保持自动试探关闭。
