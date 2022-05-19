# Objective
The purpose of this project is to create a mouse and keyboard emulator to interface with the PS Connect app for use with MacOS. 

# Reviving an archived project "ShockEmu"
Original project is archived and not active [here](https://github.com/daeken/ShockEmu). Implementing the open pull request by [willoftw](https://github.com/willoftw) that will probably never be merged from the dead repository. This version of the code is verified to work with macOS Catalina 10.15.6 with PS Connect and a disk-edition PS5.

[![macos-catalina](https://img.shields.io/badge/macos-catalina-brightgreen.svg)](https://www.apple.com/macos/catalina-preview)
[![macos-mojave](https://img.shields.io/badge/macos-mojave-brightgreen.svg)](https://www.apple.com/lae/macos/mojave)

ShockEmu requires the OS X command line development tools to be installed.  [Installation steps](https://macpaw.com/how-to/install-command-line-tools#:~:text=Go%20to%20Terminal%20in%20%2FApplications%2FUtilities%2F.&text=In%20the%20same%20way%20when,Select%20confirm%20by%20clicking%20Install.)
# Key Mapping
`only_keyboard.se` goes like this:

	git clone https://github.com/bji219/ShockEmu_PS5.git
	./build.sh <filename>.se
	./run.sh
[Key Mapping](https://github.com/daeken/ShockEmu/pull/4/files#diff-6ae6279caeccc9dd66383a63ddda7808fc6cb7ef26d7f9ae868c4d884c5c99f2) listed here

# Setup
```zsh
./build.sh only_keyboard.se
./run.sh
```

It depends on your system having [PS4 Remote Play](https://remoteplay.dl.playstation.net/remoteplay/lang/en/index.html) installed at `/Applications/RemotePlay.app`. If this is not the case, you'll need to modify `run.sh` accordingly.

SE files are, generally speaking, a mapping between an input key, mouse button, or mouse movement to a DualShock 4 input.  See the example file (`nomanssky.se`) for a breakdown of the format.
The `OS X Command Line Tools` needs [to be installed](https://stackoverflow.com/a/53078282/584548).

Relies on `python2` (see #2), which can be installed via `brew install python@2`

# SE File Format
SE files are, generally speaking, a mapping between an input key, mouse button, or mouse movement to a DualShock 4 input. See the example file (`example.se`) for a breakdown of the format.

# How It Works
ShockEmu works by intercepting the IOHID calls of PS4 Remote Play application and presents an emulated DualShock controller. It also hooks into the input routines of the application, to catch keyboard and mouse inputs, which then get mapped according to your SE file.

# But It Is Not Working!
You may have to [turn off System Integrity Protection via 'csrutil'](https://www.imore.com/how-turn-system-integrity-protection-macos) in order for `DYLD_INSERT_LIBRARIES` to function on the newest macOS. Thanks [Ben](https://github.com/benh57) for figuring this out!

# Pro Tip
The `alias` below allows for typing `play` / `enter` anywhere in `Terminal` and have `RemotePlay.app` launched with the above keys mapped:
```
$ cat ~/.zshrc | grep play
alias play="pushd [REPOSITORY_ROOT]; ./run.sh &; popd"
```
👆🏻`ShockEmu` repo location must be updated according to your machine.

# Issues
- The main use case for me would be playing Elden Ring through PS Connect. For that game, the mouseLook mapping for the camera motion controls is very stuttering and impossible to play. Maybe someone can find a better way to change the settings from the eldenring.se file below:
```
# Map mouse to the right stick
mouseLook.type = linear # 1:1 translation mode, no curve
mouseLook.stick = right
mouseLook.multY = -1 # Flips the Y axis for mouseLook
mouseLook.deadZone = .05 # Sets the dead zone for the joystick
mouseLook.decay = 5 # What the joystick movement is divided by at each tick (probably does very little)
```

- The "shift" mapping for me does not work. I tried mapping both the "O" button and R1 for the PS5 controller to shift and it does not seem to work
