# Deploy Guide — K_PYMATA4
**Tier:** TIER_9_ROBOTICS_IOT | **Stack:** Python 3.11, pymata4 2.x, Arduino (StandardFirmata), PAX 27B, AIOSS_FORMAT
**Air-gap capable after initial setup.**

## Prerequisites
Python 3.11+, pymata4 2.x, Arduino with StandardFirmata loaded. USB connection.

## Environment
Arduino Uno/Mega/Nano with StandardFirmata firmware. USB cable. 4GB RAM.

## AIOSS Integration
```bash
aioss init --module K_PYMATA4 --output ./k_pymata4.aioss
aioss append --chain ./k_pymata4.aioss --payload ./output.bin --module K_PYMATA4
aioss verify --chain ./k_pymata4.aioss
```

## Air-Gap Setup
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="K_PYMATA4",
    aioss_chain="./K_PYMATA4.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./K_PYMATA4.aioss --verbose
python -m K_PYMATA4.tests.smoke
```
