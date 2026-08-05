# Thank Helper

Thank Helper 是一个面向 **Dalamud API 15** 的 FF14 坦克职业静默自动打断助手。它会扫描已进入战斗的敌人读条，无需选中或切换目标，并在动作实际可用时优先使用插言，或对已确认可眩晕的读条使用下踢。

## 通过 Dalamud 自定义仓库安装

1. 打开 Dalamud 的“插件安装器”。
2. 打开插件安装器的“设置”，找到“自定义插件仓库”（Custom Plugin Repositories）。
3. 添加并保存以下精确 URL：

   ```text
   https://gh.atmoomen.top/raw.githubusercontent.com/Dulul818/DululFFXIVTools/main/pluginmaster.json
   ```

4. 返回插件安装器，搜索 **Thank Helper** 并安装。

## 主要功能

- 支持骑士（PLD）、战士（WAR）、暗黑骑士（DRK）和绝枪战士（GNB）。
- PvP 中强制停止。
- 优先使用插言；插言不可执行时才考虑已确认的下踢。
- 可配置 0～1500 毫秒打断延迟和成功提示。
- 支持本地下踢学习、手动同步策展学习数据以及分享码导入导出。
- 未知读条的下踢试探默认关闭，Boss 不会参与未知试探。

## 命令

```text
/thankhelper
/thankhelper interrupt
/thankhelper probe
```

## 风险提示

请先在低风险 PvE 场景验证，并保持未知下踢试探关闭，直到你确认当前游戏和 Dalamud 版本的行为符合预期。

## 公开产物

本仓库只包含可安装插件包、仓库索引、图标和策展学习数据，不包含私有源代码。
