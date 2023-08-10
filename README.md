# XIAO_ESP32C3_Wifi_Tracker
<Picture 1>

---
## Introduction
In this tutorial, we will provide a concise overview of utilizing the XIAO ESP32C3 with Micropython. Furthermore, we will explore the practical application of a Wi-Fi signal strength tracker, which proves invaluable when establishing a high-speed, high-quality family network.
Consequently, by adhering to the Wi-Fi tracker's guidance, we can effectively optimize the placement of the Wi-Fi signal enhancer, ensuring optimal signal coverage.

### Hardware

1. XIAO ESP32C3
<div class="get_one_now_container" style={{textAlign: 'center'}}>
    <a class="get_one_now_item" href="https://www.seeedstudio.com/Seeed-XIAO-ESP32C3-p-5431.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    </a>
</div>
<Picture 2>
  
 2. Expansion Board Base for XIAO
<div class="get_one_now_container" style={{textAlign: 'center'}}>
    <a class="get_one_now_item" href="https://www.seeedstudio.com/Seeeduino-XIAO-Expansion-board-p-4746.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    </a>
</div>
<Picture 3>
  
### Software

1. Thonny for micropython
<div class="get_one_now_container" style={{textAlign: 'center'}}>
    <a class="get_one_now_container" href="https://thonny.org/">
            <strong><span><font color={'FFFFFF'} size={"4"}> Download Here 🖱️</font></span></strong>
    </a>
</div>
<Picture 4>
  
2. Esptool
<div class="get_one_now_container" style={{textAlign: 'center'}}>
    <a class="get_one_now_container" href="https://github.com/espressif/esptool">
            <strong><span><font color={'FFFFFF'} size={"4"}> Git clone Here 🖱️</font></span></strong>
    </a>
</div>
<Picture 5>
  
---
## Micropython Configuration

### Install -Windows
Please follow the step in the picture
<Picture 6>

### Update the firmware with esptool

1. Open your own file location

``` git clone https://github.com/espressif/esptool.git ```

2. Download the latest firmware (This Tutorial is v1.20.0 (2023-04-26) .bin)
   
``` https://micropython.org/download/esp32c3-usb/```

3. Put the latest firm in this file location and open the file in CMD

```your own file location\esptool-master\esptool```

4. Flash the firmware by entering this command in CMD （enter bootloader mode before flashing）

```
esptool.exe --chip esp32c3 --port COM10 --baud 921600 --before default_reset --after hard_reset --no-stub  write_flash --flash_mode dio --flash_freq 80m 0x0 esp32c3-usb-20230426-v1.20.0.bin
```

Note: If you use linux, change "esptool.exe" to "esptool.py". Change  "COM10" to your own serial port. Change "esp32c3-usb-20230426-v1.20.0.bin" to the latest firmware name you downloaded.
 
### Micropython Setup

 1. Plug in your XIAO ESP32C3, open Thonny and click right bottom to configure interpreter

 2. Select interpreter- Micropython (ESP32) and Port >>> Click OK

Note: If everything goes well, you will see the output in the shell

### Install required libraries

Click "Tools" >>> Click "Management Packages" >>> Enter Library's name >>> Click "Search micropython-lib and PyPl"

### Run the scrip and Flash it to the board

1. After you finish coding, click the green button to run the scrip

2. Flash the code to the board by save the file to the board as "boot.py"

---

## Micropython Demo

### Light an Oled Screen

#### 1. Hello Seeder!

```
import time
from machine import Pin, SoftI2C
import ssd1306
import math

# ESP8266 Pin assignment
i2c = SoftI2C(scl=Pin(7), sda=Pin(6))  # Adjust the Pin numbers based on your connections
oled_width = 128
oled_height = 64
oled = ssd1306.SSD1306_I2C(oled_width, oled_height, i2c)

oled.fill(0)  # Clear the screen
oled.text("Hello, Seeder!", 10, 15)
oled.text("/////", 30, 40)
oled.text("(`3`)y", 30, 55)
oled.show()  # Show the text
```

#### 2. Loading dynamic effect

```
import time
from machine import Pin, SoftI2C
import ssd1306
import math

# ESP8266 Pin assignment
i2c = SoftI2C(scl=Pin(7), sda=Pin(6))  # Adjust the Pin numbers based on your connections
oled_width = 128
oled_height = 64
oled = ssd1306.SSD1306_I2C(oled_width, oled_height, i2c)

center_x = oled_width // 2
center_y = oled_height // 2
square_size = 6  # Size of each square
num_squares = 12  # Number of squares
angle_increment = 2 * math.pi / num_squares

while True:
    oled.fill(0)  # Clear the screen
    
    for i in range(num_squares):
        angle = i * angle_increment
        x = int(center_x + (center_x - square_size-30) * math.cos(angle))
        y = int(center_y + (center_x - square_size-30) * math.sin(angle))
        
        # Draw all squares
        for j in range(num_squares):
            angle_j = j * angle_increment
            x_j = int(center_x + (center_x - square_size-30) * math.cos(angle_j))
            y_j = int(center_y + (center_x - square_size-30) * math.sin(angle_j))
            
            oled.fill_rect(x_j, y_j, square_size, square_size, 1)  # Draw the square
        
        oled.fill_rect(x, y, square_size, square_size, 0)  # Erase the current square
        oled.show()
        time.sleep_ms(100)  # Pause before next iteration

```

