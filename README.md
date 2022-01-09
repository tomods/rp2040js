# rp2040js

Raspberry Pi Pico Emulator for the [Wokwi Simulation Platform](https://wokwi.com). It blinks, runs Arduino code, and even the MicroPython REPL!

## Online examples

If you are just looking to play around with the Raspberry Pi Pico Simulator, check out the Wokwi Simulator:

* [Raspberry Pi Pico Traffic Light](https://wokwi.com/arduino/projects/297322571959894536)
* [LCD1602 Hello World](https://wokwi.com/arduino/projects/297323005822894602)
* [MicroPython Blink](https://wokwi.com/arduino/projects/300504213470839309)
* [MicroPython 7-Segment Counter](https://wokwi.com/arduino/projects/300210834979684872)

For more information, take a look at the [wokwi-pi-pico docs](https://docs.wokwi.com/parts/wokwi-pi-pico) and the [Pi Pico MicroPython Guide](https://docs.wokwi.com/guides/micropython).

If you want to develop your own application using the Raspberry Pi Pico simulator, the following examples may be helpful:

* [Blink LEDs with RP2040js, from scratch](https://stackblitz.com/edit/rp2040js-blink?file=index.ts) - Press "Run" and patiently wait for the code to compile ;-)

## Run the demo project

### Native code
You'd need to get `hello_uart.hex` by building it from the [pico-examples repo](https://github.com/raspberrypi/pico-examples/tree/master/uart/hello_uart), then copy it to the rp2040js root directory and run:

```
npm install
npm start
```

### MicroPython code
To run the MicroPython demo, first download [rp2-pico-20210902-v1.17.uf2](https://micropython.org/resources/firmware/rp2-pico-20210902-v1.17.uf2), place it in the rp2040js root directory, then run:

```
npm install
npm run start:micropython
```

and enjoy the MicroPython REPL! Quit the REPL with Ctrl+X.

You can replace rp2-pico-20210902-v1.17.uf2 with any recent MicroPython or CircuitPython release built for the RP2040.

With MicroPython – and probably also CircuitPython – you can use the filesystem on the Pico. This becomes useful as more than one script file is used in your code. Just put a [LittleFS](https://github.com/littlefs-project/littlefs) formatted filesystem image called `littlefs.img` into the rp2040js root directory, and your `main.py` will be automatically started from there.

A simple way to create a suitable LittleFS image containing your script files is outlined in [create_littlefs_image.py](https://github.com/tomods/GrinderController/blob/358ad3e0f795d8cc0bdf4f21bb35f806871d433f/tools/create_littlefs_image.py).
So, using [littlefs-python](https://pypi.org/project/littlefs-python/), you can do the following:
```python
from littlefs import LittleFS
files = ['your.py', 'files.py', 'here.py', 'main.py']
output_image = 'output/littlefs.img'  # symlinked/copied to rp2040js root directory
lfs = LittleFS(block_size=4096, block_count=352, prog_size=256)
for filename in files:
    with open(filename, 'rb') as src_file, lfs.open(filename, 'w') as lfs_file:
        lfs_file.write(src_file.read())
with open(output_image, 'wb') as fh:
    fh.write(lfs.context.buffer)
```

Other ways of creating LittleFS images can be found [here](https://github.com/wokwi/littlefs-wasm) or [here](https://github.com/littlefs-project/littlefs#related-projects).

Currently, the filesystem is not writeable, as the SSI peripheral required for flash writing is not implemented yet. If you're interested in hacking, see the discussion in https://github.com/wokwi/rp2040js/issues/88 for a workaround.

## Learn more

- [Live-coding stream playlist](https://www.youtube.com/playlist?list=PLLomdjsHtJTxT-vdJHwa3z62dFXZnzYBm)
- [Hackaday project page](https://hackaday.io/project/177082-raspberry-pi-pico-emulator)

## License

Released under the MIT licence. Copyright (c) 2021, Uri Shaked.
