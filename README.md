# Signal Notes

独立维护的 ZrLog FreeMarker 主题。保留瀑布流文章列表、封面、分类、作者头像和 Light / Dark / System 三种模式。

## 开发

将公共工具仓库与本仓库放在同一级目录。需要 JDK 17+，公共工具使用 Maven Wrapper 构建；不需要 Node 构建或外部数据库。

```bash
git clone https://github.com/zrlog-extensions/templates.git
git clone https://github.com/zrlog-extensions/template-signal-notes.git
cd templates
git checkout 95ae88c1d12d576a07fba054cd566002b5e66c42
./mvnw verify
cd ../template-signal-notes
../templates/bin/theme check .
../templates/bin/theme preview . --port 7080
```

首次预览下载经 SHA-256 校验的 ZrLog 3.9.2。访问 http://127.0.0.1:7080；修改 FTL/CSS/JS 后刷新。详情、分类、搜索、分页和空态使用公共固定内容。主题根目录就是安装包根目录。

- [主题设计与维护约定](docs/design.md)
- [迁移来源](docs/migration.md)
- [公共规范](https://github.com/zrlog-extensions/templates/blob/master/docs/theme-contract.md)
- [完整验收流程](https://github.com/zrlog-extensions/templates/blob/master/docs/development.md)

## 配置与空态预览

```bash
../templates/bin/theme preview . --settings examples/preview-settings.json
../templates/bin/theme preview . --empty
../templates/bin/theme preview . --comments empty
```

预览评论为固定内容片段，用于验证评论容器与空结果，不代表真实评论插件的提交功能。

## 打包与安装

```bash
bash bin/package.sh
../templates/bin/theme preview dist/template-signal-notes.zip
zrlogctl theme upload dist/template-signal-notes.zip
```

`bin/package.sh` 使用相邻的 `templates`，也可设置 `ZRLOG_TEMPLATES_DIR`。输出 ZIP 和 SHA-256 文件，源码、文档、CI 和本地配置不进入安装包。上传不会自动启用主题；可在后台主题中心切换。覆盖已有主题需显式使用 `--overwrite`。

主题版本由 `template.properties` 和本仓库 Git tag / Release 管理。发布配置在 `theme.json`，CI 调用 templates 的公共构建上传 workflow；不在本仓库复制上传实现。

公共发布器把 ZIP、SHA-256 和 theme.json 一起上传到 GitHub Release。首次发布后，templates 索引可只读取 Release 中的 theme.json，为 zrlog-www 提供主题市场数据。首个独立版本发布前，latestRelease 保持 null。

```bash
../templates/bin/theme publish . --tag v1.0 --output-dir dist --dry-run
```

试点阶段公共工具需检出 `toolkit.lock.json` 指定的提交，该提交可能尚未合入 templates 主分支。