### Light a Buzzer

#### 1. Sound

```
import time
from time import sleep
import machine
from machine import Pin, SoftI2C


# Buzzer settings

buzzer_pin = machine.Pin(5, machine.Pin.OUT)
buzzer = machine.PWM(buzzer_pin)
buzzer.freq(1047)

# Buzzer working

while True:

    buzzer.duty(10)
    time.sleep(1)
    buzzer.duty(0)
    time.sleep(1)
```

#### 2. Play the Song << He's a pirate >>

```
import machine
import time

# Buzzer settings
buzzer_pin = machine.Pin(5, machine.Pin.OUT)
buzzer = machine.PWM(buzzer_pin)
buzzer.freq(1047)

# Defining frequency of each music note
NOTE_C4 = 262
NOTE_D4 = 294
NOTE_E4 = 330
NOTE_F4 = 349
NOTE_G4 = 392
NOTE_A4 = 440
NOTE_B4 = 494
NOTE_C5 = 523
NOTE_D5 = 587
NOTE_E5 = 659
NOTE_F5 = 698
NOTE_G5 = 784
NOTE_A5 = 880
NOTE_B5 = 988

# Music notes of the song, 0 is a rest/pulse
notes = [
    NOTE_E4, NOTE_G4, NOTE_A4, NOTE_A4, 0,
    NOTE_A4, NOTE_B4, NOTE_C5, NOTE_C5, 0,
    NOTE_C5, NOTE_D5, NOTE_B4, NOTE_B4, 0,
    NOTE_A4, NOTE_G4, NOTE_A4, 0,

    NOTE_E4, NOTE_G4, NOTE_A4, NOTE_A4, 0,
    NOTE_A4, NOTE_B4, NOTE_C5, NOTE_C5, 0,
    NOTE_C5, NOTE_D5, NOTE_B4, NOTE_B4, 0,
    NOTE_A4, NOTE_G4, NOTE_A4, 0,

    NOTE_E4, NOTE_G4, NOTE_A4, NOTE_A4, 0,
    NOTE_A4, NOTE_C5, NOTE_D5, NOTE_D5, 0,
    NOTE_D5, NOTE_E5, NOTE_F5, NOTE_F5, 0,
    NOTE_E5, NOTE_D5, NOTE_E5, NOTE_A4, 0,

    NOTE_A4, NOTE_B4, NOTE_C5, NOTE_C5, 0,
    NOTE_D5, NOTE_E5, NOTE_A4, 0,
    NOTE_A4, NOTE_C5, NOTE_B4, NOTE_B4, 0,
    NOTE_C5, NOTE_A4, NOTE_B4, 0,

    NOTE_A4, NOTE_A4,
    #Repeat of first part
    NOTE_A4, NOTE_B4, NOTE_C5, NOTE_C5, 0,
    NOTE_C5, NOTE_D5, NOTE_B4, NOTE_B4, 0,
    NOTE_A4, NOTE_G4, NOTE_A4, 0,

    NOTE_E4, NOTE_G4, NOTE_A4, NOTE_A4, 0,
    NOTE_A4, NOTE_B4, NOTE_C5, NOTE_C5, 0,
    NOTE_C5, NOTE_D5, NOTE_B4, NOTE_B4, 0,
    NOTE_A4, NOTE_G4, NOTE_A4, 0,

    NOTE_E4, NOTE_G4, NOTE_A4, NOTE_A4, 0,
    NOTE_A4, NOTE_C5, NOTE_D5, NOTE_D5, 0,
    NOTE_D5, NOTE_E5, NOTE_F5, NOTE_F5, 0,
    NOTE_E5, NOTE_D5, NOTE_E5, NOTE_A4, 0,

    NOTE_A4, NOTE_B4, NOTE_C5, NOTE_C5, 0,
    NOTE_D5, NOTE_E5, NOTE_A4, 0,
    NOTE_A4, NOTE_C5, NOTE_B4, NOTE_B4, 0,
    NOTE_C5, NOTE_A4, NOTE_B4, 0,
    #End of Repeat

    NOTE_E5, 0, 0, NOTE_F5, 0, 0,
    NOTE_E5, NOTE_E5, 0, NOTE_G5, 0, NOTE_E5, NOTE_D5, 0, 0,
    NOTE_D5, 0, 0, NOTE_C5, 0, 0,
    NOTE_B4, NOTE_C5, 0, NOTE_B4, 0, NOTE_A4,

    NOTE_E5, 0, 0, NOTE_F5, 0, 0,
    NOTE_E5, NOTE_E5, 0, NOTE_G5, 0, NOTE_E5, NOTE_D5, 0, 0,
    NOTE_D5, 0, 0, NOTE_C5, 0, 0,
    NOTE_B4, NOTE_C5, 0, NOTE_B4, 0, NOTE_A4
]

# Durations (in ms) of each music note of the song
# Quarter Note is 250 ms when songSpeed = 1.0
durations = [
    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 375, 125,

    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 375, 125,

    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 125, 250, 125,

    125, 125, 250, 125, 125,
    250, 125, 250, 125,
    125, 125, 250, 125, 125,
    125, 125, 375, 375,

    250, 125,
    #Rpeat of First Part
    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 375, 125,

    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 375, 125,

    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 250, 125, 125,
    125, 125, 125, 250, 125,

    125, 125, 250, 125, 125,
    250, 125, 250, 125,
    125, 125, 250, 125, 125,
    125, 125, 375, 375,
    #End of Repeat

    250, 125, 375, 250, 125, 375,
    125, 125, 125, 125, 125, 125, 125, 125, 375,
    250, 125, 375, 250, 125, 375,
    125, 125, 125, 125, 125, 500,

    250, 125, 375, 250, 125, 375,
    125, 125, 125, 125, 125, 125, 125, 125, 375,
    250, 125, 375, 250, 125, 375,
    125, 125, 125, 125, 125, 500
]

def play_song():
    total_notes = len(notes)
    for i in range(total_notes):
        current_note = notes[i]
        wait = durations[i]
        if current_note != 0:
            buzzer.duty(512)  # Set duty cycle for sound
            buzzer.freq(current_note)  # Set frequency of the note
        else:
            buzzer.duty(0)  # Turn off the sound
        time.sleep_ms(wait)
        buzzer.duty(0)  # Turn off the sound
while True:
    # Play the song
    play_song()
```
### Connect to Wi-fi

