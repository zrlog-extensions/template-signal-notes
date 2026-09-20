# Signal Notes 工作约定

- 这是独立主题仓库，根目录直接放 `template.properties` 和 FTL。
- 修改前阅读 `docs/design.md`，公共接口以 templates 的规范和所锁定运行时为准。
- 保留已有布局与交互；只因兼容性或明确设计要求调整视觉。
- 验证：`../templates/bin/theme check .`、修改 JS 后 `node --check js/theme-toggle.js`、`bash bin/package.sh`，然后用公共预览环境验证目录与 ZIP。
- CI 使用 `toolkit.lock.json` 固定公共工具提交；更新该锁时重新跑目录和 ZIP 的冒烟检查。
- 不提交 `dist/`、本地运行数据库或下载的运行时。不要把主题复制回索引仓库。
