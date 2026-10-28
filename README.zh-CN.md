# NAM FORGE · Zoom MultiStomp ZDL 工作台

[English](README.md) · [中文](README.zh-CN.md)

把 NAM（Neural Amp Modeler）捕捉批量转成 Zoom MultiStomp 的 `.ZDL` 效果（MS-50G / MS-60B / MS-70CDR），并给踏板做 128×64 单色封面。界面配色参考 Zoom G1on。

纯前端、离线运行：文件只在浏览器里处理，不上传。

![批量转换](preview-batch.png)
![封面设计](preview-design.png)
![导入图片](preview-import.png)


## 兼容

- **Zoom MultiStomp MS-50G / MS-60B / MS-70CDR**（原版；不支持 "+" 和 Four 系列）。
- **Zoom G1on**：只作为外观/配色参考（琥珀屏、蓝旋钮、灰黑面板）；G1on 本身不能加载这些 ZDL。
- **NAM / Neural Amp Modeler**：A2 Lite 捕捉（`.nam`，SlimmableContainer，3 声道 WaveNet）。
- **输出**：`.ZDL`，用 **Zoom Effect Manager 2.3.3+** 安装。

## 共享封面库

在 [NAM FORGE COVERS](https://zisonzishen0415-stack.github.io/nam-forge-covers/) 浏览/下载别人分享的封面：下载 design JSON 后，在 App 里用「封面设计 → 导入设计」导入。投稿走 GitHub Issue，审核后进入共享库。

**App 内直接分享**：封面设计 →「分享到共享库」。一次填入 fine-grained token（仅选 `nam-forge-covers`，Contents: Read and write），之后一键把 design JSON + 128×64 PNG 写进库并更新 `library.json`。勾选「记住 token」只存在本机浏览器；只在自己的电脑上这样做。

## 功能

- **批量转换**：一次拖入多个 `.nam`，自动识别 A2 Lite（SlimmableContainer 内的 3 声道 WaveNet），分配短名和槽位，导出 ZIP（`.ZDL` + 同名 `.png` + `.json`）。
- **封面库**：「封面设计」里可把编辑好的封面存成多份（本机 localStorage）；批量转换 / 换封面的表格里点击行内封面缩略图，就能为每个文件单独选封面（封面库、内置预设或当前编辑），也可以一键全部恢复为默认封面。
- **换封面**：拖入已有的 `.ZDL`，只改写 1,024 B 封面区和 12 B 显示名；权重、FXID、版本号原样保留（已做逐字节差异验证）。每个文件同样可单独选封面。
- **封面设计**：128×64 单色；默认「纯文字 · 浅底」+ 1 px 小字；预设、手绘、反相、清空。原版踏板默认不画名字，需要时勾选「显示名字」。
- **导入图片**：半实心 / 半网点混合（默认，网点量可调）、纯网点、实心、线条；缩放 25–400%、X/Y 移动，或选「移动」工具直接拖。图片按踏板的显示形状（LCD 像素纵向 1.4×）适配，不会在 ZDL 上被拉长变形。
- **所见即所得**：编辑器、踏板预览、缩略图、导出的 PNG 全部按踏板实际形状（1.4×）显示，不会出现「设计预览和实际生成宽度不同」。
- **文字**：可开关、小字 / 大字、X/Y 移动或「移字」拖动；底板 / 描边可选。
- **导入 / 导出**：封面导出 128×64 PNG；设计导出 / 导入 JSON（底图 + 文字位置 / 字号 / 开关），换电脑也能接着改。
- 引擎 **0.26**，**216** 个 capture 槽位，full rate 每块 **194 DSP**。

## 快速开始

1. 下载或克隆本仓库，用 Chrome / Edge 打开 `index.html`。
2. 不需要服务器、不联网；文件只在本地浏览器处理。
3. 也可以直接访问 GitHub Pages：<https://zisonzishen0415-stack.github.io/nam-forge/>

## 转换

1. 「批量转换」→ 拖入多个 `.nam`。
2. 起始槽位默认 **40**，可逐行改短名 / 槽位；界面里能改成 1–216 任意值。
3. 「转换全部」→「下载 ZIP」。
4. ZIP 里每个 capture 有 `NAM<槽位><短名>.ZDL`、同名 `.png`（Effect Manager 缩略图）和 `NAMLite-<短名>.json`（转换记录）。

## 换封面

1. 「换封面」→ 拖入已生成的 `.ZDL`（0.26 模板）。
2. 可改显示名；点每行封面图单独选封面，「应用封面」。
3. 只改动封面区和显示名，音色数据不动。

## 安装到踏板

- 用 **Zoom Effect Manager 2.3.3+** → Settings → Read Effects from folder → From Folder，指向解压后的目录。
- `.ZDL` 和同名 `.png` 必须放在一起；**不要改文件名**（踏板只认前 8 个字符，重名会死机）。
- 只适用于原版 MultiStomp（MS-50G / MS-60B / MS-70CDR）；不支持 "+" 系列和 Four 系列。
- 安装前备份踏板。一个 full-rate capture 占 194 DSP，多个同时开通常装不下。
- 如果踏板卡住：拔电，按住 Up + Down 插 USB 进升级模式，用 Effect Manager 删除最后安装的文件；最后手段 `All INITIALIZE` 会清空 patch。

## 技术说明

- 封面：128×64、1-bit、Zoom 列优先 RLE，容量 1,024 B。
- 名字：3×5 点阵，只支持 A–Z、0–9 和 `- + / . !`；文件名 `NAM<slot><name>` ≤ 8 字符。
- 引擎 0.26：full rate 194 DSP；48 kHz 源在 44.1 kHz 踏板上不做重采样。
- 所有转换在浏览器本地完成；没有服务器、没有上传。

## 参考与致谢

- **配色 / 硬件参考**：Zoom **G1on** —— 琥珀色 LCD、蓝色旋钮、灰黑面板。Zoom、G1on 是 Zoom Corporation 的商标；本项目与其无关联。
- **引擎与 ZDL / 封面格式**：[themanro/ZoomMultistompZDL](https://github.com/themanro/ZoomMultistompZDL) —— NAMLite 0.26、模板、槽位 identity、封面编码。该 fork 的原创部分为 MIT；上游 [repeat98/ZoomMultistompZDL](https://github.com/repeat98/ZoomMultistompZDL) 另见其说明。
- **NAM 格式**：[NeuralAmpModelerCore](https://github.com/sdatkinson/NeuralAmpModelerCore)（Steven Atkinson，MIT）。
- **字体**：[JetBrains Mono](https://github.com/JetBrains/JetBrainsMono)（SIL Open Font License 1.1），已嵌入页面；许可见 `FONT-LICENSE.txt`。
- **安装工具**：Zoom Effect Manager（第三方，不随本项目分发）。

第三方声明与完整 MIT / OFL 文本见 `NOTICE.txt`、`FONT-LICENSE.txt`。

**关键词：** Zoom MultiStomp、Zoom G1on、MS-50G（MS50G）、MS-60B（MS60B）、MS-70CDR（MS70CDR）、NAM、Neural Amp Modeler、NAM 捕捉、NAMLite、`.nam`、`.ZDL`、ZDL、Zoom Effect Manager、吉他、贝斯、综合效果器。

## 声明

- 本项目不是 Zoom Corporation 的官方产品，未获其背书或支持。
- 自定义效果文件可能导致踏板死机 / 无法启动；请先备份 patch，风险自负。
- 引擎模板与格式来自上述开源项目；NAM 捕捉由用户自行提供，本项目不分发任何 capture。