#### 1. Connect to Wi-fi

```
import network
import urequests
import utime as time

# Network settings
wifi_ssid = "Your Own SSID"
wifi_password = "Your Own Password"

def scan_and_connect():
    station = network.WLAN(network.STA_IF)
    station.active(True)

    print("Scanning for WiFi networks, please wait...")
    for ssid, bssid, channel, RSSI, authmode, hidden in station.scan():
        print("* {:s}".format(ssid))
        print("   - Channel: {}".format(channel))
        print("   - RSSI: {}".format(RSSI))
        print("   - BSSID: {:02x}:{:02x}:{:02x}:{:02x}:{:02x}:{:02x}".format(*bssid))
        print()

    while not station.isconnected():
        print("Connecting...")
        station.connect(wifi_ssid, wifi_password)
        time.sleep(10)

    print("Connected!")
    print("My IP Address:", station.ifconfig()[0])


# Execute the functions
scan_and_connect()
```

#### 2. Request New York Time online

```
from machine import Pin, SoftI2C
import ssd1306
from time import sleep
import time
import network
import urequests
import ujson

# ESP32 Pin assignment
# i2c = SoftI2C(scl=Pin(22), sda=Pin(21))

# ESP8266 Pin assignment
i2c = SoftI2C(scl=Pin(7), sda=Pin(6))  # Adjust the Pin numbers based on your connections

oled_width = 128
oled_height = 64
oled = ssd1306.SSD1306_I2C(oled_width, oled_height, i2c)

station = network.WLAN(network.STA_IF)
station.active(True)

# Network settings
wifi_ssid = "UMASS fried chicken"
wifi_password = "Zacharyloveschicken"
url = "http://worldtimeapi.org/api/timezone/America/New_York"

print("Scanning for WiFi networks, please wait...")
authmodes = ['Open', 'WEP', 'WPA-PSK' 'WPA2-PSK4', 'WPA/WPA2-PSK']
for (ssid, bssid, channel, RSSI, authmode, hidden) in station.scan():
    print("* {:s}".format(ssid))
    print("   - Channel: {}".format(channel))
    print("   - RSSI: {}".format(RSSI))
    print("   - BSSID: {:02x}:{:02x}:{:02x}:{:02x}:{:02x}:{:02x}".format(*bssid))
    print()

# Continually try to connect to WiFi access point
while not station.isconnected():
    # Try to connect to WiFi access point
    print("Connecting...")
    station.connect(wifi_ssid, wifi_password)
    time.sleep(10)

# Display connection details
print("Connected!")
print("My IP Address:", station.ifconfig()[0])


while True:
    # Perform HTTP GET request on a non-SSL web
    response = urequests.get(url)
    # Check if the request was successful
    if response.status_code == 200:
        # Parse the JSON response
        data = ujson.loads(response.text)
        # Extract the "datetime" field for New York
        ny_datetime = data["datetime"]
        # Split the date and time components
        date_part, time_part = ny_datetime.split("T")
        # Get only the first two decimal places of the time
        time_part = time_part[:8]
        # Get the timezone
        timezone = data["timezone"]
        
        # Clear the OLED display
        oled.fill(0)
        
        # Display the New York date and time on separate lines
        oled.text("New York Date:", 0, 0)
        oled.text(date_part, 0, 10)
        oled.text("New York Time:", 0, 20)
        oled.text(time_part, 0, 30)
        oled.text("Timezone:", 0, 40)
        oled.text(timezone, 0, 50)
        # Update the display
        oled.show()
    else:
        oled.text("Failed to get the time for New York!")
        # Update the display
        oled.show()
```

