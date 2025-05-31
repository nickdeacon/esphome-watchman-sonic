# ESPHOME EXTERNAL COMPONENT FOR KINGSPAN WATCHMAN SONIC OIL SENSOR
This is an esphome oil tank level sensor for the watchman sonic sensor using a CC1101 rf receiver module with esp32c3. It provides a simple indication of the level of oil within the oil tank, within home assistant. This is adequate to provide an indication of when to refill.
I have the basic Kingspan watchman sonic sensor, and in researching for this project, I could not find an existing project that did what I wanted. 

This project uses an ESP32C3 supermini with oled display module of the type readily available on aliexpress, along with a CC1101 module also available from aliexpress. Both can be obtained for a few pounds. It requires esphome to be installed within HA, and the software for this project is loaded as an external component.

My Home Assistant runs on a Raspberry pi and the software resides within the esp/components directory in a subdirectory 'watchman_sonic'

The hardware necessary for this project is simple, an ESP32C3 supermini module, and a CC1101 rf module to receive the 433MHZ signal from the oil watchman unit. No other components are required, and it is powered through the usbc connector on the esp32 module.



![modules](https://github.com/user-attachments/assets/7d3a3b50-5dce-47b3-904b-8e337b95ad3d)

Although the esp module has an inbuilt oled display, I haven't yet tried to show the oil level on the display - maybe an addition for the future.
Connection between the two modules is straight forward:

-----------------
|ESP32 |  CC1101 |
|------|---------|
|3v3   |     VCC |
|GND   |     GND |
| 3    |    GDD0 |
| 9    |     CSN |
| 6    |     SCK |
| 7    |    MOSI |
| 5    |    MISO |
| 2    |    MISO |
------------------

 Pin 2 of the esp32 is connected to the MISO signal, and this is used to check the ready status of the CC1101 during spi data transfer.

An example portion of yaml code:


 The software is located within a directory watchman_sonic and should be installed within the components directory of your esphome directory.

 An example portion of yaml code:

```YAML
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

```

tank_radius is the radius of the tank in mm (assumes circular tank vertically aligned) and the height is also in mm. The sensor will publish an estimate of tank contents in litres. If you have a different tank setup, then you will need to modify the code to get an accurate figure for tank contents. For the basic watchman sonic unit, the depth reading is 8 bit, so accuracy is limited.

The sensor code is the unique identifier of the watchman sensor and can be gleaned from the logs. Note that the sensor seems to broadcasts every 17mins or so, however placing a magnet near to the spot on the side of the unit will cause it to transmit repeatedly for several minutes.
