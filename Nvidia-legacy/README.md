# Nvidia 580 Legacy Driver Setup

To maintain full hardware acceleration and system stability on older graphics cards, PikaOS provides a specialized Nvidia 580 legacy driver. Installing this driver automatically pulls in our tailored legacy kernel.

## Supported Hardware

This driver package is officially required for the following legacy Nvidia GPU series and architectures (including desktop and mobile variants) supported by the 580 driver branch:

### Maxwell Architecture
* **GeForce 900 Series**: GTX 980 Ti, GTX 980, GTX 970, GTX 960, GTX 950
* **GeForce 900M & Mobile Series**: GTX 980 (Mobile), GTX 980M, GTX 970M, GTX 965M, GTX 960M, GTX 950M, 945M, 940MX, 940M, 930MX, 930M, 920MX, 920M, 910M
* **GeForce 800M Series**: GTX 860M, GTX 850M, GTX 845M, GTX 840M, GTX 830M
* **GeForce 700 Series (Maxwell variants only)**: GTX 750 Ti, GTX 750
* **GeForce MX Series (Maxwell)**: 940MX, 930MX, MX130, MX110
* **TITAN Series (Maxwell)**: GeForce GTX TITAN X
* **NVIDIA RTX / Quadro (Maxwell)**: Quadro M6000, M5000, M4000, M2000, M1200, M620, M520, Quadro K620 (Maxwell variant)

### Pascal Architecture
* **GeForce GTX 10 Series**: GTX 1080 Ti, GTX 1080, GTX 1070 Ti, GTX 1070, GTX 1060, GTX 1050 Ti, GTX 1050, GT 1030
* **GeForce GTX 10 Series (Mobile / Max-Q)**: GTX 1080 Mobile, GTX 1080 Max-Q, GTX 1070 Mobile, GTX 1070 Max-Q, GTX 1060 Mobile, GTX 1060 Max-Q, GTX 1050 Ti Mobile, GTX 1050 Mobile
* **GeForce MX Series (Pascal)**: MX350, MX330, MX250, MX230, MX150
* **TITAN Series (Pascal)**: TITAN X / Xp
* **NVIDIA RTX / Quadro (Pascal)**: Quadro P6000, P5000, P4000, P3200, P3000, P2200, P2000, P1000, P620, P600, P520, P500

### Volta Architecture
* **TITAN Series (Volta)**: TITAN V

## Required Packages

The setup is bundled into a single installation step:
* **`pika-nvidia-driver`**: The Nvidia 580 legacy graphics driver.
* **`pika-legacy-kernel`**: The customized kernel patch (automatically installed as a dependency).

## Installation Steps

Open your terminal and execute the following `pikman` commands to install the driver and kernel.

### 1. Update System Repositories and Packages
```bash
pikman upgrade
```

### 2. Install the Nvidia Driver
```bash
pikman i pika-nvidia-driver
```
*Note: The `pika-legacy-kernel` is automatically configured as a dependency and will install alongside the driver.*

### 3. Reboot the System
Apply the changes and boot into the new legacy Nvidia driver by restarting your computer.
```bash
sudo reboot
```

## Verification

After rebooting, verify that both the correct kernel and the driver are active.

* **Check active kernel:**
  ```bash
  uname -r
  ```
  *The output must contain `pika-legacy`.*

* **Check Nvidia driver status:**
  ```bash
  nvidia-smi
  ```
  *The output should display driver version `580.xx`.*