### ☆☆☆ Wi-fi Signal Strength Tracker

This  is the  main task in this project. Through this code, you can trake your wifi signal at home with such an easy device.

```
import network
import time
from time import sleep
import machine
from machine import Pin, SoftI2C
import ssd1306
import math

# ESP32C3 Pin assignment
i2c = SoftI2C(scl=Pin(7), sda=Pin(6))  # Adjust the Pin numbers based on your connections
oled_width = 128
oled_height = 64
oled = ssd1306.SSD1306_I2C(oled_width, oled_height, i2c)

# Network settings
wifi_ssid = "Your Own SSID"
wifi_password = "Your Own Password"
machine.freq(160000000)  # Set CPU frequency to 160 MHz (ESP8266 specific)
oled.text("Starting up...", 0, 0)
oled.show()

station = network.WLAN(network.STA_IF)
station.active(True)
station.connect(wifi_ssid, wifi_password)
time.sleep(1)

while not station.isconnected():
    time.sleep(1)

oled.fill(0)
oled.text("Connecting to", 0, 0)
oled.text(wifi_ssid, 0, 20)
oled.show()
time.sleep(2)

oled.fill(0)
ip_address = station.ifconfig()[0]  # Get the IP address
oled.text("Connected! ", 0, 0)
oled.text("IP Address:", 0, 20)
oled.text(ip_address, 0, 40)
oled.show()
time.sleep(2)

# Buzzer settings
buzzer_pin = machine.Pin(5, machine.Pin.OUT)
buzzer = machine.PWM(buzzer_pin)
buzzer.freq(1047)
buzzer.duty(0)

center_x = oled_width // 2
center_y = oled_height // 2
square_size = 6  # Size of each square
num_squares = 12  # Number of squares
angle_increment = 2 * math.pi / num_squares

x_pos = [12, 38, 64, 90]
statuses = ["poor", "normal", "good", "excellent"]

def calculate_block_count(rssi):
    # Determine the number of blocks based on RSSI values
    if -80 <= rssi < -60:
        return 1
    elif -60 <= rssi < -40:
        return 2
    elif -40 <= rssi < -20:
        return 3
    elif -20 <= rssi <= 10:
        return 4

def draw_blocks(count):
    for i in range(count):
        y_pos = 50 - calculate_block_height(i)
        oled.fill_rect(x_pos[i], y_pos, 24, calculate_block_height(i), 1)
    for i in range(count, 4):  # Clear unused area
        y_pos = 50 - calculate_block_height(i)
        oled.fill_rect(x_pos[i], y_pos, 24, calculate_block_height(i), 0)

def calculate_block_height(index):
    return 10 * (index + 1)

loop_count = 0  # Initialize loop count

while loop_count < 2:  # Execute the loop 24 times
    oled.fill(0)  # Clear the screen
    
    for i in range(num_squares):
        angle = i * angle_increment
        x = int(center_x + (center_x - square_size-30) * math.cos(angle))
        y = int(center_y + (center_x - square_size-30) * math.sin(angle))
        
        # Draw all squares
        for j in range(num_squares):
            angle_j = j * angle_increment
            x_j = int(center_x + (center_x - square_size-30) * math.cos(angle_j))
            y_j = int(center_y + (center_x - square_size-30) * math.sin(angle_j))
            
            oled.fill_rect(x_j, y_j, square_size, square_size, 1)  # Draw the square
        
        oled.fill_rect(x, y, square_size, square_size, 0)  # Erase the current square
        oled.show()
        time.sleep_ms(100)  # Pause before next iteration
        
    loop_count += 1  # Increase loop count

oled.fill(0)  # Clear the screen after finishing the loops
oled.show()

while True:
    oled.fill(0)
    station = network.WLAN(network.STA_IF)
    time.sleep(0.1)
    rssi = station.status('rssi')
    rssi_duty = 160 + 2 * int(rssi)
    rssi_duty_2 = int(rssi_duty / 2)
    rssi_abs = abs(int(rssi)) / 100
 
    block_count = calculate_block_count(rssi)
    status = statuses[block_count - 1]  # Get the status text based on block count
    
    draw_blocks(block_count)
    
    oled.text(status, 11, 56)
    
    oled.text("RSSI:", 0, 0)
    oled.text(str(rssi), 40, 0)
    # Update the display
    oled.show()

    buzzer.duty(rssi_duty)
    time.sleep(rssi_abs)
    buzzer.duty(0)
    time.sleep(rssi_abs)
    buzzer.duty(rssi_duty_2)
    time.sleep(rssi_abs)
    buzzer.duty(0)
    time.sleep(rssi_abs)

```
1. Micropython Configuration
1.1 Installation
1.2 Firmware ESPTool
1.3 Library
2. Connect to your wifi
2.1 Introduction
2.2 Example codes
3. Buzzer Work
3.1 Introduction
3.2 Example codes
4. Light an OLED
4.1 Introduction
4.2 Example codes
5. Wifi signal strength tracker
5.1 Introduction
5.2 Example codes
5.3 Application






