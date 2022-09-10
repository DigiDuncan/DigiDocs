# DigiOSC
**This project is a work in progress.**

Quickly and easily fire VRChat OSC-bound parameters based on various input methods.

## Supported Input Methods

* XInput (game controllers)
    - Buttons
    - Analog inputs
    - Battery level
* Keyboard‡
* MIDI Controller
    - Notes (digital or analog)
    - Control
    - Channel (latest note)
    - Pitchwheel
* File read
    - Text file

## Limitations
* You can not bind two inputs to the same parameter.
    - This is because you can reasonably press any combination of buttons at once, so overlap is basically guaranteed.
* Input types are restricted in what data they can send.
    - Table below.

## Input Types and the Data They Send
| Input Type                  | Data Type | Data Range   |
|-----------------------------|-----------|--------------|
| XInput: Button              | bool      | False-True   |
| XInput: Analog\*            | float     | -1.0-1.0†    |
| XInput: Battery             | int       | 0-3, -1\*\*  |
| Keyboard‡                   | bool      | False-True   |
| MIDI: Digital               | bool      | False-True   |
| MIDI: Analog                | float     | 0.0-1.0      |
| MIDI: Control               | int       | 0-127        |
| MIDI: Pitchwheel            | float     | -1.0-1.0     |
| MIDI: Channel (Latest Note) | int       | 0-127        |
| MIDI: Channel (Program)     | int       | 0-127        |
| File: Text (int)            | int       | -128-127     |
| File: Text (float)          | float     | -1.0-1.0     |
| File: Text (bool)           | bool      | False-True   |

\* This includes `LEFT_STICK_X`, `LEFT_STICK_Y`, `RIGHT_STICK_X`, `RIGHT_STICK_Y`, `LEFT_TRIGGER`, and `RIGHT_TRIGGER`.  

\*\* Wired controllers output -1.

† `STICK`s return -1.0-1.0, `TRIGGER`s return 0.0-1.0.  

‡ Keyboard input handlers require a `time` parameter, an amount of seconds from the initial keypress to release
the input. If it's not provided, it defaults to 1. This is due to limitations with the way I'm reading keyboard events, and will be fixed in a later version.
They are also pretty limited in general. This will be solved later as well, but currently, they can only accept *characters,* not arbitrary key combos. \[A\] works, \[SHIFT + A\] works, \[CTRL + A\] does not (as that does not produce a character.)

§ File read doesn't validate input. `bool` will return the truthiness of the text in the file (so basically if there's text or not), `int` will attempt to cast the text to `int` and clamp it between -128 and 127, and `float` will attempt to cast the text to `float` and clamp it between -1.0 and 1.0.

## Other Notes
* Configuration files are stored in `%localappdata%/DigiDuncan/DigiOSC/configs`.