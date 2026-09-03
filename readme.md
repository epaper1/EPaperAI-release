[English](readme.md) | [中文](readme.zh-CN.md)

Thank you for choosing the Maza-AI board. This guide covers device flash and usage.

# Flash instruction

## Install ESP-IDF
Download EIM on https://dl.espressif.cn/dl/eim/, and install ESP-IDF v6.1. Refer to instruction for installation: https://docs.espressif.com/projects/esp-idf/en/v6.1/esp32/get-started/index.html#installation

Following flashing commands need run in the "IDF_v6.1_PowerShell" (You can see the app or icon after installation).

## Check serial port
Use the following command to list the available serial ports and confirm that the board is connected to your computer. The flashing commands use `COM3` by default.

`powershell -NoProfile "[System.IO.Ports.SerialPort]::GetPortNames()"`

## Flash
<details open>
<summary><strong>Maza-AI S31</strong></summary>

Use the following command to flash the firmware to the Maza-AI S31 board:

```powershell
python -m esptool --chip esp32s31 -p COM3 -b 921600 --before default-reset --after hard-reset write-flash 0x0 EPaperAI_S31.hex
```

</details>

<details>
<summary><strong>Maza-AI S3</strong></summary>

Use the following command to flash the firmware to the Maza-AI S3 board:

```powershell
python -m esptool --chip esp32s3 -p COM3 -b 921600 --before default-reset --after hard-reset write-flash 0x0 EPaperAI_S3.hex
```

</details>


# Use device Website

On first boot, the device will start as a WiFi hotspot. Connect to `192.168.4.1` to configure your WiFi connection (only support 802.11b). The credentials are saved automatically, so subsequent boots will connect to your network directly.

To find the device's IP address, check the serial output (using `idf.py monitor` or another tool). Look for a line like `WiFi connected. IP: 192.168.1.20`, then navigate to http://192.168.1.20/ from your phone or computer to open the device website.

Alternatively, double-click the middle button and wait a moment — the IP address and sensor information will be displayed on the e-Paper screen. In addition, long-press the middle button will reset the wifi and enter hotspot mode.

## Upload image

On the device website (e.g., http://192.168.1.20/), make sure you select the correct e-Paper type (13.3E by default). Under the **"Source Image"** section, you can choose an image from your phone or computer, pinch to zoom and crop it, then process it with the selected algorithm (Floyd-Steinberg dithering usually gives the best quality). Once processed, you can upload the image to the display!

https://github.com/user-attachments/assets/2939e2c9-ce8a-43a8-aa6b-e04f5e882ee7

## Slideshow

The device also supports periodic refresh from local TF card or remote url. In section **"Local/Remote Image"** of website, check "Slideshow images in TF card" and click "Save to E-Paper", it will read jpg/png images in TF card (under /photos folder) in alphabetical order and display them periodically. If you uncheck "Slideshow images in TF card" and enter an HTTPS image URL, then save to epaper, it will fetch the remote url periodically.

After displaying the image, the ESP will enter deep sleep and wake up automatically at the specified interval (refresh interval is 86400 minutes, i.e., 1 day, by default). If you want to exit slideshow mode, press reset button. Power off and restart will also exit slideshow mode. You need reconfig slideshow on website again.

> **Note:** Only baseline JPEG images are supported — progressive JPEG is not, due to a limitation of the JPEGDEC library.

> **Note:** If you want to slideshow images in TF card, save jpg/png images in /photos folder in TF card. Note this requires PSRAM for image processing. If the image is too big to fit PSRAM, the system will crash and exit slideshow mode.

https://github.com/user-attachments/assets/2c51f380-22c1-4d44-b713-0c94f185cf90

### Remote image

Use the following URL to test the remote image feature. For best results, set the refresh interval to once per day:

https://epboard1.yellowgrass-550156fe.eastasia.azurecontainerapps.io/image

The e-Paper display will look like this:

<img width="800" alt="Daily Brief" src="https://github.com/user-attachments/assets/29debc76-46de-4ffd-bf41-07f8d8be503e" />
