# 独立仓库迁移

来源：`zrlog-extensions/templates` 提交 `fd28a514fae4246b87b7dd38f45a7cc8f274e88d` 的 `template-signal-notes/`。

通过 `git subtree split --prefix=template-signal-notes` 提取历史，独立仓库的导入提交为 `f8f1b882340d250945de66ee3d8d9a79924a647f`。保留原主题作者和提交记录。

迁移保持主题 ID `template-signal-notes`、配置键和 localStorage 键 `signal-notes-theme` 不变。现有站点可以沿用配置，后续更新包从本仓库 Release 获取。

主题设计参考 Next.js Blog 的布局节奏，不包含其官方品牌图标；元信息 url 指向本主题仓库。
