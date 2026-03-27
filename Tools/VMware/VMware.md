# VMware

## Installation on Windows
(27.03.2026)
- Turn off Windows features (Windows > Turn Windows features on or off)
  - Virtual Machine Platform
  - Windows Hypervisor Platform
  - Windows Subsystem for Linux
  - Windows Sandbox (if exist)
  - Hyper-V (if exist)
- Turn off memory integrity
  - Windows > Windows Security > Device security > Core isolation
  - Uncheck Memory integrity.
- CPU Virtualization
  - Intel: Intel Virtualization Technology, or Intel VT-x.
  - To check, go to Task Manager > Performance > CPU > Virtualization: Enabled
  - If disabled, go to BIOS setting to enable.