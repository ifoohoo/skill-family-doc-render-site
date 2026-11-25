# Skill Family 文档工具公开站点

本目录对应公开仓库 `ifoohoo/skill-family-doc-render-site` 的站点内容与渲染产物，集中提供 `skill-family-docs` 五宿主五技能插件与 `skill-family-doc-render` 渲染包的上手指南、六大原生组件示例、日常增量更新、发版防泄漏检查与配置参考。浏览站点无需安装任何命令行工具。

在线访问地址：<https://ifoohoo.github.io/skill-family-doc-render-site/>

## 本地浏览与最小检查

直接在浏览器中打开 `rendered/index.html`（公开仓库根下为 `index.html`）即可阅读。如需模拟 GitHub Pages 的仓库子路径部署行为，可在公开目录上一级启动只读静态服务：

```sh
python3 -m http.server 8000
```

页面与 `modern` 模板资源全部采用相对路径并附带内容 SHA-256 指纹（`?v=<hash8>`），不依赖构建打包器、运行时 CDN 或外部字体。维护者准备处理发布或核查门禁边界时，可先在源工作区运行只读帮助命令确认当前 `release-skill` 参数：

```sh
npx release-skill@0.9.32 help --json
```

<!-- release-skill:capability:safe-first-command -->
> **安全起点：** `npx release-skill@0.9.32 help --json` 仅打印命令行帮助，不触发构建或远端写入。日常核验站点状态时，请在源工作区运行只读的 `npm run check:release-docs`。

<!-- release-skill:capability:external-write-boundary -->
> **外部写入边界：** 正式发布会更新 `ifoohoo/skill-family-doc-render-site` 的分支、标签与 GitHub Release，首次上线还会涉及 GitHub Pages 配置。创建远端仓库、批准计划、执行发布与变更 Pages 设置均须取得当次任务的明确授权。

## 维护与联合发布流程

站点正文由源工作区 `docs/public/site/pages.json` 与同名 `<id>.md` 生成，完整 HTML、样式表、搜索脚本和图标均来自精确版本的 `modern` 模板。公开仓库只接收 `rendered/` 产物、渲染基线、本说明与许可证；发现文字或链接问题时，须回到源工作区修改 Markdown 并重新渲染，切勿直接手改生成的 HTML。

本工作区默认使用同一份生产发布计划联合交付 `skill-family-doc-render`、`skill-family-docs` 与 `workspace-site` 三个单元：

1. **冻结前准备**：在源工作区通过 `skill-family-docs-build-site` 核实代码事实并经 `agent-method-registry` 解析 `humanize-rewrite`（默认采用 `shuorenhua`）完成外部视角撰写与整篇冷读，刷新覆盖快照并重新渲染站点，对齐三个发布单元的版本源（当前本地源版本见 `docs/public/release.json`，实际线上可用版本以公开仓库与官方 Release 为准），确保完整 `npm run verify` 通过。
2. **批准前隔离复验**：`prepare` 冻结 tarball、站点快照与 Git 制品后，在独立临时目录安装冻结的渲染器 tarball，对源工作区执行只读 `--check-project`；一旦出现输入漂移、摘要失配或检查失败，立即停止，不批准该发布计划。
3. **发布与恢复**：批准后按同一计划执行发布；若遇部分成功，先通过 `release-skill reconcile` 恢复，再运行 `verify` 校验 npm registry 摘要与精确版本安装结果。

## 常见故障诊断

- **发布准备门禁失败（`GATE_FAILED`）**：查看 `check:release-docs` 报告的具体阶段（制品版本锁、公开配置同步、覆盖状态、渲染基线、内部链接、多视口版面溢出或过时限制词），按提示回到源工作区修复后重试。
- **发布中断处于 `PARTIAL` 状态**：先使用空 npm 缓存运行 `release-skill reconcile` 按既有证据恢复，再执行 `verify`，切勿强推覆盖远端分支或标签。
- **本地预览样式缺失**：确认 `index.html` 与 `assets/theme.css`、`assets/site.js`、`assets/search-index.json`、`assets/mark.svg` 位于同一生成目录内，并使用只读 HTTP 服务访问，避免使用会向正式产物注入随机属性的预览插桩工具。

## 许可证

Apache-2.0，详见 [LICENSE](LICENSE)。
