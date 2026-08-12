# HtmlToExe 网页打包器

> 将在线网址或本地 HTML 文件夹打包成独立 EXE 桌面程序的轻量级工具，基于 WebView2 技术渲染网页，无需浏览器即可运行。

---

## 开发者信息

**开发者：** Journeyer

**交流群组：**

| 群组名称 | 群号 |
|---------|------|
| 科技之星1群 | 669812887 |
| 科技之星总群 | 561116458 |
| 新人报道群 | 1032931117 |

---

## 开发语言与核心技术

### 开发语言

| 组件 | 语言 | 编译器 |
|------|------|--------|
| 打包器（packager.c） | **C 语言** | MinGW GCC |
| 模板程序（template.cpp） | **C++** | MinGW G++ |
| 授权生成器 | Python（仅生成授权文件，不参与编译） | Python 3.x |

**核心语言：C / C++**

### 技术栈

- **WebView2** (v1.0.4129.50) — 微软 Edge 内核网页渲染引擎，用于在桌面程序中嵌入现代网页
- **Windows Win32 API** — 原生窗口创建、消息循环、对话框、系统托盘等
- **Miniz** — 轻量级 ZIP 压缩/解压库，用于打包本地文件夹资源
- **WinHTTP** — Windows 原生 HTTP 客户端，用于抓取在线网页标题和图标
- **RichEdit 控件** — 用于格式化显示操作说明和关于信息
- **静态链接** — 全部使用 `-static -static-libstdc++ -static-libgcc -lwinpthread` 编译，无外部 DLL 依赖

### 构建工具链

```
编译器：MinGW-w64 (GCC/G++)
资源编译：windres
链接方式：静态链接（消除 libstdc++-6.dll、libgcc_s_seh-1.dll 依赖）
```

---

## 功能特性

### 打包器功能

- **双模式打包**：支持在线网址和本地 HTML 文件夹两种打包模式
- **自动抓取**：在线模式下自动抓取网页标题和 favicon 图标
- **自定义配置**：应用名称、窗口尺寸、滚动文字、图标文件均可自定义
- **窗口行为选项**：最大化、最小化、置顶、系统托盘、窗口居中
- **系统托盘**：打包完成后自动最小化到托盘，右键菜单支持显示主窗口、关于说明、操作手册、退出程序
- **日志输出**：自动输出详细日志到 `packager_log.txt`，便于问题排查
- **授权管理**：基于 XOR 加密 + Base64 编码的授权文件验证机制

### 打包后程序功能

- **完全自包含**：WebView2Loader.dll 内嵌于 EXE 中，运行时自动释放，无需随程序分发任何 DLL
- **右键导航菜单**：后退 / 前进 / 刷新 / 复制 / 全选
- **键盘快捷键**：F5 刷新、Alt+← 后退、Alt+→ 前进、Ctrl+C 复制、Ctrl+A 全选
- **程序内导航**：所有链接在程序内打开（包括 target=_blank），不跳转外部浏览器
- **标题栏滚动文字**：支持自定义标题栏滚动文字效果
- **登录状态持久化**：WebView2 用户数据目录重定向至 `%APPDATA%\<应用名称>\WebView2Data`，关闭重开后保留登录状态
- **WebView2 运行时检测**：启动时自动检测 WebView2 Runtime 是否安装，未安装时提示用户下载
- **系统托盘模式**：勾选托盘选项后，点击关闭按钮或 Alt+F4 时隐藏到托盘而非退出
- **临时文件清理**：程序退出时自动清理临时释放的 WebView2Loader.dll

---

## 系统要求

### 打包器运行环境

| 项目 | 要求 |
|------|------|
| 操作系统 | Windows 7 SP1 或更高版本 |
| 架构 | x86 / x64 |
| WebView2 Runtime | 需已安装（Windows 10/11 通常已预装） |
| 授权文件 | `license.lic` 需与打包器在同一目录下 |

### 打包后程序运行环境

