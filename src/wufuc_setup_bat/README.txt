wufuc Setup Script Usage
==========================

wufuc disables the "Unsupported Hardware" message in Windows Update on Windows 7
and 8.1, allowing PCs with Intel Kaby Lake, AMD Ryzen, or other unsupported
processors to continue receiving updates.

Requirements
------------
  Windows 7 (x64/x86) or Windows Server 2008 R2
  Windows 8.1 (x64/x86) or Windows Server 2012 R2

Must be run as Administrator.

Usage
-----

1. Interactive install (default)
   Right-click install_wufuc.bat and select "Run as administrator".
   Follow the on-screen prompts.

2. Silent install (no prompts)
   From an administrator command prompt:
     install_wufuc.bat /UNATTENDED

   Installs without any confirmation and restarts automatically when done.
   To skip the restart, add /NORESTART:
     install_wufuc.bat /UNATTENDED /NORESTART

3. Silent uninstall
     install_wufuc.bat /UNINSTALL /UNATTENDED
   Or simply:
     uninstall_wufuc.bat /UNATTENDED

   Skip restart:
     uninstall_wufuc.bat /UNATTENDED /NORESTART

Parameters
----------
  /UNINSTALL    Enter uninstall mode
  /UNATTENDED   Skip all confirmation prompts (silent mode)
  /NORESTART    Do not prompt for restart after completion

Common Scenarios
----------------
  Scenario                            Command
  ────────────────────────────────────────────────────────────
  Install, auto reboot               install_wufuc.bat /UNATTENDED
  Install, no reboot                  install_wufuc.bat /UNATTENDED /NORESTART
  Uninstall, auto reboot              uninstall_wufuc.bat /UNATTENDED
  Uninstall, no reboot                uninstall_wufuc.bat /UNATTENDED /NORESTART

Notes
-----
  - A reboot is recommended after install/uninstall for changes to take full
    effect.
  - If a previous wufuc 0.1~0.5 installation modified wuaueng.dll, the script
    will restore it via SFC automatically. No manual action is needed.
  - Do not run directly from a ZIP archive. Extract all files to a permanent
    location first (e.g. C:\Program Files\wufuc).
