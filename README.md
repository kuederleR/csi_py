# csi-py

A Python library for reading and parsing Channel State Information (CSI) data from serial ports. This library is designed to work with ESP32-based CSI tools, enabling real-time analysis of Wi-Fi signal characteristics.

## Features

- Real-time CSI data parsing from serial ports
- Dynamic filtering of static subcarriers
- Easy access to CSI amplitude, phase, and metadata
- Simple and intuitive API

## Installation

```bash
pip install csi-py
```

## Usage

Here is a simple example of how to use `csi-py` to read and print the RSSI (Received Signal Strength Indication) from a serial port:

```python
import sys
from csi_py.csireader import CSIReader

def print_rssi(data):
    print(f"RSSI: {data.meta['rssi']}")

def main():
    if len(sys.argv) < 2:
        print("Usage: python main.py <serial_port>")
        sys.exit(1)
    port = sys.argv[1]
    reader = CSIReader(port, data_callback=print_rssi, rate=20)
    try:
        reader.run()
    except KeyboardInterrupt:
        reader.stop()
        print("Stopped.")

if __name__ == "__main__":
    main()
```

## API Reference

### `CSIReader(port, data_callback=None, rate=None, detect_static_samples=10, dynamic_filter=True)`

- `port`: The serial port to read from (e.g., `/dev/ttyUSB0`).
- `data_callback`: A function to be called with the parsed CSI data. The function will receive a `CSIData` object as an argument.
- `rate`: The rate in Hz at which to process the CSI data.
- `detect_static_samples`: The number of samples to use for detecting static subcarriers.
- `dynamic_filter`: A boolean indicating whether to enable the dynamic subcarrier filter.

### `run()`

Starts reading and parsing CSI data from the serial port. This is a blocking method.

### `stop()`

Stops the CSI reader.

### `CSIData`

A class that holds the parsed CSI data.

- `amplitude`: A NumPy array containing the amplitude of the CSI data.
- `phase`: A NumPy array containing the phase of the CSI data.
- `raw`: The raw CSI data.
- `meta`: A dictionary containing metadata about the CSI data, such as RSSI, MAC address, and channel.

## Examples

For more detailed examples, please see the `examples` directory.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
