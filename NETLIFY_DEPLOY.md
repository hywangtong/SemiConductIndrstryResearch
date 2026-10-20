# SemiConductIndrstryResearch — Netlify 部署说明

## 当前问题

`master` 分支根目录只发现一个 `.html` 文件，但不存在 Netlify 站点根路径默认使用的 `index.html`。已有站点地址当前返回 404。

## 最小变更

将本修复包的 `netlify.toml` 放进仓库 **根目录**（和原来的 `.html` 文件同一层级），提交到 **master**。**保留原始 HTML 文件，不要改名、删除或重新编码它的文件名**。

`netlify.toml` 会让 Netlify：

1. 在仓库根目录执行 `mkdir -p dist && cp ./*.html dist/index.html`；
2. 将已有的 HTML 文件复制为 `dist/index.html`；
3. 以 `dist` 作为发布目录，因而能从 `/` 访问页面。

该项目目前没有识别到 `package.json`，所以无需设置 npm 或 Node.js 构建环境，也无需启动后端服务。

## 在 GitHub 网页端操作

1. 打开仓库 `hywangtong/SemiConductIndrstryResearch` 的 `master` 分支。
2. 点击 **Add file → Create new file**，文件名输入 `netlify.toml`。
3. 粘贴本修复包 `netlify.toml` 的全部内容，点击 **Commit changes**，提交到 `master`。
4. 打开已关联的 Netlify 站点，进入 **Deploys**，检查新部署。若没有自动触发，使用 **Trigger deploy**。
5. 部署完成后访问 `https://unique-cranachan-66302c.netlify.app/` 验证页面。

## 构建设置预期值

- Production branch: `master`
- Base directory: `.`（仓库根目录）
- Build command: `mkdir -p dist && cp ./*.html dist/index.html`
- Publish directory: `dist`

`netlify.toml` 的对应配置通常优先于 Netlify UI 中冲突的构建设置。如果构建日志不是上述命令，请检查构建日志及站点实际关联的分支。

## 本地验证

在仓库根目录运行：

```sh
mkdir -p dist && cp ./*.html dist/index.html
ls -lh dist/index.html
```

如果仓库将来增加了多个 `.html` 文件，则应把构建命令中的 `./*.html` 换成明确的首页源文件名，避免歧义。

## 边界与验证

这份修复只针对已经确认的入口文件和发布目录问题，不会修改或删除原有网页源代码。尚未取得 GitHub 写入授权、Netlify 管理权限，也无法从此运行环境下载原始 HTML，因此这里不能宣称已将修改推送至仓库、或已完成在线部署。原始 HTML 内部是否引用无法访问的资源，需要部署后进一步检查。