---
description: wiki 介绍，一句话
title: Wiki 名字，会显示在左侧一栏和 wiki 的顶部一栏，要简介
keywords:
- 填写本文件所在的上一级目录 (Grove，SenseCAP，reTerminal...)
image: https://files.seeedstudio.com/wiki/seeed_logo/logo_2023.png
slug: /md 文件名 注意文件名
last_update:
  date: 05/31/2023
  author: 你的名字
---

# 写在前面

1.这里是有关产品的功能使用的 wiki 标准介绍，包括产品的本身功能，基于产品的 project等，产品介绍的 wiki 的标准请看https://doc.weixin.qq.com/doc/w3_AQIA4QaAAGk169LtP88RoS1h8XyDg?scode=AGEAZwfLABEgxWyQ5iAQIA4QaAAGk。


# Wiki Template

> 产品图：

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/seeed_logo/logo_2023.png" style={{width:1000, height:'auto'}}/></div>


> 购买链接附上：

<div class="get_one_now_container" style={{textAlign: 'center'}}>
    <a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    </a>
</div>

## Introduction

> 此处完成产品的介绍

> 包括产品的简单介绍、优势、应用场景等。

> 产品的组成可以通过列表来展示。

| Item                  |Value      |Remarks    |
| :---------:           |:--------- |:---------:|
| CPU                   |           |           |
| Flash Memory          |           | KB        |
| Memory                |           |           |
| SRAM                  |           | KB        |
| Module Storage        |           |           |
| Carrier Board Storage |           |           |
| Wifi                  |           |           |
| Display               |           |           |
| Bluetooth             |           |           |

### Features部分用无序列表列出。

- 
- 
- 

## Hardware Overview

Before everything starts, it is quite essential to have some basic parameters of the product. The following table provides information about the characteristics of 产品名称.

| Characteristic                         | Value   | Unit  |
| :-------:                              | :-----: | :---: |
| Operating Voltage                      |         | mW    |
| Power Consumption                      |         | mA    |
| Output Voltage/Current                 |         | mV/mA |
| Measurement Range                      |         |       |
| Field of View                          |         |       |
| Rate(这里可以是传感器返回数据的默认频率)  |         |       |
| Digital I/O Pins                       | 列出引脚 | -     |
| Analog I/O Pins                        | 列出引脚 | -     |
| I2C interface                          | 列出引脚 | -     |
| I2C Address(如果是IIC通信的话)          |         |       |
| SPI interface                          | 列出引脚 | -     |
| UART interface                         | 列出引脚 | -     |
| Power supply and downloading interface | Type-C  | -     |
| Dimensions                             |         | mm    |

> 在下方可以放上引脚图。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/Seeeduino-XIAO/img/Seeeduino-XIAO-pinout-1.jpg" style={{width:1000, height:'auto'}}/></div>


## Getting Started

> 如果是产品类，可以直接从下面开始：

### Equipment Installation

> 如果设备需要安装部件或者组装后才可使用，请在这里填写步骤。

### Environmental Preparation

> 如果产品对系统环境有要求，需要介绍支持系统下的环境配置，例如驱动的安装和安装包的提供等的内容。

##### install -Windows

##### install -MacOS

### Boot

> 如果设备有烧录固件的方法，请在这里提供步骤。

### Reset

> 如果设备有重置的方法，请在这里提供步骤。

:::note

    > 使用设备的注意事项也可以在这里提。

:::

## Arduino Library Overview

:::tip

