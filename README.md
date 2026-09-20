# Signal Notes

ZrLog FreeMarker 主题，提供瀑布流文章列表、封面、分类、作者头像，以及 Light / Dark / System 三种外观模式。主题作者署名为 OpenAI。

本仓库维护主题源码、设计约定、配置和版本发布。[templates](https://github.com/zrlog-extensions/templates) 提供公共规范、Java 构建与上传工具、基准预览环境，以及供 zrlog-www 使用的主题市场索引。

## 开始开发

需要 Git、JDK 17+、Bash 和 curl。工具使用 Maven Wrapper，无需另装 Maven。首次构建和预览需要联网下载依赖与运行时；主题无需 Node 构建，修改 JavaScript 时使用 Node 检查语法。

在同一工作目录克隆两个仓库：

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

访问 http://127.0.0.1:7080 。预览使用经 SHA-256 校验的 ZrLog 3.9.2 和固定示例内容，无需外部数据库；修改 FTL/CSS/JS 后刷新页面，按 Ctrl+C 结束预览。

上面的工具提交与 [toolkit.lock.json](toolkit.lock.json)、CI 保持一致。升级工具时同步更新锁文件和 `.github/workflows/theme.yml` 的两个提交引用，并重新验收。

修改前阅读[主题设计与维护约定](docs/design.md)：保留瀑布流列表、详情页正文布局和统一明暗状态；详情页不重复摘要，不加入 Next.js / Vercel 品牌标识。公共数据接口见[主题规范](https://github.com/zrlog-extensions/templates/blob/95ae88c1d12d576a07fba054cd566002b5e66c42/docs/theme-contract.md)。

## 配置与空态预览

结束前一个预览后，按需运行其中一条：

```bash
../templates/bin/theme preview . --settings examples/preview-settings.json
../templates/bin/theme preview . --empty
../templates/bin/theme preview . --comments empty
```

预览评论为固定内容片段，用于验证评论容器与空结果，不代表真实评论插件的提交功能。配置定义和值变更后需重启预览。

## 构建与验收

以下命令均在主题仓库根目录执行：

```bash
# 修改此 JavaScript 文件时检查语法
node --check js/theme-toggle.js
bash bin/package.sh
bash ../templates/bin/verify-preview.sh . 17081 template-signal-notes
```

构建产物为 `dist/template-signal-notes.zip` 和 `.zip.sha256`。ZIP 根目录直接包含 `template.properties`，仅打包主题运行文件。`bin/package.sh` 默认使用相邻的 `templates`；其他位置可通过 `ZRLOG_TEMPLATES_DIR` 指定。

验收脚本分别验证源码目录和经 ZrLog 安装器安装的 ZIP，检查路由、页面及资源；结果保存在 `../templates/target/preview-check/template-signal-notes/`。浏览器中还需检查桌面与手机端、明暗切换及持久化、正文/代码/表格样式和横向溢出，详见[公共验收流程](https://github.com/zrlog-extensions/templates/blob/95ae88c1d12d576a07fba054cd566002b5e66c42/docs/development.md)和[已有浏览器验收记录](docs/browser-acceptance.json)。兼容基准为 ZrLog 3.9.2。

如已配置 zrlogctl，可运行 `zrlogctl theme upload dist/template-signal-notes.zip` 上传到自己的站点，再到后台主题中心启用；覆盖已有主题需显式使用 `--overwrite`。

## 发布与市场索引

普通 main push 和 PR 运行构建及目录/ZIP 验收；推送 `v<version>` tag 才会调用公共 Java 发布器，向 GitHub Release 上传 ZIP、SHA-256 和带下载信息的 `theme.json`。

准备好版本并提交后，以当前 `template.properties` 的 `version=1.0` 为例：

```bash
# 只生成发布文件并显示上传命令
../templates/bin/theme publish . --tag v1.0 --output-dir dist --dry-run
# 验收通过后，推送 tag 触发正式发布
git tag v1.0
git push origin v1.0
```

每次发布时，tag 去掉 `v` 后必须与 `template.properties` 的版本完全一致。当前尚无独立 Release，`latestRelease` 保持 `null`；dry-run 生成的配置不表示已经发布。

首次发布成功后，将 templates 中的配置地址切换为本仓库的 `releases/latest/download/theme.json`，或将发布信息更新到 main 的 `theme.json`。索引同步只获取该配置，不拉取源码或 ZIP。主题也可自行维护 Release，只需遵守[发布与市场配置契约](https://github.com/zrlog-extensions/templates/blob/95ae88c1d12d576a07fba054cd566002b5e66c42/docs/publication-and-market.md)。
