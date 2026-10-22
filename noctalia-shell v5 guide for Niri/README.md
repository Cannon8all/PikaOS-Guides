# Hyprland configuration guide





> Optionally before editing configuration files backup your previous configuration located in:
{.is-warning}
 - ~/.config/hypr 


# Install the noctalia-shell package in the terminal

>`pikman install noctalia-shell`

# Deactivate Otter-shell
**Open your ``~/.config/hypr/exec.conf``**
Remove or comment out (`--`) the following lines:
>hl.exec_cmd("otter-wallpaper")
>hl.exec_cmd("otter-osd")
>hl.exec_cmd("otter-notifications")
>hl.exec_cmd("otter-polkit")
>hl.exec_cmd("otter-idle")
>hl.exec_cmd("otter-theme-gen")
>hl.exec_cmd("otter-weather --watch 900 --write-cache")
>hl.exec_cmd("otter-search")
>hl.exec_cmd("otter-clip daemon")
>hl.exec_cmd("otter-bar")


# Add the noctalia-shell
Add the following to your ```~/.config/hypr/exec.conf```
>hl.exec_cmd("noctalia")

# Configure the keybinds
## Core Noctalia keybinds
**Open your ```~/.config/hypr/keybinds.conf```**
Remove or comment (`--`) the following keybinds


>hl.bind(mainMod .. " + " .. "D", hl.dsp.exec_cmd("otter-launcher"), { description = "Application Launcher" })
>hl.bind(mainMod .. " + " .. "P", hl.dsp.exec_cmd("otter-screenshot"), { description = "Screenshot" })
>hl.bind(mainMod .. " + " .. "SHIFT + P", hl.dsp.exec_cmd("otter-rec"), { description = "Screen Recorder" })
>hl.bind(mainMod .. " + " .. "T", hl.dsp.exec_cmd("otter-transcribectl toggle"), { description = "Toggle Transcription" })
>hl.bind(mainMod .. " + " .. "tab",  hl.dsp.exec_cmd("otter-overview"), { description = "Show Overview" })

**Add the Noctalia keybinds**

>hl.bind(mainMod .. "+Space", hl.dsp.exec_cmd(ipc .. "panel-toggle launcher"))
>hl.bind(mainMod .. "+S", hl.dsp.exec_cmd(ipc .. "panel-toggle control-center"))
>hl.bind(mainMod .. "+comma", hl.dsp.exec_cmd(ipc .. "settings-toggle"))
>hl.bind("ALT + Tab", hl.dsp.exec_cmd(ipc .. "window-switcher"))
>hl.bind("PRINT", hl.dsp.exec_cmd("noctalia msg screenshot-region"))

## Audio
**Replace**
>hl.bind("XF86AudioRaiseVolume", hl.dsp.exec_cmd("otter-osd volume-up 5"), { locked = true, repeating = true, description = "Raise Volume" })
>hl.bind("XF86AudioLowerVolume", hl.dsp.exec_cmd("otter-osd volume-down 5"), { locked = true, repeating = true, description = "Lower Volume" })
>hl.bind("XF86AudioMute", hl.dsp.exec_cmd("otter-osd volume-mute-toggle"), { locked = true, description = "Toggle Mute" })

**With**

>hl.bind("XF86AudioRaiseVolume", hl.dsp.exec_cmd(ipc .. "volume-up"))
>hl.bind("XF86AudioLowerVolume", hl.dsp.exec_cmd(ipc .. "volume-down"))
>hl.bind("XF86AudioMute", hl.dsp.exec_cmd(ipc .. "volume-mute"))

## Brightness
**Replace**
>hl.bind("XF86MonBrightnessUp", hl.dsp.exec_cmd("otter-osd brightness-up 5"), { locked = true, repeating = true, description = "Brightness Up" })
>hl.bind("XF86MonBrightnessDown", hl.dsp.exec_cmd("otter-osd brightness-down 5"), { locked = true, repeating = true, description = "Brightness Down" })
>hl.bind("Caps_Lock", hl.dsp.exec_cmd("otter-osd caps-lock"), { locked = true, description = "Caps Lock OSD" })

**With**

>hl.bind("XF86MonBrightnessUp", hl.dsp.exec_cmd(ipc .. "brightness-up"))
>hl.bind("XF86MonBrightnessDown", hl.dsp.exec_cmd(ipc .. "brightness-down"))

# Window rules
**Open your ```~/.config/hypr/windows.conf```**
Add the following rules

## Enable blur for Noctalia’s bar, panels, dock, and notifications.
>--Noctalia blur
hl.layer_rule({
  name = "noctalia",
  match = {
    namespace = "^noctalia-(bar-.+|notification|dock|panel|attached-panel|osd|window-switcher)$",
  },
  no_anim = true,
  ignore_alpha = 0.5,
  blur = true,
  blur_popups = true,
})

## Noctalia Settings window rule
>-- Noctalia Settings
hl.window_rule({
    match = { class = "dev.noctalia.Noctalia" },
    float = true,
    size = { 1080, 920 },
})





---
> Check out the official Noctalia Wiki for additional window appearance and blur configuration options.
https://docs.noctalia.dev/v4/getting-started/compositor-settings/hyprland/
{.is-success}

**Restart your session to see the changes.**
