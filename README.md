# lib_RP2040-TM1638
TM1638 library for RP2040 using SDK-C/C++ 

![image](https://github.com/user-attachments/assets/80a675f8-adf8-47e4-8c55-9b945ca06cb6)

This library was created to control modules based on the TM1638 chip,
featuring a 7-segment display, LEDs, and buttons.

1. Internal (Private) Functions
  
  send_byte(data)
  - Sends a byte to the TM1638, bit by bit, using the clock (_clk) and data (_dio) GPIO pins.
  - Required for the bit-banged serial communication with the module.
  
  command(cmd)
  - Sends a command to the TM1638 by toggling the STB pin before and after transmission.
  - Used for all control operations (brightness, power on/off, etc.).
  
  update()
  - Updates the display and LEDs by sending the contents of _segments and _led_state buffers.
  - Synchronizes software state with the hardware module.
  
  encode_char(c)
  - Converts characters ('0'-'9', 'A'-'Z', 'a'-'z', space, '-', '*') into 7-segment codes.
  - Enables text display on the 7-segment module.
  
  read_byte()
  - Reads a byte sent from the TM1638 via the data pin.
  - Serves as the basis for button state reading.
  
  read_keys()
  - Reads and returns a bitmask representing pressed buttons.
  - Aggregates multiple byte reads into a single mask.
  
  2. Public Functions (API)
  
  tm1638_init(brightness)
  - Initializes GPIO pins, clears display memory, and sets brightness (0-7).
  - Must be called before any other library functions.
  
  tm1638_clear()
  - Clears the display and turns off all LEDs.
  - Ideal to start fresh on the module.
  
  tm1638_show_string(string, pos)
  - Displays a string starting at position pos (0-7).
  - Converts and loads characters into the segment buffer.
  
  tm1638_set_power(on)
  - Turns the display on (true) or off (false).
  - Controls visibility without losing buffer data.
  
  tm1638_set_brightness(brightness)
  - Adjusts the display brightness (0-7) at runtime.
  
  tm1638_set_led(pos, on)
  - Turns an individual LED on or off at position pos (0-7).
  
  tm1638_set_leds(leds)
  - Sets the state of all LEDs using an array of 8 values.
  
  tm1638_set_leds_mask(mask)
  - Controls all LEDs using an 8-bit binary mask.
  
  tm1638_read_keys_mask()
  - Reads button states and returns an 8-bit mask (higher bits = buttons on the right).
  
  tm1638_get_keys(key)
  - Reads button states and fills key[8] array with 1 (pressed) or 0 (not pressed).
  
  3. Strategies and Observations
  
  - Unified buffer: Maintains _segments and _led_state in RAM and updates the module via update().
  - Encoding table: _COD[] translates ASCII characters into segment codes.
  - Reading modes: Provides both bitmask and array-based button reading methods.
  
  Summary:
  An efficient and flexible library for TM1638 modules on Raspberry Pi Pico, offering
  display control, LED management, and button reading through a clear and concise API.
