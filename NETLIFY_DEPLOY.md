# 半导体工业全流程调研报告 — Netlify 部署说明

## 网站结构

- `index.html`：目录首页，包含各章和章内小节的链接。
- `chapter-01.html` 至 `chapter-12.html`：12 章正文。
- `conclusion.html`：最终结论。

所有页面均为独立的静态 HTML，使用相对链接，保留内嵌样式和图表。可以直接双击 `index.html` 离线阅读，无需安装 npm 依赖或启动后端。

## Netlify 配置

仓库根目录的 `netlify.toml` 将全部根目录 HTML 文件复制到 `dist/`，保留各自的文件名：

- Base directory：`.`
- Build command：`mkdir -p dist && cp ./*.html dist/`
- Publish directory：`dist`

关联仓库部署后，`/` 打开目录首页，`/chapter-01.html` 等路径打开对应正文。发布时需包含全部 HTML 文件；只发布 `index.html` 会造成章节链接无法访问。

## 本地构建与验证

在仓库根目录运行：

```sh
mkdir -p dist && cp ./*.html dist/
```

Windows PowerShell 对应命令：

```powershell
New-Item -ItemType Directory -Force dist
Copy-Item .\*.html .\dist\
```

确认 `dist/index.html`、12 个章节页及 `dist/conclusion.html` 均存在。打开 `dist/index.html`，检查目录链接、小节定位、返回目录、上一节/下一节和返回顶部。窄屏下检查表格横向滚动，并检查打印预览。

`dist/` 为生成目录，不提交到 Git。修改源文件后重新构建；新增章节时同步更新首页目录、各子页导航和翻页链接。

## 国产设备与厂家专项

`semiconductor-domestic-alternatives.html` 为离线独立报告，首页和直驱/丝杠专项均有入口。它读取的原目录为 `semiconductor-direct-drive-inventory.csv`，筛选任一直驱或丝杠等级含完整 A、B、P 符号的条目，包含复合等级。

维护调研数据后，在仓库根目录运行 `python build-domestic-alternatives.py`（仅使用 Python 标准库），生成报告和四份 UTF-8 BOM CSV：

- `semiconductor-domestic-alternatives.csv`：245 个原条目及国产候选对应。
- `semiconductor-domestic-products.csv`：设备/系列、生产主体、运动证据和替代边界。
- `semiconductor-domestic-manufacturers.csv`：厂家及待核主体索引。
- `semiconductor-domestic-sources.csv`：参考资料与适用范围。

当前报告覆盖37个设备家族、146条产品/系列及历史或专利线索、88个厂家及待核主体。79个主体按公开资料列为境内制造主体，其余单列；这些数量不表示所有产品均达到原机工艺要求，也不表示已穷尽国内全部供应商。

Netlify 仍只复制根目录 HTML。网页内嵌全部 CSV 导出数据，线上和离线下载均不依赖独立 CSV 文件或网络接口。生成后检查245行覆盖、型号和来源引用、筛选/重置/CSV、窄屏横向滚动与打印；再按上面的命令复制 HTML 到 `dist/`。无需安装前端依赖。
