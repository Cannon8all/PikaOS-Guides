## Pre-Requisites
- At least 4 GB of RAM
- At least 2 CPU cores
- Virtualization enabled in the BIOS
- Docker and Docker Compose
- FreeRDP

### How to enable virtualization in the BIOS
During startup, press the designated key for your motherboard brand to enter the BIOS (consult your motherboard manual if unsure):
`Delete`, `F10`, `F1`, `F2`, etc.

**Common BIOS Setting Names for Intel:**
- Intel Virtualization Technology
- Intel VT-x / Intel VT
- Intel (VMX) Virtualization Technology

**Common BIOS Setting Names for AMD:**
- SVM Mode (Secure Virtual Machine)
- AMD Virtualization
- AMD-V


| Brand | Navigation Path in BIOS | Setting Name |
| :--- | :--- | :--- |
| **ASUS** | Advanced Mode (F7) > Advanced > CPU Configuration | Intel Virtualization Technology / SVM Mode |
| **MSI** | Overclocking / OC > CPU Features / Advanced CPU Configuration | Intel Virtualization Tech / SVM Mode |
| **Gigabyte** | Tweaker / M.I.T. > Advanced CPU Core Settings | Intel Virtualization Technology / SVM Mode |
| **HP** | Configuration / System Configuration | Virtualization Technology |
| **Dell** | Settings > Virtualization Support | Virtualization |
| **Lenovo** | Security > Virtualization | Intel Virtualization Technology / AMD-V |



# Install and Setup Docker

**Install Docker from the PikaOS Repository.**
>`pikman i docker.io docker-compose`


**Add your user to the "docker" group.**
>`sudo groupadd docker`
> `sudo usermod -aG docker $USER
`

**Enable the Docker service at boot.**
>`sudo systemctl enable --now docker`


# Install FreeRDP

**Install Stable via Flatpak.**
>`flatpak install com.freerdp.FreeRDP`

**Install Experimental via Nightly FreeRDP Repository**
Follow the FreeRDP instructions on how to add the repository and installation.
>https://github.com/FreeRDP/FreeRDP/wiki/PreBuilds

# Download and Install Winboat
**Download the `.deb` file from the official Winboat website.
https://winboat.app/**

## Install the package
**Run the installer (adapt the filename below to match your downloaded version):**
>`pikman i ~/Downloads/winboat-0.9.0-amd64.deb
`

---

# Final Step: Reboot

> You must **log out and log back in** (or restart your PC) before launching Winboat. 
> This applies the changes to your user groups so Winboat can communicate with Docker without permission issues.

# System Performance Impact: 
> Please note that Docker and Winboat are configured to start automatically when your system boots. 
> Because Winboat runs a virtualized environment inside a background container, this autostart behavior can increase system boot times and consume notable CPU and RAM resources right after startup. 
{.is-warning}

**If you experience performance degradation on lower-end systems, consider disabling the Docker service at boot or configuring Winboat to run only on demand.**


## Disable Winboat Container Autostart (Application Level)
**To prevent the Windows environment itself from launching automatically in the background when Docker starts, change the toggle inside the application settings:**

1. Open **Winboat**.
2. Go to the **Configuration** menu on the left side.
3. Turn off the **Auto Start Container** toggle.

![winboatautostart.png](/winboatautostart.png)

## Disable Docker Service Autostart
**To prevent Docker from running automatically when you boot your computer, run this command in your terminal:**
>`sudo systemctl disable docker
`

*Note: After disabling autostart, you must manually start the Docker background service every time you want to use Winboat by running:*
>`sudo systemctl start docker
`