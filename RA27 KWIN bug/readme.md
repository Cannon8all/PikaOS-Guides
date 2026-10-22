**A critical regression can occur in KWin (Wayland) where Plasma repeatedly crashes or fails to initialize correctly on systems using newer AMD graphics cards (such as the Radeon RX 9070).** 

This issue produces the following error messages in the system logs
* `amdgpu: [drm] *ERROR* Unsupported screen format RA24 little-endian`
* `kwin_wayland: Atomic modeset commit failed! Invalid argument`
* `kwin_wayland: Applying output configuration failed!`

## Temporary Workaround: Enable Software Cursor

Until a permanent upstream patch is applied to the system packages, you can force KWin to use a software-based mouse cursor instead of hardware-driven DRM rendering. This bypasses the faulty screen format initialization.

### Follow these steps to apply the workaround system-wide:

Open the global environment configuration file in a terminal:
  
>sudo nano /etc/environment


Manually add the following line to the bottom of the file:
   
>KWIN_FORCE_SW_CURSOR=1
---
* Save the file (`Ctrl+O`, `Enter`) and exit the editor (`Ctrl+X`).

* Reboot your system to apply the changes.
