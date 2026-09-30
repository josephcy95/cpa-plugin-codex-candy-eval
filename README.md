# Codex 降智测试（CLIProxyAPI 插件）

在 [CLIProxyAPI](https://github.com/router-for-me/CLIProxyAPI) 管理面板中进行**糖果测试**、**模型指纹测试**和 **ModelTrace 模型归因**。

![example](docs/images/example.png)

## 安装

### CPA 版本要求

最低要求 CPA [v7.3.3](https://github.com/router-for-me/CLIProxyAPI/releases/tag/v7.3.3)。

### 插件商店

CPA 管理面板（CPAMC 或 CPAMP）插件商店搜索安装 `cpa-codex-candy-eval`，并在面板左侧打开「Codex 降智测试」。

### 安装 Joseph Fork（无需等待 upstream）

此 fork 的商店来源：

```text
https://raw.githubusercontent.com/josephcy95/cpa-plugin-codex-candy-eval/main/registry.json
```

在 CPA 的 `config.yaml` 已有 `plugins` 节点中添加来源（保留其他配置）：

```yaml
plugins:
  enabled: true
  store-sources:
    - "https://raw.githubusercontent.com/josephcy95/cpa-plugin-codex-candy-eval/main/registry.json"
```

重启 CPA，刷新插件商店，搜索 **Codex 降智测试 · Joseph Fork**，安装
`0.3.5` 或更新版本。CPA 从本仓库的 Release 下载当前平台的 ZIP 并校验
`checksums.txt`；不需要手动复制或备份 `.so`。安装后按面板提示重启 CPA。
默认官方来源仍然保留。

如果已经通过官方来源安装同 ID 的插件，CPA 会限制跨来源更新。要保留
现有插件配置和历史，可在 `plugins.configs.cpa-codex-candy-eval.store`
中仅修改以下来源字段，保留已有 `version`、`release-tag` 和其他字段：

```yaml
source-id: "source-5fbea05af2fb"
source-url: "https://raw.githubusercontent.com/josephcy95/cpa-plugin-codex-candy-eval/main/registry.json"
repository: "https://github.com/josephcy95/cpa-plugin-codex-candy-eval"
```

这一步表示主动选择信任此 fork 的来源；不会移动或删除插件库。重启 CPA
后，在 Joseph Fork 条目点击更新，商店会保存新版本的安装记录。历史继续使用相同插件 ID，保存在原来的
`plugins/cpa-codex-candy-eval-state.json` 文件中。
若 CPA 或管理面板不支持 `store-sources`，需先升级到支持自定义来源的版本。

### 人工安装

进入 CPA 工作目录，执行对应的安装命令。macOS 和 Linux：

```sh
curl -fsSL https://raw.githubusercontent.com/josephcy95/cpa-plugin-codex-candy-eval/main/install.sh | sh
```

Windows 请先停止 CPA，再在 PowerShell 中运行：

```powershell
irm https://raw.githubusercontent.com/josephcy95/cpa-plugin-codex-candy-eval/main/install.ps1 | iex
```

也可以从 [Releases](https://github.com/josephcy95/cpa-plugin-codex-candy-eval/releases/latest) 下载对应平台的压缩包，解压后将插件文件重命名为 `cpa-codex-candy-eval-v<版本号>.<扩展名>`，放入对应的 `plugins/<系统>/<架构>/` 目录。

在 CPA 的 `config.yaml` 中启用插件：

```yaml
plugins:
  enabled: true
  dir: "plugins"
  configs:
    cpa-codex-candy-eval:
      enabled: true
```

## 使用

在「凭证」标题右侧选择类型，默认「认证文件 · codex」，也可选择「全部凭证」。点击凭证旁的按钮单独测试，或勾选多个凭证批量测试。未勾选时，批量操作针对当前类型下的全部可用凭证。

凭证列表默认将 Pro（含 Pro 5x）排在最前，其次是 Plus，Team 和其他凭证居中，
Free 排在最后。Pro 标签为紫色，Plus 标签为蓝色，支持明暗主题。糖果测试
按钮旁可直接启用或停用认证文件凭证；正在测试时账户操作暂不可用。

### 糖果测试

选择模型、推理强度和单凭证次数，点击「测试」。推理强度选择 `none` 时由 CPA 决定。

正确答案为 **21**。展开凭证可查看历次回答，正确率按当前选择的模型和推理强度统计。

### 指纹测试

选择模型和采集模式，点击「采集」。结果会提示与所选模型是否一致；疑似替换时会显示更接近的模型。展开凭证可查看历史记录和详情。

| 模式 | 每个凭证的请求数 | 适用情况 |
| --- | ---: | --- |
| 快速 | **60 次** | 初步检查 |
| 标准 | **200 次** | 日常测试 |
| 严格 | **400 次** | 结果不明确时复测 |

结果不明确或不稳定时，可使用严格模式复测。

### ModelTrace

选择模型，点击凭证旁的「测试」，或勾选凭证批量测试。插件自动生成三条数字挑战，与内置候选模型比对，显示最接近的模型、家族及归因概率。展开凭证可查看历史记录和详情。

### 记录与额度

- 测试会消耗凭证额度。
- 每个凭证保留最近 20 次糖果测试、5 组指纹测试和 5 组 ModelTrace 测试，刷新或重启后仍可查看。

## 致谢

- [codex-candy-eval](https://github.com/haowang02/codex-candy-eval)
- [ModelTrace](https://github.com/xqy2006/ModelTrace) — 数字指纹归因方法与候选库（MIT）
- [LINUX DO](https://linux.do/) - 新的理想型社区
