## Serial API Reference

### Command and Response Prefixes

The serial protocol uses simple prefixes to clearly distinguish messages:

- **Commands sent from the PC to the Arduino must begin with `#`**
- **Messages sent from the Arduino back to the PC begin with `@`**

This makes it easy to separate input from output when logging or parsing serial data.

---

## Valid Commands

Below is a list of all valid commands, their parameters, and the expected Arduino responses.

---

### `#SETTINGSLEDRGB <R> <G> <B>`

Sets the LED colour settings (does not automatically turn them on).

Parameters:  

- `R`, `G`, `B` — values from 0–255

**Expected response:**

```
@LEDRGB = R G B
```

Example:

```
#SETTINGSLEDRGB 128 64 0
```

---

### `#LEDON`

Turns the LEDs on using the current saved LED settings.

**Expected response:**

```
@LEDON
```

Example:

```
#LEDON
```

---

### `#LEDOFF`

Turns the LEDs off.

**Expected response:**

```
@LEDOFF
```

Example:

```
#LEDOFF
```

---

### `#SETALL <R> <G> <B> <gainExt> <gainSca> <intTimeExt> <intTimeSca>`

Sets LED colour, detector gains, and detector integration times in a single command, and turns the LEDs on.

Valid gains:  
`1`, `4`, `16`, `60`

Valid integration times (ms):  
`24`, `50`, `60`, `120`, `240`, `480`, `600`

**Expected responses (in this order):**

```
@LEDSETTINGS = R G B
@LEDON
@GAINSETTINGS = <gainExt> <TCS34725_GAIN_XX> <gainSca> <TCS34725_GAIN_XX>
@INTTIMESETTINGS = <intTimeExt> <TCS34725_INTEGRATIONTIME_XXXMS> <intTimeSca> <TCS34725_INTEGRATIONTIME_XXXMS>
```

If invalid gain values are used:

```
@Invalid gain setting: X
```

If invalid integration times are used:

```
@Invalid integ time setting: X
```

Example:

```
#SETALL 100 20 200 16 4 120 240
```

---

### `#READEXT`

Reads raw RGB values from the Extinction (Ext) sensor.

**Expected response:**

```
@EXT = r g b
```

Example:

```
#READEXT
```

---

### `#READSCA`

Reads raw RGB values from the Scattering (Sca) sensor.

**Expected response:**

```
@SCA = r g b
```

Example:

```
#READSCA
```

---

### `#CHECKSERIAL`

Checks that serial communication is functioning.

**Expected response:**

```
@SERIALOK
```

Example:

```
#CHECKSERIAL
```

---

### `#RESET`

Performs a soft reset of the Arduino using the watchdog timer.

**Expected response:**  
No immediate response (device resets).  
After reboot, you will see:

```
@SETUPSTARTING
@SETUPCOMPLETE
```

Example:

```
#RESET
```

---
