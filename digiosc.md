# DigiOSC
Quickly and easily fire VRChat OSC-bound parameters based on various input methods.

If ran from the terminal, you can use the `-d` flag to run DigiOSC in debug mode.

## How to Add an Input (and get it working on your avatar)

1. Add an animation to your avatar controlled by a parameter.
    * There's a great tutorial for that [here](https://www.youtube.com/watch?v=XqtSg6_W07Y) or use the amazing [AV3Creator](https://rafacasari.gumroad.com/l/av3creator) by *rafacasari.*
2. Open up DigiOSC. If you haven't yet, create a new configuration by selecting the *New Configuration* option.
3. Edit the configuration you want to add an input to by selecting *Edit Configuration* from the main menu, and then the configuration you want.
4. Choose *Add Input.*
5. Select the type of input you'd like to add.
    * **XInput Button** is a single button on an XInput-enabled game controller.
    * **XInput Analog** is an axis or analog input on an XInput controller, like a stick or a trigger.
    * **XInput Battery** corresponds to the battery level of your controller.††
    * **Keyboard** corresponds to a button press on the keyboard. The DigiOSC window must be in focus.
    * **MIDI Note** is a single note hit on a MIDI controller. Digital will just tell you if the note is on/off, but analog will read the pressure it was hit with.
    * **MIDI Control** is a control changing on a MIDI controller.
    * **MIDI Channel** reads the current state of a MIDI channel. **Last Note** is the latest note that was pressed, **Current Program** is the current program (instrument) the channel is set to.
    * **MIDI Pitchwheel** is the current state of the pitch bend wheel on your MIDI controller.
    * **File** reads the contents of a file, and if it's valid and changed, sends that data to the parameter. **Bool* just reads if the file is empty or not, but **Int** must be a valid whole number, and **Float** must be a valid decimal number.
    * See [this table](#Input_Types_and_the_Data_They_Send) for more info about what data you can expect from each type.
6. Once you're happy with what inputs you have set up, exit to the main menu and select *Run OSC Server*.
7. Now, just keep DigiOSC open as you boot VRChat. Make sure OSC is enabled on your avatar (the option's in the raidal menu) and the inputs you set up should now control the parameters you set!

This can be a very basic or a very powerful tool. Please, show me the things you create with this!

## Supported Input Methods

* XInput (game controllers)
    - Buttons
    - Analog inputs
    - Battery level
* Keyboard‡
* MIDI Controller
    - Notes (digital or analog)
    - Control
    - Channel (latest note or current program)
    - Pitchwheel
* File read
    - Text file

## Limitations
* You can not bind two parameters to the same input.
    - There are ways to solve this problem via Unity.
* Input types are restricted in what data they can send.
    - Table below.
* You may need to install a MIDI loopback driver to get MIDI working on Windows.
    - I use [loopMIDI](https://www.tobias-erichsen.de/software/loopmidi.html) for the driver, and [VMPK](https://vmpk.sourceforge.io/) as a virtual MIDI device.

## Input Types and the Data They Send
| Input Type                  | Data Type | Data Range   |
|-----------------------------|-----------|--------------|
| XInput: Button              | bool      | False-True   |
| XInput: Analog\*            | float     | -1.0-1.0†    |
| XInput: Battery††           | int       | 0-3, -1\*\*  |
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

†† As of now, the battery is only read the controller on slot 1.

‡ Keyboard input handlers require a `time` parameter, an amount of seconds from the initial keypress to release
the input. If it's not provided, it defaults to 1. This is due to limitations with the way I'm reading keyboard events, and will be fixed in a later version.
They are also pretty limited in general. This will be solved later as well, but currently, they can only accept *characters,* not arbitrary key combos. \[A\] works, \[SHIFT + A\] works, \[CTRL + A\] does not (as that does not produce a character.)

§ File read doesn't validate input. `bool` will return the truthiness of the text in the file (so basically if there's text or not), `int` will attempt to cast the text to `int` and clamp it between -128 and 127, and `float` will attempt to cast the text to `float` and clamp it between -1.0 and 1.0.

## Other Notes
* Configuration files are stored in `%localappdata%/DigiDuncan/DigiOSC/configs`.
