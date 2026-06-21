Pin 5 (GPIO3)  ────────┐
                       [ BUTTON ]
Pin 9 (GND)    ─────────────────┘

sudo nano /boot/firmware/config.txt
`dtoverlay=gpio-shutdown,gpio_pin=3,active_low=1`