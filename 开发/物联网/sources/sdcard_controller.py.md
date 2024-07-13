```python
import machine
import sdcard
import uos

sd = None


def mount_sdcard():
    global sd

    # Assign chip select (CS) pin (and start it high)
    cs = machine.Pin(18, machine.Pin.OUT)

    # Intialize SPI peripheral (start with 1 MHz)
    spi = machine.SPI(1,
                      baudrate=1000000,
                      polarity=0,
                      phase=0,
                      bits=8,
                      firstbit=machine.SPI.MSB,
                      sck=machine.Pin(12),
                      mosi=machine.Pin(16),
                      miso=machine.Pin(14))

    # Initialize SD card
    sd = sdcard.SDCard(spi, cs)

    # Mount filesystem
    vfs = uos.VfsFat(sd)
    uos.mount(vfs, "/sd")
