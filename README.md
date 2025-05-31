# esphome-watchman-sonic
esphome oil tank level sensor for the watchman sonic sensor using a CC1101 rf receiver module with esp32c3
I have the basic Kingspan watchman sonic sensor, and I wanted a simple way of monitoring the oil level within Home Assistant. This project uses an ESP32C3 supermini with oled display module of the type readily available on aliexpress, along with a CC1101 module also available from aliexpress.

![modules](https://github.com/user-attachments/assets/7d3a3b50-5dce-47b3-904b-8e337b95ad3d)

Although the esp module has an inbuilt oled display, I haven't yet tried to show the oil level on the display - maybe an addition for the future.

Connection between the two modules is straight forward:
ESP32                  CC1101
3v3                    VCC
GND                    GND
 3                     GDD0
 9                     CSN
 6                     SCK
 7                    MOSI
 5                    MISO
 2                    MISO

 Pin 2 of the esp32 is connected to the MISO signal, and this is used to check the ready status of the CC1101.

 The software is located within a directory watchman_sonic and should be installed within the components directory of your esphome directory.

 YAML extract:
 spi:
   clk_pin: 6
   miso_pin: 5
   mosi_pin: 7
external_components:
  - source: components
    components: watchman_sonic
sensor:
  - platform: watchman_sonic
    gdo0_pin: 3
    led_pin: 8
    ready_pin: 2
    cs_pin: 9
    sensor_code: xxxxx
    tank_radius: xxx
    tank_height: xxx
    tank_litres: {name: Oil tank}

tank radius is the radius of the tank in mm (assumes circular tank) and the height is also in mm. The sensor code is the unique identifier of the watchman sensor and can be gleaned from the logs. Note that the sensor seems to broadcasts every 17mins or so.
