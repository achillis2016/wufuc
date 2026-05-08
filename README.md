# wufuc

[中文版本](./README_CN.md)

> **Note**: This is a community-maintained fork of [wufuc](https://github.com/zeffy/wufuc). The original project is no longer accessible.

Fixes Windows Update error **0x80240037** ("Unsupported Hardware"), allowing you to continue installing updates on Windows 7 and 8.1 systems with Intel Kaby Lake, AMD Ryzen, or other unsupported processors.

## Downloads

Download the latest version from [Releases](https://github.com/achillis2016/wufuc/releases/latest), extract, and run `install_wufuc.bat` as Administrator.

**Silent install**:
```
install_wufuc.bat /UNATTENDED /NORESTART
```

See `src\wufuc_setup_bat\README.txt` for detailed usage.

## Donate :heart:

[**Click here for donation options!**](./DONATE.md)

## Background

The release notes for Windows updates KB4012218 and KB4012219 included the following:

> Enabled detection of processor generation and hardware support when PC tries to scan or download updates through Windows Update.

These updates marked the implementation of a [policy change](https://blogs.windows.com/windowsexperience/2016/01/15/windows-10-embracing-silicon-innovation/) they announced some time ago, where Microsoft stated that they would not be supporting Windows 7 or 8.1 on next-gen Intel, AMD and Qualcomm processors.

This is essentially a big middle finger to anyone who decides to not "upgrade" to Windows 10,
and it is especially unfortunate considering the extended support periods for Windows 7 and 8.1 won't be ending until January 4, 2020 and January 10, 2023 respectively.

Some people with older Intel and AMD processors are also affected! The original project received user reports of the following CPUs all being blocked from receiving updates:

- Intel Atom Z530
- Intel Atom D525
- Intel Core i5-M 560
- Intel Core i5-4300M
- Intel Core i7-4930K
- Intel Pentium B940
- AMD FX-6300
- AMD FX-8350
- AMD Turion 64 Mobile Technology ML-34

## Bad Microsoft!

If you are interested, you can read the original write-up on discovering the CPU check in the `old-kb4012218-19` branch of the original repository.

The tl;dr version is basically, inside a system file named `wuaueng.dll`, there are two functions responsible for the CPU check: `IsDeviceServiceable(void)` and `IsCPUSupported(void)`. 
`IsDeviceServiceable` simply calls `IsCPUSupported` once, and then re-uses the result that it receives on subsequent calls.

## Features

- Enables Windows Update on PCs with unsupported processors.
- Written in C, the best programming language. :sunglasses:
- Completely free (as in freedom) software.
- Does not modify any system files.
- Byte pattern-based patching, which means it will usually keep working even after new updates come out.
- No dependencies.

## Frequently Asked Questions

See [FAQ.md](./FAQ.md).

## How it works

This is a basic run-down of what wufuc does when you install it:

- The installer registers a scheduled task that automatically starts wufuc on system boot/user log on.
- Depending on how the Windows Update service is configured to run, wufuc will:
    * **Shared process**: inject itself into the service host process that Windows Update will run in when it starts.
    * **Own process**: wait for the Windows Update service to start and then inject into it.
- Once injected, wufuc will hook some functions where appropriate:
    * `LoadLibraryExW` hook will automatically hook the `IsDeviceServiceable()` function inside `wuaueng.dll` when it is loaded.
    * `RegQueryValueExW` hook is necessary to provide compatibility with [UpdatePack7R2](https://github.com/achillis2016/wufuc/issues/100). This hook not applied when `wuauserv` is configured to run in its own process.

## Sponsors

### [Advanced Installer](https://www.advancedinstaller.com/)

The installer packages are created with Advanced Installer using an [open source license](http://www.advancedinstaller.com/free-license.html). 
Advanced Installer's intuitive and friendly user interface allowed the original author to quickly create a feature complete installer with minimal effort. Check it out!

## Special thanks

- Wen Jia Liu ([@wj32](https://github.com/wj32)) for his awesome program [Process Hacker](https://github.com/processhacker2/processhacker), and also for his [phnt headers](https://github.com/processhacker2/processhacker/tree/master/phnt).
- Duncan Ogilvie ([@mrexodia](https://github.com/mrexodia)) for [x64dbg](https://github.com/x64dbg/x64dbg), its [`patternfind.cpp`](https://github.com/x64dbg/x64dbg/blob/development/src/dbg/patternfind.cpp) algorithm, and its issue template which was adapted for this project.
- Tsuda Kageyu ([@TsudaKageyu](https://github.com/TsudaKageyu)) for his excellent [minhook](https://github.com/TsudaKageyu/minhook) library.

[Latest]: https://github.com/achillis2016/wufuc/releases/latest
