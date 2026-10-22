# Install Otter-Shell
Install the core shell.

> pikman i otter-shell


# Backup Configuration
> Before making changes, back up your existing configuration directory located at: ~/.config/hypr/
> {.is-warning}


# Deactivate Default Modules
In ~/.config/hypr/ remove or comment out the following lines to disable the default Pikabar/Noctalia shell components:
**Remove or //**
>exec-once=systemctl --user enable --now hypridle
>exec-once=/usr/bin/mate-polkit
>exec-once=swww-daemon
>exec-once=pikabar

# Add the Otter-Shell Autostart Modules 
Add these lines to your config.kdl to ensure the Otter-Shell modules start at boot:
>exec-once=otter-bar
>exec-once=otter-wallpaper
>exec-once=otter-osd
>exec-once=otter-notifications
>exec-once=otter-polkit
>exec-once=otter-idle
>exec-once=otter-weather --watch 900 --write-cache
>exec-once=otter-clip daemon
>exec-once=otter-theme-gen
>exec-once=otter-search
>exec-once=otter-transcribe

# Configure keybinds
Configure your primary navigation and session shortcuts. You can adjust these keys to suit your preference.
The following keybinds can be used as an example.

**Edit ~/.config/hypr/keybinds.conf** 
## Launcher
 **Change**
>bind=$mainMod,d,exec,pikabar-launcher

**Into**
>bind=$mainMod, d, exec, pkill otter-launcher || otter-launcher
{.is-success}

## Logout
**Add**
>bind=CTRL ALT, Delete, exec, pkill otter-logout || otter-logout

## Screenshot
**Delete** all 5 lines at the bottom of the config from:
bind=CTRL,Print,exec,grim...

This Keybind will use **PrtScn** and save screenshots to the clipboard

**Add**
>bind = , Print, exec, otter-screenshot

This keybind will use **Shift+PrtScn** and save screenshots to the Pictures folder

**Add**
>bind = SHIFT, Print, exec, otter-screenshot --freeze --output ~/Pictures/screenshot.png

# Notifications and OSD
## Capslock and Numlock
**Add**
>bindl = , Caps_Lock, exec, otter-osd caps-lock
>bindl = , Num_Lock,  exec, otter-osd num-lock
## Audio
**Replace the current 3 XF86Audio... keybinds**
>bindle = , XF86AudioRaiseVolume, exec, otter-osd volume-up 5
>bindle = , XF86AudioLowerVolume, exec, otter-osd volume-down 5
>bindl  = , XF86AudioMute, exec, otter-osd volume-mute-toggle
## Brightness (unfinished)
**Replace the current XF86MonBrightness... keybinds**
>bindle = , XF86MonBrightnessUp,   exec, brightnessctl set +5% && otter-osd brightness-up 5
>bindle = , XF86MonBrightnessDown, exec, brightnessctl set -5% && otter-osd brightness-down 5

> For laptop's this could need additional parameters to make sure the correct backlight is controlled.
{.is-warning}