| 项目 | 要求 |
|------|------|
| 操作系统 | Windows 7 SP1 或更高版本 |
| 架构 | x86 / x64 |
| WebView2 Runtime | **必须安装**（不可或缺） |
| 附加 DLL | 无需任何额外 DLL |

### WebView2 Runtime 下载

- **官方下载地址**：https://developer.microsoft.com/microsoft-edge/webview2/
- Windows 10/11 系统通常已预装，无需额外操作
- Windows 7/8 需手动下载安装

---

## 使用说明

### 快速开始

1. **获取打包器**：将 `分发\构建程序\` 目录下的 `HtmlToExe打包器.exe` 和 `license.lic` 放在同一目录
2. **运行打包器**：双击运行 `HtmlToExe打包器.exe`
3. **选择打包模式**：
   - 选择「在线网址」— 输入网页地址（如 `https://example.com`），程序自动抓取标题和图标
   - 选择「本地文件夹」— 选择包含 `index.html` 的本地文件夹，程序将整个文件夹打包
4. **填写应用信息**：
   - 应用名称：生成的 EXE 文件名（必填）
   - 窗口尺寸：宽×高，最小 320×240，默认 1024×768
   - 滚动文字：标题栏滚动文字（可选，留空不滚动）
   - 图标文件：程序图标，建议 ICO 格式（可选，留空自动检测）
5. **选择窗口功能**：按需勾选 最大化 / 最小化 / 置顶 / 托盘 / 居中
6. **开始打包**：点击「开始打包」按钮，选择保存位置，等待打包完成
7. **获取结果**：打包完成后的 EXE 保存在 `分发\打包输出\` 目录

### 界面按钮说明

| 按钮 | 功能 |
|------|------|
| 开始打包 | 执行打包操作 |
| 取消打包 | 取消当前打包任务（不退出程序） |
| 操作说明 | 查看详细使用说明和必备环境 |
| 退出程序 | 退出打包器 |

### 授权说明

- 打包器需配合 `license.lic` 授权文件使用，授权文件需与 EXE 放在同一目录
- 授权文件采用 XOR 加密 + Base64 编码算法生成，包含有效期信息
- 启动时自动验证授权，界面标题栏显示授权到期时间
- 如未激活或授权过期，请联系开发者或加群获取：561116458

### 分发说明

打包后的 EXE 文件可直接分发给用户，无需附加任何 DLL 或文件。但目标电脑必须安装 WebView2 Runtime，建议随程序附上 WebView2 Runtime 安装包或在说明中提醒用户安装。

---

## 关于杀毒软件误报

### 误报原因

本软件可能被部分杀毒软件（如 360、火绒等）误报为 `Backdoor/CobaltStrike.jo` 等威胁，这属于**误报**，原因如下：

1. **资源内嵌机制**：打包器将模板 EXE 作为资源嵌入自身，运行时释放并使用 UpdateResourceW 写入资源，这种"EXE 中嵌入 EXE"的模式与某些恶意软件的行为特征相似
2. **静态链接**：为消除 DLL 依赖，程序使用全静态链接编译，生成的 EXE 体积较大且包含完整的运行时代码，容易被启发式扫描误判
3. **文件释放行为**：程序运行时会从内嵌资源释放 WebView2Loader.dll 到临时目录，这种行为模式被部分杀毒软件视为可疑
4. **自包含设计**：打包后的 EXE 完全自包含（内嵌网页资源、图标、DLL），这种高度自包含的特性容易触发启发式检测

### 解决方案

- **添加信任/白名单**：将 `HtmlToExe打包器.exe` 和打包后的程序添加到杀毒软件的信任列表或白名单中
- **临时关闭杀毒**：打包时可临时关闭杀毒软件实时防护，打包完成后再开启
- **使用 Windows Defender**：Windows 自带 Defender 通常不会误报本程序
- **数字签名**：如需彻底解决误报问题，可对程序进行数字代码签名认证（需自费购买代码签名证书）

> **声明**：本软件不含任何恶意代码、后门或远程控制功能。CobaltStrike 相关报障纯属启发式误报，程序行为完全透明可控。

---

## 目录结构

```
HtmlToExe-C/
├── packager.c              # 打包器主程序源码（C 语言）
├── packager.rc             # 打包器资源文件（对话框、图标定义）
├── template.cpp            # 模板程序源码（C++，打包后程序的基础）
├── template.rc             # 模板程序资源文件
├── resource.h              # 资源头文件（控件 ID 定义）
├── icon_util.h             # 图标工具头文件
├── app.ico                 # 应用图标
├── license.lic             # 授权文件
├── miniz.c / miniz.h       # Miniz 压缩库源码
├── miniz_tdef.c            # Miniz 压缩实现
├── miniz_tinfl.c           # Miniz 解压实现
├── miniz_zip.c             # Miniz ZIP 实现
├── 构建命令.txt             # 完整编译构建命令
├── packager_log.txt        # 打包器运行日志
├── microsoft.web.webview2.1.0.4129.50/  # WebView2 SDK
├── webview2/               # WebView2 库文件
└── 分发/                    # 分发目录
    ├── 构建程序/
    │   ├── HtmlToExe打包器.exe   # 打包器可执行文件
    │   └── license.lic           # 授权文件
    ├── web/                       # 网页版打包工具
    │   └── HtmlToExe-Web.html     # 在线版打包器（单文件，可部署到静态平台）
    ├── 打包输出/                  # 打包后的程序输出目录
    └── 使用说明.txt
