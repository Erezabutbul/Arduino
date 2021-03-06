Servo
===

Send a Servo config message to configure a pin a servo. Then use the `SET_PIN_MODE`
message to attach/detach Servo support to a pin. This saves space in the protocol
by reusing the `SET_PIN_MODE` message, but the host software implementation
could have a different interface, e.g. Arduino's `attach()` and `detach()`.

The `SERVO_CONFIG` message can be sent at any time to chang the settings.

Added in Firmata protocol version 2.1.0.

Servo config
```
// minPulse and maxPulse are 14-bit unsigned integers
0  START_SYSEX          (0xF0)
1  SERVO_CONFIG         (0x70)
2  pin number           (0-127)
3  minPulse LSB         (0-6)
4  minPulse MSB         (7-13)
5  maxPulse LSB         (0-6)
6  maxPulse MSB         (7-13)
7  END_SYSEX            (0xF7)
```

*This is just the standard `SET_PIN_MODE` message:*
Set digital pin mode
```
0  set digital pin mode (0xF4) (MIDI Undefined)
1  pin number           (0-127)
2  state                (SERVO, 4)
```

Write to servo, servo write is performed if the pin mode is SERVO
```
0  ANALOG_MESSAGE       (0xE0-0xEF)
1  value LSB
2  value MSB
```

If the pin number is higher than 15, or if the value to write to the servo is
greater than 14 bits, then the Extended Analog message can be used in place
of the standard `ANALOG_MESSAGE`:

```
0  START_SYSEX              (0xF0)
1  extended analog message  (0x6F)
2  pin                      (0-127)
3  bits 0-6                 (least significant byte)
4  bits 7-13                (most significant byte)
... additionaly bytes may be sent if more bits are needed
N  END_SYSEX                (0xF7)
```
