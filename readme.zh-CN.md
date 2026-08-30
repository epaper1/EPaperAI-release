[English](readme.md) | [中文](readme.zh-CN.md)

感谢您选择 Maza-AI 智能板。本指南涵盖设备使用与开发。

# 烧录教程

使用以下命令检查可用串口，请确认板子是否连接到机器。烧录命令中默认使用COM3。

`powershell -NoProfile "[System.IO.Ports.SerialPort]::GetPortNames()"`


<details open>
<summary><strong>Maza-AI S31</strong></summary>

使用以下命令将固件烧录到 Maza-AI S31 开发板：

```powershell
python -m esptool --chip esp32s31 -p COM3 -b 921600 --before default-reset --after hard-reset write-flash 0x0 EPaperAI_S31.hex
```

</details>

<details>
<summary><strong>Maza-AI S3</strong></summary>

使用以下命令将固件烧录到 Maza-AI S3 开发板：

```powershell
python -m esptool --chip esp32s3 -p COM3 -b 921600 --before default-reset --after hard-reset write-flash 0x0 EPaperAI_S3.hex
```

</details>


# 使用设备网站

首次启动时，设备会创建一个 WiFi 热点。连接到 `192.168.4.1` 来配置你的 WiFi 连接（仅支持802.11b网络）。WiFi 信息会自动保存，之后每次启动都会自动连接到你的网络。

要查找设备的 IP 地址，请检查串口输出（使用 `idf.py` 或其他工具）。找到类似 `WiFi connected. IP: 192.168.1.20` 的信息，然后在手机或电脑上访问 http://192.168.1.20/ 打开设备网站。

如果不方便查看串口输出，可以双击中间按钮稍等片刻 —— 墨水屏屏幕上会显示 IP 地址和传感器信息。另外，长按中间按钮会重置当前IP，进入选网模式。

## 图片上传

在设备网站（如 http://192.168.1.20/ ），请确保选择正确的墨水屏型号（默认为 13.3E）。在 **"源图像"** 区域，你可以从手机或电脑中选择图片，通过缩放和裁剪调整，然后使用选定的算法进行处理（FS抖动通常效果最佳）。处理完成后，即可将图片上传到屏幕！

https://github.com/user-attachments/assets/2939e2c9-ce8a-43a8-aa6b-e04f5e882ee7

## 使用轮播功能

设备还支持定时从TF卡或远程图片刷新。在设备网站的 **"本地/远程图片"** 区域，勾选“轮播TF卡图片”并点击“保存到墨水屏”，系统会按文件名顺序轮播TF卡里的jpg/png图片（请使用 /photos 目录）。不勾选“轮播TF卡图片”并输入远程图片链接（只支持https），然后点击“保存到墨水屏”，系统可以定期获取远程图片并刷新屏幕。

显示图片后，ESP 会进入深度睡眠模式，并按设定的间隔（刷新间隔默认 86400 分钟，即 1 天）自动唤醒刷新。如果需要退出轮播功能，可以按reset按钮重启。断电重启也会退出轮播功能，需要在设备网站上重新设置。

> **注意：** JPEG图片仅支持基线（baseline）JPEG 格式，不支持渐进式（progressive）JPEG。

> **注意：** 如果需要轮播TF卡的图片，请把jpg/png图片放在TF卡的/photos目录下。注意该功能需要PSRAM来处理图片，图片如果过大系统会重启，会退出轮播功能。

https://github.com/user-attachments/assets/2c51f380-22c1-4d44-b713-0c94f185cf90
