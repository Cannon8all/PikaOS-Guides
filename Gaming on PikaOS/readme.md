# Gaming on PikaOS

PikaOS ships with the latest graphics drivers out of the box, so your GPU is ready for gaming from the first boot. To get everything else you need, install the `pika-gameutils-meta` package. It preinstalls a set of launchers and tools:

- **Launchers:** Steam, Heroic, Lutris, Faugus Launcher
- **Tools:** ProtonPlus, GOverlay, MangoHud, Gamescope

Install it from a terminal:

> `pikman i pika-gameutils-meta`

You can also install it from the **PikaOS Welcome** app.

---
<div style="margin-top: 50px;"></div>


# Which Proton version should I use?

Proton is the compatibility layer that lets Windows games run on Linux. Steam picks a version for each game by default, but some games run better on a different one. This section explains the official versions from Valve and the community builds, and helps you choose.

### Official Proton versions (Valve)

- **Proton 11.0:** The current stable series. It is a good default for most games and is updated automatically through Steam.
- **Proton Experimental:** Valve's bleeding-edge branch. New fixes land here first and are added to the stable releases later. Use it for new games, for games that don't work well on stable Proton, it can also break a game that worked before.
- **Proton 10.0 and older:** Previous stable series. Only pick one if a game works with it but not with newer versions.

To choose a version for one game in Steam, right-click the game, select **Properties → Compatibility**, tick **Force the use of a specific Steam Play compatibility tool**, and pick a version from the list.

### Community Proton builds

Community builds add extra features and fixes that Valve's Proton doesn't have yet. They are made by volunteers and are not supported by Valve.

You can install and update community builds with **ProtonPlus**, which comes with `pika-gameutils-meta`. It can set them up for Steam, Lutris and Heroic.

- **Proton-GE:** Made by GloriousEggroll and based on Valve's Proton, with many extra fixes. It is known for its video playback fixes, which help games with missing or broken cutscenes. It also includes many fixes for individual games and optional OptiScaler support. It is the most widely used community build, and a good first one to try when a game won't run on Valve's Proton.
- **Proton-CachyOS:** Made by the CachyOS team and based on Proton Experimental. It adds quality-of-life features, performance work and experimental changes, including low-latency versions of DXVK and VKD3D, FSR4 upgrading for AMD GPUs and winewayland improvements. It comes in different builds for different CPUs. Choose `x86_64_v3` if your CPU supports it, otherwise the plain `x86_64` build. If you can pick between an SLR (Steam Linux Runtime) build and a native one, we recommend the SLR build.
- **Proton-EM:** Made by Etaash-mathamsetty. It focuses on native Wayland support (winewayland), HDR and FSR4 upscaling, without the extras that Proton-CachyOS adds. Many of Proton-CachyOS's and Proton-GE's Wayland improvements originate here, so it's the one to follow if you want the latest Wayland development.


### Which one should I pick?

