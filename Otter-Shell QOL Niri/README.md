# Some quality of life Otter-Shell tweaks for niri

## Chapters

1. Setup logout for otter-logout
2. Add a custom toggle button for otter-settings
3.
---

## 1. Setup logout for otter-logout
- Open otter-settings in the terminal with: otter-settings
- Go to the Logout tab
- Change the Logout Exec line to: niri msg action quit --skip-confirmation


<img width="1157" height="814" alt="otter logout (Edit)" src="https://github.com/user-attachments/assets/0b93fc81-6db2-4931-8482-fc9e35a16788" />

## 2. Add a custom toggle button in the bar for otter-settings
### Make a button
---

- Open otter-settings in the terminal with: otter-settings
- Go to the Bar tab
- In the bottom you can click (1): Add Custom Buttom

<img width="243" height="74" alt="Screenshot from 2026-04-14 19-13-34" src="https://github.com/user-attachments/assets/0576fc6a-196b-42b5-aa54-04d1e3fdf6e9" />

### Setup the command and icon
---

- Add the Button Settings Command (2): pkill otter-settings || otter-settings
- You can get the icons at: https://www.nerdfonts.com/
- Copy and paste the icon you want (3)

<img width="696" height="132" alt="Screenshot from 2026-04-14 19-14-00" src="https://github.com/user-attachments/assets/689e70c7-8c56-4494-9bcc-6c38c898be72" />

### Add the new widget to the bar
---

- Go back to the top of the Bar tab and add the custon named button you created as a widget.

<img width="1037" height="172" alt="Screenshot from 2026-04-14 19-17-30" src="https://github.com/user-attachments/assets/b3e7b93a-0d49-4054-92a0-54cc07b8444d" />
