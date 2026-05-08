# wufuc

[English Version](./README.md)

> **注意**：这是 [wufuc](https://github.com/zeffy/wufuc) 的社区维护分支，原始项目已不可访问。

修复 Windows Update 错误 **0x80240037**（"不支持的硬件"），使搭载 Intel Kaby Lake、AMD Ryzen 等不受支持处理器的 Windows 7 和 8.1 系统可以继续安装更新。

## 下载

从 [Releases](https://github.com/achillis2016/wufuc/releases/latest) 下载最新版本，解压后以管理员身份运行 `install_wufuc.bat`。

**免确认安装**：
```
install_wufuc.bat /UNATTENDED /NORESTART
```

详细用法参见 `src\wufuc_setup_bat\使用说明.txt`。

## 捐赠 :heart:

[**点击此处查看捐赠方式！**](./DONATE.md)

## 背景

Windows 更新 KB4012218 和 KB4012219 的发布说明中包含以下内容：

> 当 PC 尝试通过 Windows Update 扫描或下载更新时，启用对处理器代次和硬件支持的检测。

这些更新标志着微软[政策变更](https://blogs.windows.com/windowsexperience/2016/01/15/windows-10-embracing-silicon-innovation/)的实施，微软声称将不再为搭载新一代 Intel、AMD 和 Qualcomm 处理器的 Windows 7 或 8.1 系统提供支持。

这对于任何决定不"升级"到 Windows 10 的用户来说，基本上就是一记响亮的耳光。尤其令人遗憾的是，Windows 7 和 8.1 的扩展支持期分别要到 2020 年 1 月 4 日和 2023 年 1 月 10 日才结束。

部分使用老旧 Intel 和 AMD 处理器的用户也受到了影响！原始项目收到了以下 CPU 被阻止接收更新的用户报告：

- Intel Atom Z530
- Intel Atom D525
- Intel Core i5-M 560
- Intel Core i5-4300M
- Intel Core i7-4930K
- Intel Pentium B940
- AMD FX-6300
- AMD FX-8350
- AMD Turion 64 Mobile Technology ML-34

## 微软真坑！

如果你感兴趣，可以在原始仓库的 `old-kb4012218-19` 分支中阅读关于发现 CPU 检测机制的原始分析文章。

简而言之，在名为 `wuaueng.dll` 的系统文件中，有两个负责 CPU 检测的函数：`IsDeviceServiceable(void)` 和 `IsCPUSupported(void)`。`IsDeviceServiceable` 实际上只调用一次 `IsCPUSupported`，然后在后续调用中复用其结果。

## 功能特性

- 使不受支持处理器的 PC 能够继续使用 Windows Update。
- 使用 C 语言编写，最好的编程语言。（译注：原作者观点 :sunglasses:）
- 完全自由（自由软件意义上的自由）软件。
- 不修改任何系统文件。
- 基于字节模式匹配的补丁技术，这意味着即使新的更新发布后，它通常仍能正常工作。
- 无外部依赖。

## 常见问题

参见 [FAQ.md](./FAQ.md)。

## 工作原理

以下是安装 wufuc 后的基本运行流程：

- 安装程序注册一个计划任务，在系统启动或用户登录时自动启动 wufuc。
- 根据 Windows Update 服务的配置方式，wufuc 会：
    * **共享进程**：注入到 Windows Update 启动时所在的共享服务宿主进程中。
    * **独立进程**：等待 Windows Update 服务启动，然后注入其中。
- 注入后，wufuc 会根据需要 Hook 以下函数：
    * `LoadLibraryExW` Hook 会在 `wuaueng.dll` 被加载时自动 Hook 其中的 `IsDeviceServiceable()` 函数。
    * `RegQueryValueExW` Hook 用于提供与 [UpdatePack7R2](https://github.com/achillis2016/wufuc/issues/100) 的兼容性。当 `wuauserv` 配置为独立进程运行时，此 Hook 不会应用。

## 赞助

### [Advanced Installer](https://www.advancedinstaller.com/)

安装包使用 Advanced Installer 制作，使用了其[开源许可证](http://www.advancedinstaller.com/free-license.html)。Advanced Installer 直观友好的用户界面使得原作者能够以最小的精力快速创建功能完善的安装程序。可以了解一下！

## 特别感谢

- Wen Jia Liu ([@wj32](https://github.com/wj32)) 提供了出色的程序 [Process Hacker](https://github.com/processhacker2/processhacker)，以及 [phnt 头文件](https://github.com/processhacker2/processhacker/tree/master/phnt)。
- Duncan Ogilvie ([@mrexodia](https://github.com/mrexodia)) 提供了 [x64dbg](https://github.com/x64dbg/x64dbg)、其中的 [`patternfind.cpp`](https://github.com/x64dbg/x64dbg/blob/development/src/dbg/patternfind.cpp) 算法，以及本项目借鉴的 issue 模板。
- Tsuda Kageyu ([@TsudaKageyu](https://github.com/TsudaKageyu)) 提供了出色的 [minhook](https://github.com/TsudaKageyu/minhook) 库。

[Latest]: https://github.com/achillis2016/wufuc/releases/latest
