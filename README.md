# Skill Family 文档工具站点

这个仓库保存 `skill-family-docs` 插件与 `skill-family-doc-render` npm 包的统一公开文档站。浏览网站不需要安装软件；页面介绍上手、写作、渲染、发布、技能入口、命令配置、排错和升级。

网站入口：<https://ifoohoo.github.io/skill-family-doc-render-site/>

## 最小示例

直接打开 `index.html` 即可浏览。需要模拟 GitHub Pages 的项目子路径时，在仓库上级目录运行：

```sh
python3 -m http.server 8000
```

再从浏览器进入本仓库目录。页面和资源使用相对链接，不需要构建工具或外部字体。

需要维护站点时，先在私有源工作区运行只读帮助命令，确认当前 release-skill 的参数和边界：

```sh
npx release-skill@0.9.14 help --json
```

## 维护和发布

页面由私有工作区中的源文件生成。公开仓库只接收渲染产物、基线、本说明和许可证。发现内容错误时，应回到源工作区修改并重新渲染，不直接编辑公开产物。

<!-- release-skill:capability:safe-first-command -->
> **安全起点：** `npx release-skill@0.9.14 help --json` 只获取并显示帮助，不发布站点。需要处理站点时，先在源工作区运行只读评估和渲染检查。

<!-- release-skill:capability:external-write-boundary -->
> **外部写入边界：** 发布会更新 `ifoohoo/skill-family-doc-render-site` 的分支、标签和 GitHub Release；首次上线还会修改 GitHub Pages 设置。创建仓库、批准、发布和启用 Pages 前都需要当前任务的明确授权。

最小维护流程是：修改源页面，重新渲染，检查基线，只选择 `workspace-site` 准备发布，审查并批准冻结计划，然后发布、验证并核对 Pages。

## 故障排查

准备失败时，先检查源工作区中的渲染配置、`.release-skill/project.yaml`、页面源和渲染基线。页面打不开时，先确认 `index.html` 与 `assets/style.css` 位于同一个发布快照内，再用本地 HTTP 服务复现 GitHub Pages 的访问方式。不要在公开仓库手工修补生成文件。

## 许可

Apache-2.0，见 [LICENSE](LICENSE)。