```

---

## 构建方法

### 环境准备

- 安装 MinGW-w64 工具链
- 确保 WebView2 SDK 位于项目目录下
- 将 MinGW\bin 加入系统 PATH

### 编译步骤

详细编译命令见 `构建命令.txt`，主要步骤如下：

```bash
# 1. 编译 Miniz 库
gcc -c miniz.c -o miniz.o
gcc -c miniz_tdef.c -o miniz_tdef.o
gcc -c miniz_tinfl.c -o miniz_tinfl.o
gcc -c miniz_zip.c -o miniz_zip.o

# 2. 编译模板 EXE（静态链接）
g++ -c template.cpp -o template.o -I"." -I"microsoft.web.webview2.1.0.4129.50/build/native/include" -DUNICODE -D_UNICODE
windres template.rc -o template_res.o
g++ template.o template_res.o miniz.o miniz_tdef.o miniz_tinfl.o miniz_zip.o -o template.exe \
    -lole32 -loleaut32 -lshell32 -luser32 -ladvapi32 -luuid -mwindows -municode \
    -static -static-libstdc++ -static-libgcc -lwinpthread

# 3. 编译打包器资源（将 template.exe 嵌入）
windres packager.rc -o packager_res.o

# 4. 编译打包器（静态链接）
gcc packager.c miniz.o miniz_tdef.o miniz_tinfl.o miniz_zip.o packager_res.o -o packager.exe \
    -mwindows -municode -lwinhttp -lcomctl32 -luser32 -lgdi32 -lshell32 -lole32 -luuid \
    -static -static-libgcc
```

### 授权文件生成

使用 Python 生成授权文件（仅生成授权，不参与程序编译）：

```python
import base64

_ENCRYPT_KEY = "Journeyer+kejizhixing2019"

def _xor_encrypt(data, key):
    key_bytes = key.encode()
    return bytes(data[i] ^ key_bytes[i % len(key_bytes)] for i in range(len(data)))

def encrypt_license_date(date_str):
    plain = date_str.encode()
    encrypted = _xor_encrypt(plain, _ENCRYPT_KEY)
    return base64.b64encode(encrypted).decode()

# 生成有效期到 2026-12-31 的授权
license_content = encrypt_license_date("2026-12-31")
with open("license.lic", "w", encoding="utf-8") as f:
    f.write(license_content)
