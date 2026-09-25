# Analog Input, PWM, and DAC

## Example 3: Read a potentiometer
| Position | Raw (0–4095) | Millivolts |
|----------|--------------|------------|
| Min      | 0            | 142        |
| 1/4      | 1023         | 966        |
| 1/2      | 2047         | 1790       |
| 3/4      | 3069         | 2590       |
| Max      | 4095         | 3134       |

The ADC readings from Example 3 mostly matched expectations, but they weren't perfectly linear. At the lowest position the reading was 142 mV instead of 0, and at max it was 3134 mV instead of the full 3300 mV. This is normal for the ESP32's ADC with 11db attenuation, which is known to be a little off at the very top and bottom of its range.

## Example 4: Adjust LED brightness with a potentiometer
| Position | Raw | Predicted Duty (0–255) |
|----------|-----|------------------------|
| Min      | 0   | 0                      |
| 1/4      | 1023 | 63                   |
| 1/2      | 2047 | 127                  |
| 3/4      | 3069 | 191                  |
| Max      | 4095 | 255                  |

The PWM duty values from Example 4 matched the predictions exactly, since duty is just calculated from the raw ADC value using map(). There's no real "measurement" involved, just math, so there wasn't much room for error.

## Example 5: Measure changing DAC output
| Code | Predicted V (code/255 × 3.3V) | Measured V |
|------|-------------------------------|------------|
| 0    | 0.00 V                        | 66.7 mV    |
| 64   | 0.83 V                        | 855 mV     |
| 128  | 1.66 V                        | 1.631 V    |
| 192  | 2.48 V                        | 2.422 V    |
| 255  | 3.30 V                        | 3.195 V    |

The DAC voltages from Example 5 came out close to predicted but slightly low across the board. At code 0 the measured value was 66.7 mV instead of 0 V, and at code 255 it was 3.195 V instead of 3.30 V. This is also expected, since the ESP32's DAC doesn't hit 0V or the full reference voltage exactly, so a small offset and gain error is normal.

### Overall, the ADC input isn't perfectly linear, PWM duty is just math derived from that ADC value with no separate error source, and the DAC output tracks predictions closely but with a small expected offset. It's also worth noting that the PWM numbers are duty-cycle values, not voltage measurements, actual measured DAC voltage only comes from Example 5.