If this is your first time using Arduino, we highly recommend you to refer to [Getting Started with Arduino](https://wiki.seeedstudio.com/Getting_Started_with_Arduino/).

:::

> 请大概介绍产品使用的库，比如是基于什么编写的，是否可以直接通过Arduino IDE下载？除了这种方法以外请附上GitHub的下载链接。

<div class="github_container" style={{textAlign: 'center'}}>
    <a class="github_item" href="超链接">
    <strong><span><font color={'FFFFFF'} size={"4"}> Download the Code</font></span></strong> <svg aria-hidden="true" focusable="false" role="img" className="mr-2" viewBox="-3 10 9 1" width={16} height={16} fill="currentColor" style={{textAlign: 'center', display: 'inline-block', userSelect: 'none', verticalAlign: 'text-bottom', overflow: 'visible'}}><path d="M8 0c4.42 0 8 3.58 8 8a8.013 8.013 0 0 1-5.45 7.59c-.4.08-.55-.17-.55-.38 0-.27.01-1.13.01-2.2 0-.75-.25-1.23-.54-1.48 1.78-.2 3.65-.88 3.65-3.95 0-.88-.31-1.59-.82-2.15.08-.2.36-1.02-.08-2.12 0 0-.67-.22-2.2.82-.64-.18-1.32-.27-2-.27-.68 0-1.36.09-2 .27-1.53-1.03-2.2-.82-2.2-.82-.44 1.1-.16 1.92-.08 2.12-.51.56-.82 1.28-.82 2.15 0 3.06 1.86 3.75 3.64 3.95-.23.2-.44.55-.51 1.07-.46.21-1.61.55-2.33-.66-.15-.24-.6-.83-1.23-.82-.67.01-.27.38.01.53.34.19.73.9.82 1.13.16.45.68 1.31 2.69.94 0 .67.01 1.3.01 1.49 0 .21-.15.45-.55.38A7.995 7.995 0 0 1 0 8c0-4.42 3.58-8 8-8Z" /></svg>
    </a>
</div>

### Function

Before we get started developing a sketch, let's look at the available functions of the library.

- `函数名称` —— 功能、作用、可选参数、输入输出
- 
- 
- 

### Installation

- **Method One**

Since you have downloaded the zip Library, open your Arduino IDE, click on **Sketch > Include Library > Add .ZIP Library**. Choose the zip file you just downloaded，and if the library install correct, you will see **Library added to your libraries** in the notice window. Which means the library is installed successfully.

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/Get_Started_With_Arduino/img/Add_Zip.png" style={{width:600, height:'auto'}}/></div>

<br></br>

- **Method Two**

The library manager was added starting with Arduino IDE versions 1.5 and greater (1.6.x). It is found in the 'Sketch' menu under 'Include Library', 'Manage Libraries...'

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/Get_Started_With_Arduino/img/Library.png" style={{width:800, height:'auto'}}/></div>

When you open the Library Manager you will find a large list of libraries ready for one-click install. To find a library for your product, search for the product name or a keyword such as 'k type' or 'digitizer', and the library you want should show up. Click on the desired library, and the 'Install' button will appear. Click that button, and the library should install automatically. When installation finishes, close the Library Manager.

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/Get_Started_With_Arduino/img/library_manager.png" style={{width:1000, height:'auto'}}/></div>

### Default Variables
> 请介绍出现的全局变量

### Upgrading the Package

> 介绍未来的升级方式和操作步骤。


## Arduino / XIAO Example

Now that we have our library installed and we understand the basic functions, let's run some examples for our 产品名称 to see how it behaves.

> 将重复且相同的步骤放前面。

**Step 1.** Launch the Arduino application.

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/seeed_logo/arduino.jpg" style={{width:800, height:'auto'}}/></div>

<div class="download_arduino_container" style={{textAlign: 'center'}}>
    <a class="download_arduino_item" href="https://www.arduino.cc/en/software"><strong><span><font color={'FFFFFF'} size={"4"}>Download Arduino IDE</font></span></strong>
    </a>
</div>

**Step 2.** Select your development board model and add it to the Arduino IDE.

- If you want to use **Seeed Studio XIAO SAMD21** for the later routines, please refer to **[this tutorial](https://wiki.seeedstudio.com/Seeeduino-XIAO/#software)** to finish adding.

- If you want to use **Seeed Studio XIAO RP2040** for the later routines, please refer to **[this tutorial](https://wiki.seeedstudio.com/XIAO-RP2040-with-Arduino/#software-setup)** to finish adding.

- If you want to use **Seeed Studio XIAO nRF52840** for the later routines, please refer to **[this tutorial](https://wiki.seeedstudio.com/XIAO_BLE/#software-setup)** to finish adding.

- If you want to use **Seeed Studio XIAO ESP32C3** for the later routines, please refer to **[this tutorial](https://wiki.seeedstudio.com/XIAO_ESP32C3_Getting_Started#software-setup)** to finish adding.

- If you want to use **Seeed Studio XIAO ESP32S3** for the later routines, please refer to **[this tutorial](http://wiki.seeedstudio.com/xiao_esp32s3_getting_started#software-preparation)** to finish adding.

### Demo 1 建议是使用的模块名字或者是项目名

> 示例1的功能和应用场景介绍。

#### Materials Required

> 此处是运行本示例需要的材料和购买链接

<table align="center">
	<tr>
	    <th>Name</th>
	    <th>Name</th>
	</tr>
	<tr>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    		</a>
		</div></td>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    		</a>
		</div></td>
	</tr>
	<tr>
	    <td><a href="购买链接" target="_blank"><b>GET ONE NOW</b></a></td>
	    <td><a href="购买链接" target="_blank"><b>GET ONE NOW</b></a></td>
	</tr>
</table>

```c++
//文件名

代码段

```

> 此代码可以进行怎么样的修改以完成怎么样的操作。（介绍可延伸性）介绍demo中是否有一些难以理解函数的用法，可在此加以说明。

> 贴图，效果展示。

例

Opening your serial monitor to a baud rate of 9600 should show the distance between the sensor and the object it's pointed at in both millimeters and feet. The output should look something like the below image.

### Demo 2 建议是使用的模块名字或者是项目名

> 示例2的功能和应用场景介绍。

#### Materials Required

> 此处是运行本示例需要的材料和购买链接

<table align="center">
	<tr>
	    <th>Name</th>
	    <th>Name</th>
	</tr>
	<tr>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    		</a>
		</div></td>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    		</a>
		</div></td>
	</tr>
	<tr>
	    <td><a href="购买链接" target="_blank"><b>GET ONE NOW</b></a></td>
	    <td><a href="购买链接" target="_blank"><b>GET ONE NOW</b></a></td>
	</tr>
</table>


```c++
//文件名

代码段

```

> 此代码可以进行怎么样的修改以完成怎么样的操作。（介绍可延伸性）介绍demo中是否有一些难以理解函数的用法，可在此加以说明。

> 贴图，效果展示。

例

Opening your serial monitor to a baud rate of 9600 should show the distance between the sensor and the object it's pointed at in both millimeters and feet. The output should look something like the below image.

## Python Package Overview

:::tip

If this is your first time using Raspberry Pi, we highly recommend you to refer to [Getting Started with Raspberry Pi](https://tinkergen-helper.yuque.com/tinkergen_helper/slgacm/tn0wlt).

:::

> 请在正文开始注明兼容性，测试使用的树莓派型号、系统版本等信息。还有使用的Python信息。

> 然后加一些简单的介绍作为开场。

<div class="github_container" style={{textAlign: 'center'}}>
    <a class="github_item" href="超链接">
    <strong><span><font color={'FFFFFF'} size={"4"}> Download the Code</font></span></strong> <svg aria-hidden="true" focusable="false" role="img" className="mr-2" viewBox="-3 10 9 1" width={16} height={16} fill="currentColor" style={{textAlign: 'center', display: 'inline-block', userSelect: 'none', verticalAlign: 'text-bottom', overflow: 'visible'}}><path d="M8 0c4.42 0 8 3.58 8 8a8.013 8.013 0 0 1-5.45 7.59c-.4.08-.55-.17-.55-.38 0-.27.01-1.13.01-2.2 0-.75-.25-1.23-.54-1.48 1.78-.2 3.65-.88 3.65-3.95 0-.88-.31-1.59-.82-2.15.08-.2.36-1.02-.08-2.12 0 0-.67-.22-2.2.82-.64-.18-1.32-.27-2-.27-.68 0-1.36.09-2 .27-1.53-1.03-2.2-.82-2.2-.82-.44 1.1-.16 1.92-.08 2.12-.51.56-.82 1.28-.82 2.15 0 3.06 1.86 3.75 3.64 3.95-.23.2-.44.55-.51 1.07-.46.21-1.61.55-2.33-.66-.15-.24-.6-.83-1.23-.82-.67.01-.27.38.01.53.34.19.73.9.82 1.13.16.45.68 1.31 2.69.94 0 .67.01 1.3.01 1.49 0 .21-.15.45-.55.38A7.995 7.995 0 0 1 0 8c0-4.42 3.58-8 8-8Z" /></svg>
    </a>
</div>

### Function

Before we get started developing a sketch, let's look at the available functions of the library.

- `函数名称` —— 功能、作用、可选参数、输入输出
- 
- 
- 

### Installation

#### Online one-click installation

One-click installation, quick start, what ever you call, with the single command below, we can install/update all dependencies and latest grove.py.

:::caution

If you are using Raspberry Pi with Raspberrypi OS >= Bullseye, you cannot use this command line.

:::

```sh
curl -sL https://github.com/Seeed-Studio/grove.py/raw/master/install.sh | sudo bash -s -
```

:::info

if everything goes well, you will see the following notice.

```sh
Successfully installed grove.py-0.5
#######################################################
Lastest Grove.py from github install complete   !!!!!
#######################################################
```

:::

#### Step by step installation

Besides the one-click installation, you can also install all the dependencies and latest grove.py step by step.

:::caution

If you are using Raspberry Pi with Raspberrypi OS >= Bullseye, you have to use this command line only with Python3.

:::

```sh
git clone https://github.com/Seeed-Studio/grove.py
cd grove.py
# Python3
sudo pip3 install .
```

### Dependencies

> 介绍导入的库和依赖包信息。

### Default Variables

> 请介绍出现的全局变量。

### Class

> 请介绍设置的python代码类。

### Upgrading the Package

> 介绍未来的升级方式和操作步骤。

## Raspberry Pi Example

Now that we have our library installed and we understand the basic functions, let's run some examples for our 产品名称 to see how it behaves.

> 将重复且相同的步骤放前面。

**Step 1.** 


**Step 2.** 

**Step 3.** 

### Demo 1 建议是使用的模块名字或者是项目名

> 示例1的功能和应用场景介绍。

#### Materials Required

> 此处是运行本示例需要的材料和购买链接

<table align="center">
	<tr>
	    <th>Name</th>
	    <th>Name</th>
	</tr>
	<tr>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    		</a>
		</div></td>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    		</a>
		</div></td>
	</tr>
	<tr>
	    <td><a href="购买链接" target="_blank"><b>GET ONE NOW</b></a></td>
	    <td><a href="购买链接" target="_blank"><b>GET ONE NOW</b></a></td>
	</tr>
</table>

```c++
//文件名

代码段

```

> 此代码可以进行怎么样的修改以完成怎么样的操作。（介绍可延伸性）介绍demo中是否有一些难以理解函数的用法，可在此加以说明。

> 贴图，效果展示。

### Demo 2 建议是使用的模块名字或者是项目名

> 示例2的功能和应用场景介绍。

#### Materials Required

> 此处是运行本示例需要的材料和购买链接

<table align="center">
	<tr>
	    <th>Name</th>
	    <th>Name</th>
	</tr>
	<tr>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    		</a>
		</div></td>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    		</a>
		</div></td>
	</tr>
	<tr>
	    <td><a href="购买链接" target="_blank"><b>GET ONE NOW</b></a></td>
	    <td><a href="购买链接" target="_blank"><b>GET ONE NOW</b></a></td>
	</tr>
</table>


```c++
//文件名

代码段

```

> 此代码可以进行怎么样的修改以完成怎么样的操作。（介绍可延伸性）介绍demo中是否有一些难以理解函数的用法，可在此加以说明。

> 贴图，效果展示。
例


## Troubleshooting

> 我们在收到研发的内容之后，可以记录下自己在执行demo中遇到的问题，然后记录在此，帮助用户少踩坑。

## Resources

- **[文件类型]** [文件名称]("链接")
- **[文件类型]** [文件名称]("链接")
- **[文件类型]** [文件名称]("链接")

## Tech Support

Please submit any technical issues into our [forum](https://forum.seeedstudio.com/).

<p style={{textAlign:'center'}}><a href="https://www.seeedstudio.com/act-4.html?utm_source=wiki&utm_medium=wikibanner&utm_campaign=newproducts" target="_blank"><img src="https://files.seeedstudio.com/wiki/Wiki_Banner/new_product.jpg" /></a></p>

## 附录

### HTML表格

<table align="center">
	<tr>
	    <th> </th>
	    <th colspan="4">Button Header</th>
	</tr>
	<tr>
	    <td rowspan="6"><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/reComputer-Jetson-Nano/4_3.jpg" style={{width:600, height:'auto'}}/></div></td>
	    <td align="center">1</td>
	    <td align="center">PWR BTN</td>
	    <td align="center">7</td>
	    <td align="center">AUTO ON</td>
	</tr>
	<tr>
	    <td align="center">2</td>
	    <td align="center">GND</td>
	    <td align="center">8</td>
	    <td align="center">DIS</td>
	</tr>
	<tr>
	    <td align="center">3</td>
	    <td align="center">FC REC</td>
	    <td align="center">9</td>
	    <td align="center">UART TXD</td>
	</tr>
	<tr>
	    <td align="center">4</td>
	    <td align="center">GND</td>
	    <td align="center">10</td>
	    <td align="center">UART RXD</td>
	</tr>
	<tr>
	    <td align="center">5</td>
	    <td align="center">SYS RET</td>
	    <td align="center">11</td>
	    <td align="center">LED +</td>
	</tr>
    <tr>
	    <td align="center">6</td>
	    <td align="center">GND</td>
	    <td align="center">12</td>
	    <td align="center">LED -</td>
	</tr>
</table>

### 注释

<!--这是注释-->

### HTML表格超链接

<div class="get_one_now_container" style={{textAlign: 'center'}}>
    <a class="get_one_now_item" href="购买链接">
            <strong><span><font color={'FFFFFF'} size={"4"}> Get One Now 🖱️</font></span></strong>
    </a>
</div>

### HTML表格文字超链接

<a href="购买链接" target="_blank"><b>GET ONE NOW</b></a>


### 锚点

<span id="jump1">Placeholders</span>

[**Getting Started -- Special notes on command line -- Placeholders**](#jump1)

### 文字颜色高亮

export const Highlight = ({children, color}) => (
  <span
    style={{
      backgroundColor: color,
      borderRadius: '2px',
      color: '#fff',
      padding: '0.2rem',
    }}>
    {children}
  </span>
);

上面这段话应该放在需要高亮Wiki的最前面。

<Highlight color="#25c2a0">Docusaurus green</Highlight> and <Highlight color="#1877F2">Facebook blue</Highlight> are my favorite colors.


<span style={{backgroundColor: 'red'}}>Foo</span>

## Tabs

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="apple" label="Apple" default>
    This is an apple 🍎
  </TabItem>
  <TabItem value="orange" label="Orange">
    This is an orange 🍊
  </TabItem>
  <TabItem value="banana" label="Banana">
    This is a banana 🍌
  </TabItem>
</Tabs>

## 警告与提示

:::note

Some **content** with _Markdown_ `syntax`. Check [this `api`](#).

:::


:::tip

Some **content** with _Markdown_ `syntax`. Check [this `api`](#).

:::


:::info

Some **content** with _Markdown_ `syntax`. Check [this `api`](#).

:::


:::caution

Some **content** with _Markdown_ `syntax`. Check [this `api`](#).

:::


:::danger

Some **content** with _Markdown_ `syntax`. Check [this `api`](#).

:::