```

---

## 在线网页版

本项目同时提供在线网页版打包工具，与桌面版功能一致，托管于静态平台，方便用户无需下载即可使用。

### 在线版特性

- **纯前端单文件应用**：单个 HTML 文件，无需服务器后端，可直接部署到任意静态托管平台
- **双模式支持**：在线网址配置 + 本地文件夹打包（拖拽上传，自动检测 index.html）
- **完整配置项**：应用名称、窗口尺寸、滚动文字、图标上传预览、窗口功能选项（最大化/最小化/置顶/托盘/居中）
- **配置生成与下载**：
  - 在线模式：生成 JSON 配置文件下载
  - 本地模式：使用 JSZip 将所有文件 + 配置打包为 ZIP 下载
- **与桌面版 UI 一致**：青色 #20B2AA 主题、Microsoft YaHei 字体、扁平化设计
- **部署平台**：GitHub Pages / Gitee Pages / Cloudflare Pages / Vercel 等任意静态托管平台

### 在线版使用

**在线访问地址**：https://kejizhixing.github.io/htmltoexe-web/

1. 访问在线版页面（已部署至 GitHub Pages）
2. 选择打包模式（在线网址 / 本地文件夹）
3. 填写应用信息（名称、窗口尺寸、滚动文字、图标）
4. 勾选窗口功能选项（最大化 / 最小化 / 置顶 / 托盘 / 居中）
5. 点击「开始打包」按钮
6. 下载生成的配置文件（在线模式为 JSON，本地模式为 ZIP 资源包）
7. 将下载的配置文件/资源包交给桌面版打包器，完成最终 EXE 打包

### 部署方法

将 `分发/web/HtmlToExe-Web.html` 文件上传至任意静态托管平台即可：

- **GitHub Pages**：将文件放入仓库根目录，启用 GitHub Pages
- **Gitee Pages**：将文件上传至 Gitee 仓库，开启 Gitee Pages 服务
- **Cloudflare Pages / Vercel / Netlify**：直接部署单个 HTML 文件
- **本地使用**：直接在浏览器中打开该 HTML 文件即可使用

> 在线版生成配置文件和资源包，最终 EXE 打包仍需桌面版打包器完成（因 EXE 资源写入需本地编译环境）。

---

## 常见问题

**Q: 程序启动后白屏？**
A: 请检查网络连接（在线模式）或确认 index.html 路径正确（本地模式）。

**Q: 程序无法启动或闪退？**
A: 请确认目标电脑已安装 WebView2 Runtime。

**Q: 打包后的程序关闭后登录状态丢失？**
A: WebView2 用户数据目录已重定向至 `%APPDATA%\<应用名称>\WebView2Data`，正常情况下登录状态会保留。如仍丢失，请检查该目录是否有写入权限。

**Q: 图标显示不正确？**
A: 请使用标准 ICO 格式图标文件，建议包含多种尺寸（16/32/48/256）。

**Q: 标题栏没有滚动文字？**
A: 请在「滚动文字」输入框中填写内容，留空则不滚动。

**Q: 杀毒软件报毒怎么办？**
A: 本软件为误报，请添加至杀毒软件白名单。详见上方「关于杀毒软件误报」章节。

**Q: 找不到 libstdc++-6.dll 或 libgcc_s_seh-1.dll？**
A: 本程序已使用静态链接编译，正常情况下不会出现此问题。如使用自行编译版本，请确保添加 `-static -static-libstdc++ -static-libgcc` 编译参数。

**Q: 授权过期或未激活？**
A: 请联系开发者或加入交流群 561116458 获取新的授权文件。

---

## 软件许可与版权声明

1. **版权归属**：本软件完整著作权归开发者 Journeyer 所有，受《中华人民共和国著作权法》及国际相关版权条约保护。
2. **禁止篡改行为**：严禁对程序进行逆向工程、反编译、篡改版权标识、修改程序内核逻辑后二次分发发布。
3. **合法使用要求**：使用者必须遵守国家各项法律法规，若存在违规使用行为，全部法律责任由用户本人独立承担。
4. **免责说明**：软件以现状形式提供，不附带任何明示或隐含使用担保；因使用软件产生的一切财产、数据损失，开发者不承担任何赔偿责任。

**请遵守法律法规，尊重知识产权**

---

*Copyright (c) 2026 Journeyer. All rights reserved.*
