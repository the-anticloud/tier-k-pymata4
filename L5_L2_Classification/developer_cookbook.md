# Developer Cookbook — K_PYMATA4
**Stack:** Python 3.11, pymata4 2.x, Arduino (StandardFirmata), PAX 27B, AIOSS_FORMAT
**Domain:** PyMata4: Arduino/StandardFirmata interface for Anticloud hardware prototyping

## Arduino sensor reading
```python
from k_pymata4 import AnticloudArduino

arduino = AnticloudArduino(
    com_port="COM4",
    pax_model="./pax-27b-q4.gguf",
    aioss_chain="./pymata4.aioss"
)

# Read analog sensors
arduino.set_pin_mode_analog_input(pin=0, callback=None)
distance_mm = arduino.analog_read(pin=0)

# PAX interpretation
interp = arduino.pax_interpret(
    sensor_type="ultrasonic_distance",
    value=distance_mm,
    context="robot navigation"
)
print(f"Distance: {distance_mm}mm — {interp.assessment}")
```

## AIOSS Chain Append
```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()
```
