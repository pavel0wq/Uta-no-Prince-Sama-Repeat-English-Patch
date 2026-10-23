# Uta no Prince-sama Repeat — English AI Translation (PSP)

<img width="4032" height="3024" alt="IMG_0281" src="https://github.com/user-attachments/assets/38c0d324-dae9-408b-949b-e4c9f637219a" />
<img width="4032" height="3024" alt="IMG_0283" src="https://github.com/user-attachments/assets/6ba8aabe-dccd-4cad-afee-0046ba7a7f13" />
<img width="4032" height="3024" alt="IMG_0282" src="https://github.com/user-attachments/assets/3c7b136e-fa8b-493e-b33d-25ae5c63e1fa" />



An unofficial English translation patch for **Uta no☆Prince-sama♪ Repeat** (PSP, Broccoli, 2011, `NPJH50446`).

This repository holds the translated script and the tools that rebuild the game with it. It does **not** contain the game or any copyrighted game data. You need your own copy of the Japanese ISO.

## Status

| Part | Status |
|---|---|
| Main story: all routes, all chapters (~41,000 lines) | ✅ Translated |
| Choices, speaker names, chapter / route titles | ✅ Translated |
| Name entry screen (Latin keyboard, English labels, default name *Haruka Nanami*) | ✅ Translated |
| Menus, system messages, help text, PSP dialogs (EBOOT) | ✅ Translated |
| Music Dictionary | ✅ Translated |
| Music Quiz minigame (200 questions × 3 difficulties) | ✅ Translated |
| Memorial short stories (112 stories, incl. the "Memorial Complete" messages) | ✅ Translated |
| Lyrics Test minigame | ✅ Lyrics in **romaji** (so they match the sung audio); katakana answer choices in English |
| Text inside images (titles, buttons, some UI textures) | ❌ Not yet |
| Song lyrics shown during performances | ❌ Not touched (they are in images) |

### Known issues / not yet verified in game

- Line wrapping in the **Memorial**, **Dictionary**, **Quiz** and **Lyrics Test** screens has not been checked on real hardware or in an emulator yet. Text may overflow in a few places.
- The kanji input mode on the name entry screen is disabled (only Latin letters and digits are available).
- Some UI graphics are still in Japanese.

Bug reports with screenshots are very welcome. Please open an issue.

## Applying the patch

1. Get a clean dump of the Japanese ISO:
   - `Uta no Prince-sama - Repeat (Japan).iso`
   - MD5: `99781ae9d7898da748ada2346019d919`
   - SHA-1: `e2751eac1fae76b01ab1f95d343052bb1012c8c3`
2. Download `UtaPri_Repeat_English.xdelta` from the [Releases](../../releases) page.
3. Apply it with any xdelta tool:
   - **Windows:** [xdelta UI](https://www.romhacking.net/utilities/598/) or [Delta Patcher](https://github.com/marco-calautti/DeltaPatcher)
   - **macOS / Linux:**
     ```sh
     xdelta3 -d -s "Uta no Prince-sama - Repeat (Japan).iso" UtaPri_Repeat_English.xdelta "Uta no Prince-sama - Repeat (English).iso"
     ```
4. Play the patched ISO on a PSP with custom firmware or in [PPSSPP](https://www.ppsspp.org/).

Start a **new save**. Saves made with the Japanese version keep their Japanese names.

## Repository layout

```
text/            translated text (CSV, UTF-8) — edit these to improve the translation
  00.csv–08.csv  main story scripts (one file per script archive)
  eboot.csv      menus / system strings from the EBOOT
  mem.csv        Memorial short stories
  dictionary.csv Music Dictionary
  quiz.csv       Music Quiz
  lyric.csv      Lyrics Test (romaji)
  chapters.csv   chapter / route select titles
  glossary.md    names, honorifics and style conventions
  tl/            translation batches as they were applied
fonts/           M PLUS Rounded 1c (used to draw the variable-width English font)
tools/           extraction, insertion and build scripts (Python)
```

### How it works (short version)

- The game only renders Shift-JIS text. English is written with full-width Latin characters, whose glyphs in the game font are **redrawn** from M PLUS Rounded 1c with proportional (variable-width) metrics (`tools/font.py`).
- Story scripts are decoded and re-encoded by `tools/story.py` / `tools/script_text.py`. Long messages are re-wrapped and split into extra pages when needed.
- EBOOT strings are written in place when they fit; otherwise they are moved to free space and every pointer / `lui+addiu` reference is repointed (`tools/eboot_menu.py`). Small code patches (name entry, nameplate, proportional labels) live in `tools/eboot_text.py` and `tools/eboot_patch.py`.
- NIS-style `.DAT` archives are unpacked and repacked with `tools/nispack.py`, and the ISO is rebuilt in place by `tools/isopatch.py`.

## Contributing

Translation fixes are easiest as edits to the CSV files in `text/`: change the `en` column only and keep the `id` / `jp` columns untouched. Please follow `text/glossary.md` for names and honorifics (e.g. *Haruka Nanami*, *Ringo-sensei*, *Hijirikawa-sama*).

## Credits

- Translation and tools: the UtaPri Repeat English patch project
- Font: [M PLUS Rounded 1c](https://fonts.google.com/specimen/M+PLUS+Rounded+1c) (SIL Open Font License)

## Disclaimer

This is a non-commercial fan project and is not affiliated with Broccoli, Shining Production or any rights holder. *Uta no☆Prince-sama♪* and all related names and content belong to their respective owners. Please support the official releases. Do not ask for or share ISO files.
