# Installing Noctalia-shell on PikaOS for Hyprland and Niri

>**Hyprland configuration guide**
{.is-success}

---



> Optionally before editing configuration files backup your previous configuration located in:
{.is-warning}

 - ~/.config/hypr 
- ~/.config/niri 
- ~/.config/quickshell

# Install the noctalia-shell package in the terminal

>`pikman install noctalia-shell`

# Deactivate the default pikabar
- Open your ```~/.config/hypr/exec.conf```
>**Remove or comment out (`#`) the following line:**
>`exec-once=pikabar`

Optionally you can disable swww sinds noctalia comes with its own wallpaper manager
>**Remove or comment out (`#`) the following line**
>`spawn-at-startup "swww-daemon"`

# Add the noctalia-shell
Add the following to your ```~/.config/hypr/exec.conf```
>`exec-once = qs -c noctalia-shell`

# Configure the keybinds
Noctalia uses its own launcher. Replace the default keybind in `~/.config/hypr/keybinds.conf`

**Old configuration:**

>`bind=$mainMod,D,exec,walker`


**New configuration:**
>`bind=$mainMod,D,exec,qs -c ipc call launcher toggle
`

**Restart your session to see the changes.**

<br><br>
---
>**Niri configuration guide**
{.is-success}

<br>

> Optionally before editing configuration files backup your previous configuration located in:
{.is-warning}

- ~/.config/niri/config.kdl

# Install the noctalia-shell package in the terminal

>`pikman install noctalia-shell`

# Deactivate the Default Pikabar
Open `~/.config/niriconfig.kdl`. 

>**Remove or comment out (`//`) the following line**
>`spawn-at-startup "pikabar"`

Optionally you can disable swww sinds noctalia comes with its own wallpaper manager
>**Remove or comment out (`//`) the following line**
>`spawn-at-startup "swww-daemon"`


# Add the Noctalia-Shell
Add the following line to `~/.config/niri/config.kdl`

>`spawn-at-startup "qs" "-c" "noctalia-shell"`

# Configure the Keybinds
Noctalia uses its own launcher. Replace the default keybind in `~/.config/niri/config.kdl`.

**Old configuration:**

>`Mod+D { spawn "pikabar-launcher"; }`

**New configuration:**

>`Mod+D { spawn "qs" "-c" "noctalia-shell" "ipc" "call" "launcher" "toggle"; }`


# Set Up Noctalia wallpaper and overview
To ensure the wallpaper layer renders correctly, add or adjust the following layer-rule:
>layer-rule {
    match namespace="^noctalia-wallpaper*"
    place-within-backdrop true
}


> Check out the official Noctalia Wiki for additional background and overview configuration options.
https://docs.noctalia.dev/v4/getting-started/compositor-settings/niri/
{.is-success}

**Restart your session to see the changes.**