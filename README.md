# TipCL 插件市场

TipCL 启动器的官方插件市场索引。在启动器 **插件页 → 设置** 里填入下面的索引地址即可浏览和安装插件：

```
https://raw.githubusercontent.com/wfwwq/TipCL-market/main/index.json
```

也可以在插件市场里直接填本仓库 `index.json` 的地址，或把插件 zip 拖进插件页单独安装。

## 收录插件

| 插件 | 版本 | 说明 |
|---|---|---|
| [hello-plugin](https://github.com/wfwwq/TipCL/tree/main/samples/hello-plugin) | 0.1.0 | 官方示例：演示页面、AI 标签、浮层、首页卡片、凭据、预设、工具、启动钩子等全部能力 |
| [dsh-game](https://github.com/luckui/dsh-game) | 0.1.3 | 大肥鱼玩 Minecraft：AI 以鲸鱼娘身份自己进服游玩，砍树、合成、挖矿、盖房、种地、去下界，边玩边说 |

## 提交你的插件

1. 插件按官方示例组织：文件夹根部放 `tipcl-plugin.json` 清单，打成 zip（清单在 zip 根部，或单一顶层文件夹内）
2. 在本仓库发 Release，tag 用 `plugin-<id>-<version>`，附件传 zip
3. 计算 zip 的 SHA-256，向本仓库 `index.json` 提 PR，在 `plugins` 数组里加一项：

```json
{
  "id": "my-plugin",
  "name": "我的插件",
  "version": "1.0.0",
  "author": "你的名字",
  "description": "一句话说明",
  "homepage": "https://github.com/你/仓库",
  "icon": "https://raw.githubusercontent.com/wfwwq/TipCL-market/main/icons/my-plugin.png",
  "verified": false,
  "download": {
    "url": "https://github.com/wfwwq/TipCL-market/releases/download/plugin-my-plugin-1.0.0/my-plugin-1.0.0.zip",
    "sha256": "zip 文件的 sha256，64 位十六进制",
    "size": 12345
  }
}
```

4. `verified` 由仓库维护者审核后改为 `true`。未审核的插件需要用户在启动器设置里打开「允许安装未审核插件」才能装

## 索引格式（schema 1）

- `schema`：必须是 `1`
- `name`：市场名称，展示用
- `plugins[]`：
  - `id`：必填，`[a-z0-9][a-z0-9-_.]{1,63}`，全局唯一
  - `version`：必填，semver
  - `download.url` / `download.sha256`：必填，sha256 必须是 64 位十六进制；安装时会校验，不符拒绝安装
  - `download.size`：可选，字节数
  - `verified`：可选，是否已审核
  - `icon`：可选，市场列表里显示的图标地址（http(s) URL 或 data URI，建议 64×64 以上 PNG/JPG）；可把图标文件放到本仓库 `icons/` 目录，用 raw 地址引用
  - 其余字段（`name` / `author` / `description` / `homepage` 等）透传给启动器展示

启动器支持同时填多个市场索引，按 `plugins[].id` 先到先得合并。

## 插件开发

见主仓库 [docs/plugins/DESIGN.md](https://github.com/wfwwq/TipCL/blob/main/docs/plugins/DESIGN.md)（接口契约）和 [samples/](https://github.com/wfwwq/TipCL/tree/main/samples)（示例与宿主测试）。
