# RCA Studio II for MiSTer

## November 14, 2026 release

This release follows the October 16 release. It adds a selectable Studio III audio filter, fixes EF1 stopping while the display is off, reorganizes the OSD, adds keyboard cursor keys for keypad A, and corrects the Crop Offset steps.

## Highlights

### Studio III audio filter

- Added Audio & Video > Studio III Filter, a low-pass on Studio III audio that softens the raw square wave.
- Apollo 80 (default) has a 2576 Hz pole, matched to TLD1985's Academy Apollo 80 footage.
- MPT-02 has a 434 Hz pole, measured from FliP's direct CDP1864 captures of a PAL MPT-02.
- Off plays the unfiltered square wave, as in earlier releases.
- Studio II and Visicom audio are unchanged. The Studio III NTSC (CDP1863) path uses the same filter provisionally until hardware recordings are available.

### CDP1861/CDP1864 EF1 fix

- EF1 (display status) now keeps pulsing every frame while the display is off, as the datasheets describe. Previously it stopped along with the display.
- DMA and interrupts still stop while the display is off.

### Keyboard cursor keys

- The arrow keys act as a keypad A joystick (2/8/4/6). Pressing two arrows together gives the diagonal (1/3/7/9). Enter is 5 and Backspace is 0.
- They also work in the built-in game menu.

### OSD reorganization

- Top level: Load Software, Load CHIP-8, Machine, Apply and Reset, Input, Audio & Video, Palettes, System, Reset.
- Mapping, Joystick profile, Players and Numstick moved to a new Input page.
- Audio & Video lists audio options first, then video.
- Palettes have their own page, which shows the current machine's palette and, when Custom is selected, its loader.
- System gained Info Messages (on by default), which shows joystick profile changes as OSD popups. Clear is now labelled Clear (Soft Reset). The duplicate Reset and Unload Software and Reset entries were removed; the top-level Reset remains.

### Crop Offset fix

- The Crop Offset labels skipped +6, so every choice from 8 upward moved the picture 2 lines less than its label, and +12 was not available. The option now runs from -12 to +12 in steps of 2, and each label matches the result.
- Saved negative offsets move by one step after updating; a saved -12 becomes +12. Re-select the offset if the picture shifts.

## Additional engineering work

- Added a Studio III filter unit test (`make -C verilator mpt02-audio-test`) and a `--studio3-filter` option for the headless simulator.
- Documented the filter measurements and reference recordings in `docs/mpt02-audio.md`.
- Added the FPGA side of an experimental FujiNet cartridge. It needs a FujiNet-enabled MiSTer Main that is not released yet, so release builds leave it out (`FUJINET` macro in `Studio-II.qsf`).
- README, development and how-to-play documentation updates.

## Testing and validation

- The Studio III filter unit test and Verilator lint pass.
- MiSTer hardware checks passed: the reorganized OSD pages, per-machine palette menus, every Crop Offset step at 1080p with 216p crop, the Studio III filter options, and normal play on CDP1861 and CDP1864 machines after the EF1 change.
- The Quartus build meets all reported timing checks with fitter seed 13. Worst setup slack is +0.399 ns; worst hold slack is +0.252 ns.

## Known limitations

- Studio IV is not supported.
- CHIP-8 is unavailable in Visicom mode.
- The Studio III NTSC filter response has not been checked against hardware recordings.
- Marcel van Tongeren's optional interpreter retains its original memory limitations; bundled OpenStudio2 is recommended for normal CHIP-8 use.
- Direct analog video remains separately unverified.
