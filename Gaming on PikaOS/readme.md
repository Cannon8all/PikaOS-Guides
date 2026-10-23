# Gaming on PikaOS

PikaOS ships with the latest graphics drivers out of the box, so your GPU is ready for gaming from the first boot. To get everything else you need, install the `pika-gameutils-meta` package. It preinstalls a set of launchers and tools:

- **Launchers:** Steam, Heroic, Lutris, Faugus Launcher
- **Tools:** ProtonPlus, GOverlay, MangoHud, Gamescope

Install it from a terminal:

```
pikman i pika-gameutils-meta
```

You can also install it from the **PikaOS Welcome** app.

---

## Which Proton version should I use?

### Official Proton versions (Valve)

- **Proton Experimental:** Valve's newest, most cutting-edge build. Pick it for new games, for games that struggle on the current stable release, or when [ProtonDB](https://www.protondb.com/) recommends it.
- **Proton 11.0:** The current stable release. A good default for games that are known to run well on it.
- **Proton 10.0 and older:** Older stable releases. Only use these if a game works with them and not with newer versions.

> Community builds are for experienced users who want extra features, want to run Proton outside of Steam, or need fixes that Valve's Proton doesn't have yet. They are not supported by Valve. They are community projects and experimental by nature.
{.is-info}

### Community Proton builds

- **Proton-CachyOS:** Made by the CachyOS team, with extra quality-of-life features, fixes and experimental changes. It comes in two flavors, `proton-cachyos-native` and `proton-cachyos-slr`. We recommend `proton-cachyos-slr`.
- **Proton-EM:** A good pick if you want winewayland and FSR4 without Proton-CachyOS's extras. Many of Proton-CachyOS's improvements originate here, so it's also the one to follow for the latest winewayland development.
- **Proton-GE:** Made by [GloriousEggroll](https://github.com/GloriousEggroll/proton-ge-custom). It includes various fixes and can help in specific cases.

---

## Shader pre-caching

Games compile shaders (small programs for your GPU) while you play. The first time you meet a new effect, this can cause stutter or long loading times.

Steam can compile some shaders ahead of time. With Proton-CachyOS, Proton-GE or Proton-EM, it's better to turn this off. These builds already include the codecs needed for in-game videos, and a reasonably modern CPU and GPU can handle compiling shaders on the fly.

Steam's pre-caching also doesn't cover everything, and updating the Proton version can force all shaders to be compiled again. If you have time to spare, you can leave it on.

To turn it off, open **Steam → Settings → Downloads** and untick:

- Allow background processing of Vulkan shaders
- Enable Shader Pre-caching

> After turning off Steam's shader pre-caching, increase your maximum shader cache size (see below).
{.is-warning}

---

## Increasing the shader cache size

Compiled shaders are stored on your system and reused later. The cache has a size limit, though, and once it's full, the oldest shaders are dropped. Some large games have over 1 GB of shaders, so they can end up recompiling on every launch. Raising the limit avoids the repeated stutter and long load times.

Open a terminal and create the config directory if it doesn't exist:

```
mkdir -p ~/.config/environment.d
```

Create the config file:

```
touch ~/.config/environment.d/gaming.conf
```

Open it in nano:

```
nano ~/.config/environment.d/gaming.conf
```

Paste the line for your GPU vendor.

**AMD** (12 GB):

```
MESA_SHADER_CACHE_MAX_SIZE=12G
```

**NVIDIA** (12 GB):

```
__GL_SHADER_DISK_CACHE_SIZE=12000000000
```

Save the file with `Ctrl + O` and `Enter`, then exit nano with `Ctrl + X`.

Log out and back in for the change to apply.

---

## falcond

falcond is PikaOS's profile-based gaming performance daemon. It runs quietly in the background, notices when you start a game, and applies the right performance settings for that game. You can make your own profiles and tweak settings by hand for each title.

A selection of popular profiles is already included, like the Proton profile that activates for any game running in a Proton compatibility layer.

> Don't use falcond together with the older gamemode. They will interfere with each other.
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

### falcond-gui

**falcond-gui** is a very handy app for falcond. Use it to manage falcond without editing files by hand.

---

## Troubleshooting Proton

### Capturing and sharing Proton logs

If a game crashes or misbehaves, a Proton log is the first thing anyone helping you will ask for.

> When sharing logs, upload them to [paste.pika-os.com](https://paste.pika-os.com/) instead of pasting them directly into the chat. Drag and drop the log file onto the website, then share the link it gives you.
{.is-info}

To turn on Proton logging for a game:

1. In Steam, right-click the game and select **Properties**.
2. Under **Launch Options**, enter:

```
PROTON_LOG=1 %command%
```

The next time you start the game, Proton writes a log file to your home folder named `steam-[AppID].log`. For example, Counter-Strike 2 has the AppID 730, so its log is `steam-730.log`.

### Saving logs to a custom folder

To save the log somewhere else, add `PROTON_LOG_DIR` with the folder you want:

```
PROTON_LOG=1 PROTON_LOG_DIR=/home/username/steam-logs %command%
```

Replace `username` with your own username.
