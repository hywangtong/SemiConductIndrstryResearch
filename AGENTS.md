# Repository Guidelines

## Project Structure & Module Organization

This repository publishes a Chinese-language semiconductor manufacturing research report as a static website.

- `index.html`: complete report, chapter navigation, tables, process diagrams, and embedded CSS, including responsive and print styles.
- `netlify.toml`: deployment configuration; builds into `dist/` and publishes that directory.
- `NETLIFY_DEPLOY.md`: deployment notes. Some instructions describe the earlier HTML filename; use the current `index.html` and `netlify.toml` as the source of truth.

There are no separate source, asset, or test directories, JavaScript runtime dependencies, or backend services. Keep the report readable offline.

## Build, Test, and Development Commands

Run commands from the repository root:

- `Start-Process .\index.html` (PowerShell): open the report in the default browser for local preview.
- `mkdir -p dist && cp ./*.html dist/index.html` (POSIX shell): reproduce the configured Netlify build.
- `New-Item -ItemType Directory -Force dist; Copy-Item .\index.html .\dist\index.html` (PowerShell): produce the equivalent deployment output on Windows.
- `git diff --check`: check changes for whitespace errors before committing.

No npm installation or compilation is required. The Netlify wildcard assumes exactly one root HTML file; update the build command if adding others. Keep generated `dist/` output out of commits.

## 本机预览

这是纯静态网站，无需安装依赖或编译。可选择以下任一方式预览：

### 直接用浏览器打开

在文件资源管理器中进入 `C:\Code\半导体工业流程调研`，双击 `index.html`；也可以在项目根目录的 PowerShell 中运行：

```powershell
Start-Process .\index.html
```

### 启动本地 HTTP 服务

需要已安装 Python。在 PowerShell 中运行：

```powershell
cd "C:\Code\半导体工业流程调研"
python -m http.server 8000 --bind 127.0.0.1
```

然后在浏览器中访问 <http://127.0.0.1:8000>。

- 预览期间保持终端运行。
- 修改文件并保存后，刷新浏览器即可查看更新。
- 在终端按 `Ctrl+C` 停止服务。
- 若端口 `8000` 被占用，可改用 `8001` 等空闲端口，并同步修改访问地址。

## Coding Style & Naming Conventions

Preserve UTF-8 encoding, `lang="zh-CN"`, and Chinese report prose. Match existing HTML layout and compact CSS; avoid reformatting the entire document. No formatter, linter, or universal indentation rule is configured. Use two spaces for TOML settings.

Use descriptive kebab-case classes and preserve anchor patterns such as `chapter-04` and `chapter-04-sub-01`. Update navigation links alongside heading IDs. Retain semantic headings, table headers, keyboard access, and accessible labels.

## Testing Guidelines

No automated framework or coverage threshold exists. Before submitting, open the page and check chapter links, return-to-top navigation, narrow-screen table scrolling, and print preview. Confirm Chinese text renders correctly and `dist/index.html` exists after building. For content changes, verify terminology and supporting references.

## Commit & Pull Request Guidelines

Existing commits (`origin`, `for netlify`) establish no formal convention. Write short, descriptive imperative subjects, for example `Fix mobile table overflow`.

Keep pull requests focused. Describe the affected chapters or behavior, explain the reason, and list manual checks performed. Link relevant issues when available; include desktop/mobile screenshots for layout changes and print-preview evidence for print-style changes.
