Note from Steve: I'm using this AHK-MIDI v2 library in https://github.com/steveseguin/powerpoint_remote.

Thank you to the original devs.

------------------------------------ (original readme below) ------

# About This Fork

## Midi2.ahk
This is compatible with AutoHotkey v2. Due to this impact, the description method has been changed. Like this.

```ahk

#include "path\to\Midi2.ahk"

midi := AHKMidi()
midi.midiEventPassThrough := True
midi.delegate := MyDelegate()
;midi.specificProcessCallback := True

; open midi device manually
; midi.OpenMidiInByName("name")
; midi.OpenMidiOutByName("name")
;
; or save and load devices setting with ini file
; auto save selection from task tray menu
midi.settingFilePath := A_ScriptDir . "\setting.ini"

Class MyDelegate
{
    ; if specificProcessCallback is set true
    ; trigger only when the applicable process is front
    ; replace any spaces or "." in the process name with "_".
    explorer_exe_MidiNoteOnC4(event) {
        MsgBox("C4 on explorer.exe")
    }

    MidiNoteOnC4(event) {
        MsgBox("C4 Pressed")
    }

    ; use "s" instead of "#"
    MidiNoteOnCs4(event) {
        MsgBox("C#4 Pressed")
    }

    MidiControlChange(event) {
        MsgBox(event.controller . "=" . event.value)

        ; pass through this event to midi out device
        event.eventHandled := false
    }
}

```

## Midi.ahk

- Merged [Support for Midi Out and getDeviceByName by fashberg · Pull Request #7](https://github.com/dannywarren/AutoHotkey-Midi/pull/7)
- Merged [Fix: NoteOff event may not triggered by 9chu · Pull Request #1](https://github.com/dannywarren/AutoHotkey-Midi/pull/1)

- Add option that path through ignored event to output device. To turn on, write `midiEventPassThrough  := True` [#3](https://github.com/hetima/AutoHotkey-Midi/pull/3)
- Add save and load I/O setting with ini file [#4](https://github.com/hetima/AutoHotkey-Midi/pull/4)

- Midi out to specific device [#6](https://github.com/hetima/AutoHotkey-Midi/pull/6)

- Add MidiOutRawData() [#7](https://github.com/hetima/AutoHotkey-Midi/pull/7)


# AutoHotkey-Midi

Add MIDI input event handling to your AutoHotkey scripts

```ahk

#include AutoHotkey-Midi/Midi.ahk

midi := new Midi()
midi.OpenMidiOutByName("X-TOUCH MINI")
midi.OpenMidiInByName("X-TOUCH MINI")

; send some  Outout
midi.MidiOut("CC", 1, 127, 0) ; ControllerChange on Channel 1, Code 27
midi.MidiOut("N1", 1, 1, 100) ; Note on On Channel 1, Note 1, Velocity 100

Return

MidiNoteOnA4:
    MsgBox You played note A4!
    Return

MidiControlChange1:
    cc := midi.MidiIn()
    ccValue := cc.value
    MsgBox You set the mod wheel to %ccValue%
    Return

```

## Requirements

* A modern version of AutoHotKey (1.1+) from https://www.autohotkey.com/
* A system with winmm.dll (Windows 2000 or greater)

## License

BSD

## TODO

* Documentation!
