# Skill Family 文档工具站点

这个仓库保存 `skill-family-docs` 插件与 `skill-family-doc-render` npm 包的统一公开文档站。浏览网站不需要安装软件；页面介绍上手、写作、渲染、发布、技能入口、命令配置、排错和升级。

网站入口：<https://ifoohoo.github.io/skill-family-doc-render-site/>

## 最小示例

直接打开 `index.html` 即可浏览。需要模拟 GitHub Pages 的项目子路径时，在公开仓库上级目录运行：

```sh
python3 -m http.server 8000
```

再从浏览器进入公开仓库目录。页面和模板资源都使用相对链接，不需要构建工具或外部字体。

需要维护站点时，先在私有源工作区运行只读帮助命令，确认当前 release-skill 的参数和边界：

```sh
npx release-skill@0.9.14 help --json
```

## 维护和发布

页面由私有工作区中的 `docs/public/site/*.md` 和 `pages.json` 生成。完整 HTML、主题 CSS、浏览器脚本和图标来自固定版本的 `editorial` 模板；项目不单独维护主题 CSS 或 JavaScript。公开仓库只接收渲染产物、基线、本说明和许可证。发现内容错误时，应回到 Markdown 源修改并重新渲染，不直接编辑公开产物。

<!-- release-skill:capability:safe-first-command -->
> **安全起点：** `npx release-skill@0.9.14 help --json` 只获取并显示帮助，不发布站点。需要处理站点时，先在源工作区运行只读评估和渲染检查。

<!-- release-skill:capability:external-write-boundary -->
> **外部写入边界：** 发布会更新 `ifoohoo/skill-family-doc-render-site` 的分支、标签和 GitHub Release；首次上线还会修改 GitHub Pages 设置。创建仓库、批准、发布和启用 Pages 前都需要当前任务的明确授权。

三个发布单元使用同一份生产计划。prepare 前，用目标版本的本地候选完成事实核对、style-guard 审校、覆盖刷新和站点渲染，并对齐渲染器、文档插件与站点的版本真源。这里的 `skill-family-doc-render-workspace` 是渲染配置中的仓库名。

prepare 冻结 npm tarball、站点快照和 Git 制品。计划批准前，从计划读取冻结 tarball，在隔离目录安装，再用该安装包对本站执行完整的只读 `--check-project`。工作区输入漂移、制品摘要不匹配或检查失败时停止，不批准该计划。

批准后按同一计划发布。部分成功时先用 release-skill 的 `reconcile` 恢复，再运行 `verify`，核对 npm registry 的 `dist.integrity` 并完成精确版本安装检查。

## 故障排查

准备失败时，先检查源工作区中的渲染配置、`.release-skill/project.yaml`、Markdown 页面源、覆盖记录和渲染基线。

页面打不开时，先确认 `index.html`、`assets/editorial/style.css`、`assets/editorial/site.js` 和 `assets/editorial/mark.svg` 位于同一个发布快照内，再用本地 HTTP 服务复现 GitHub Pages 的访问方式。不要在公开仓库手工修补生成文件。

## 许可

Apache-2.0，见 [LICENSE](LICENSE)。