| If you... | Try |
| --- | --- |
| Just want your games to work | Proton 11.0 (or Steam's default) |
| Play a brand new game, or a game that doesn't run well | Proton Experimental, and check ProtonDB |
| Have a game that only works with an older version | Proton 10.0 or older |
| See missing videos or cutscenes, or need a game-specific fix | Proton-GE |
| Use an AMD GPU and want FSR4 upgrading and low-latency extras | Proton-CachyOS |
| Want native Wayland or HDR | Proton-EM |

---

# Proton Launch Commands

Launch commands (also called launch options) let you change how a game starts. You can switch Proton features on or off, show an FPS overlay, run the game inside Gamescope, and more. This page explains how to use them and lists the most useful ones.

---

## How to use launch commands

In Steam, right-click the game, select **Properties**, and type the command into the **Launch Options** box under the **General** tab.

### What is %command%?

`%command%` stands for the game's normal start command. Steam replaces it with the real command when you press Play. Everything you write around it changes how the game is started.

> `VARIABLE=value program %command% -game-argument`

- **Variables go before `%command%`.** They change how the game or Proton behaves, for example `PROTON_LOG=1`.
- **Programs go before `%command%`.** Tools like `mangohud` or `gamescope` start first and run the game inside them.
- **Game arguments go after `%command%`.** These are passed to the game itself, for example `-novid`.

You can combine as many as you like, separated by spaces:

> `PROTON_LOG=1 mangohud %command% -novid`

> If a game won't start after you change its launch options, clear the box and try one option at a time. A typo is the most common cause.
{.is-warning}

---

## General commands

These work on any GPU.

| Command | What it does |
| --- | --- |
| `PROTON_LOG=1 %command%` | Writes a Proton log to your home folder (see [Troubleshooting Proton](#troubleshooting-proton) below). |
| `mangohud %command%` | Shows an overlay with FPS, temperatures and usage (requires MangoHud). |
| `PROTON_ENABLE_WAYLAND=1 %command%` | Runs the game with the Wayland driver instead of XWayland. Experimental, and only available in custom Proton builds such as Proton-CachyOS and Proton-GE. |
| `PROTON_USE_WINED3D=1 %command%` | Uses OpenGL-based WineD3D instead of DXVK/VKD3D. Only for older games or GPUs without good Vulkan support. |
| `PROTON_NO_ESYNC=1 %command%` | Turns off esync. Try it if a game crashes or freezes. |
| `PROTON_NO_FSYNC=1 %command%` | Turns off fsync. Try it if a game crashes or freezes. |
| `VKD3D_CONFIG=dxr %command%` | Enables ray tracing in DirectX 12 games, if your GPU supports it. |

## NVIDIA commands

| Command | What it does |
| --- | --- |
| `PROTON_ENABLE_NVAPI=1 %command%` | Enables NVAPI, which games need for features like DLSS. |
| `PROTON_HIDE_NVIDIA_GPU=0 %command%` | Stops Proton from hiding your NVIDIA GPU from the game. Some games need this to detect the card. |
| `__GL_THREADED_OPTIMIZATIONS=1 %command%` | Moves some of the NVIDIA OpenGL driver's CPU work to a separate thread. Off by default because it can lower performance in some games. Only affects OpenGL games. |
| `__GL_MaxFramesAllowed=1 %command%` | Limits how many frames the NVIDIA driver queues ahead (2 by default), which can lower input latency.  |

### NVIDIA hybrid laptops

On a laptop with an integrated GPU and an NVIDIA GPU.

> `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only %command%`

## AMD commands

| Command | What it does |
| --- | --- |
| `mesa_glthread=true %command%` | Enables threaded OpenGL in Mesa, which can reduce CPU load in OpenGL games. Some games already have it on by default, and it can cause problems in a few. |
| `PROTON_FSR4_UPGRADE=1 %command%` | Upgrades FSR 3.1 games to FSR4 on supported AMD GPUs. Recent Proton-CachyOS builds do this automatically, so you only need it to request a specific FSR4 DLL version. Older builds and other custom Proton builds may still need it. |

### AMD hybrid laptops

To run a game on the dedicated AMD GPU instead of the integrated one:

> `DRI_PRIME=1 %command%`

---

# Gamescope

Gamescope runs the game inside its own small window or fullscreen session. It's useful for fixing resolution or scaling problems, capping the framerate, and upscaling.

> `gamescope -W 2560 -H 1440 -r 144 -f -- %command%`

The `--` separates Gamescope's options from the game. Everything before it is for Gamescope, and `%command%` comes after it.

### Common options

| Option | What it does |
| --- | --- |
| `-W` / `-H` | Output resolution (your screen). |
| `-w` / `-h` | The game's internal resolution. Set this lower than the output resolution to upscale. |
| `-r` | Limits the game's refresh rate (frames per second). |
| `-o` | Refresh rate limit while the game is not focused, for example `-o 30` to save power in the background. |
| `-f` | Starts fullscreen. |
| `-b` | Starts in a borderless window. |
| `-F fsr` | Upscales with FSR 1.0. Other filters include `nis`, `linear`, `nearest`, `pixel` and `sgsr`. |
| `-S integer` | Scales the game in whole-number steps for sharp pixels. Other modes include `fit`, `fill` and `stretch`. |
| `--sharpness 5` | Sets sharpness for FSR and NIS from 0 to 20. Lower values are sharper. |
| `--adaptive-sync` | Enables variable refresh rate (VRR) if your display supports it. |
| `-e` | Enables Steam integration, so the Steam overlay works inside Gamescope. |
| `--force-grab-cursor` | Always uses relative mouse mode, instead of switching depending on whether the cursor is visible. Use it if the cursor isn't captured properly, for example if it escapes the game window or limits camera movement. |
| `--mangoapp` | Shows the MangoHud overlay from inside Gamescope. |

> Use `--mangoapp` instead of `mangohud` when you run a game inside Gamescope. Using both shows two overlays.
{.is-warning}

### Examples

Play a game at 1080p on a 1440p screen and upscale it with FSR:

> `gamescope -w 1920 -h 1080 -W 2560 -H 1440 -r 144 -F fsr -f -- %command%`

Sharp integer scaling with Steam overlay support and VRR:

> `gamescope -W 2560 -H 1440 -S integer -e --adaptive-sync -f -- %command%`

Borderless window with an FPS overlay and the cursor kept in the game:

> `gamescope -W 2560 -H 1440 -b --force-grab-cursor --mangoapp -- %command%`

Cap the framerate at 60 FPS to reduce heat and noise:

> `gamescope -W 2560 -H 1440 -r 60 -f -- %command%`

<div style="margin-top: 50px;"></div>

# Steam Shader pre-caching

Games compile shaders (small programs for your GPU) while you play. The first time you meet a new effect, this can cause stutter or long loading times.

Steam can compile some shaders ahead of time. With Proton-CachyOS, Proton-GE or Proton-EM, it's better to turn this off. These builds already include the codecs needed for in-game videos, and a reasonably modern CPU and GPU can handle compiling shaders on the fly.

Steam's pre-caching also doesn't cover everything, and updating the Proton version can force all shaders to be compiled again. If you have time to spare, you can leave it on.

To turn it off, open **Steam → Settings → Downloads** and untick:

- Allow background processing of Vulkan shaders
- Enable Shader Pre-caching

> After turning off Steam's shader pre-caching, increase your maximum shader cache size (see below).
{.is-warning}


## Increasing the shader cache size

Compiled shaders are stored on your system and reused later. By default, the cache has a size limit (1 GB on AMD), and once it's full, the oldest shaders are dropped. Some large games have over 1 GB of shaders, so they can end up recompiling on every launch. Raising the limit avoids the repeated stutter and long load times.

**Open a terminal and create the config directory if it doesn't exist:**

> `mkdir -p ~/.config/environment.d`

**Create and open the file in nano:**

> `nano ~/.config/environment.d/gaming.conf`

**Paste the correct configline below.**

AMD:

> `MESA_SHADER_CACHE_MAX_SIZE=12G`

NVIDIA:

> `__GL_SHADER_DISK_CACHE_SIZE=12000000000`

Save the file with `Ctrl + O` and `Enter`, then exit nano with `Ctrl + X`.

Log out and back in for the change to apply.

---
<div style="margin-top: 50px;"></div>

# falcond

falcond is PikaOS's profile-based gaming performance daemon. It runs quietly in the background, notices when you start a game, and applies the right performance settings for that game. You can make your own profiles and tweak settings by hand for each title.

A selection of popular profiles is already included, like the Proton profile that activates for any game running in a Proton compatibility layer.

**Falcond-gui is the PikaOS app you can use to configure everything in a handy way.**

> Don't use `falcond` together with the `feral gamemode`. They will interfere with each other.
>
> Falcond does not have to be added to the game launch options.
{.is-danger}

### What it does

- **Automatic game detection:** Detects running games and applies optimized settings.
- **Per-game profiles:** Each game can have its own profile.
- **Performance mode:** Turns on system-wide performance optimizations while you play.
- **3D V-Cache management:** On AMD CPUs with 3D V-Cache (X3D), it manages the V-Cache settings for you.
- **SCX scheduler integration:** Picks the CPU scheduler (scx) that suits the specific game.
- **VRAM prioritization (dmem):** Uses the Linux dmem cgroup, a recent technology for managing GPU video memory (VRAM). The game or heavy app in the foreground gets priority for VRAM, so background programs like web browsers or the desktop compositor can't hold on to it and cause stutter. This is built into falcond.
- **Proton support:** Works with Steam Proton games, and a global profile gives good coverage for games without their own profile.
- **Device modes:** Profiles for desktops, handhelds, HTPCs and more.
- **Low overhead:** Uses very few system resources.

### What is dmem?

Your GPU has a limited amount of video memory (VRAM). Normally, background programs can use a chunk of it, which can lead to stutter when a game needs more. dmem lets the system give VRAM priority to the game you're playing.

---
<div style="margin-top: 50px;"></div>

# Troubleshooting Proton

### Capturing and sharing Proton logs

If a game crashes or misbehaves, a Proton log is the first thing anyone helping you will ask for.

When sharing logs, upload them to [paste.pika-os.com](https://paste.pika-os.com/) instead of pasting them directly into the chat. Drag and drop the log file onto the website, then share the link it gives you.

To turn on Proton logging for a game:

1. In Steam, right-click the game and select **Properties**.
2. Under **Launch Options**, enter:

> `PROTON_LOG=1 %command%`

The next time you start the game, Proton writes a log file to your home folder named `steam-[AppID].log`. For example, Counter-Strike 2 has the AppID 730, so its log is `steam-730.log`.

---
