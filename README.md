# Li-Fi image link

Sending a picture from one computer to another with light.

This is a student team project from the Best Information Transmission Laboratory at National Taiwan Ocean University. The code was written between July and September 2022 and is kept here as it was; it is not maintained.

A video of the link working is in [Williamochi/project](https://github.com/Williamochi/project).

## How it works

1. **Compress (Python).** The image is cut into 8×8 blocks and coded the way baseline JPEG does it: DCT, quantization, zigzag scan, DPCM for the DC terms, run-length coding for the AC terms, then Huffman coding.
2. **Protect (Python).** A Hamming code adds parity bits so that single-bit errors can be corrected at the receiver.
3. **Send (Arduino).** The bits go to an Arduino over USB serial. The Arduino switches a light source on and off, one bit every 100 ms.
4. **Receive (Arduino).** A second Arduino reads a light sensor, compares each reading with a threshold to recover the bit, and passes the bytes to the second computer.
5. **Rebuild (Python).** The receiver undoes steps 2 and 1 and shows the picture. PSNR measures how close it is to the original.

At 100 ms per bit the link carries about 10 bits per second, so it is a demonstration of the whole chain rather than a fast link.

## What is in this repository

| Path | Contents |
| --- | --- |
| `Jpack/` | The image pipeline: JPEG-style encoder and decoder, the Hamming code, a simulated noisy channel for testing it, and a script that reads from the serial port. `main.py` runs encode and decode on a test image. |
| `JPEG/`, `hihi-pancake/` | Earlier versions and experiments of the same pipeline. |
| `Li-Fi/work_ver_1/` | The Arduino sketches for the transmitter (`transfer`) and the receiver (`receive`). |
| `Li-Fi/` (other folders) | Arduino tests with different light sources and sensors: LED, laser, photoresistor and a BH1750 light sensor. |
| `tramsmitt_pyserial.py`, `receive_pyserial.py` | Short scripts that write to and read from the serial ports. |
| `Li-Fi/SaadOjo-DIY_Li-Fi-6aaab2f/`, `Li-Fi/LiFi_arduino-main/` | Example projects by other authors that we studied. They are not our work and are kept unchanged. |

## Running it

You need Python 3 with `numpy`, `opencv-python`, `sympy`, `matplotlib` and `pyserial`, and the Arduino IDE for the sketches.

The serial port names are written into the scripts (for example `COM3`, `COM4` and `COM8`). Change them to the ports your Arduinos use.

To try the compression pipeline without any hardware, run `python main.py` inside `Jpack/`.

## Team

Four students worked on this; see the contributors list. My part was measuring the optical channel to characterize path loss, and analysing Huffman coding for compressing the image data.
