# Keebio Iris Rev 8 — QMK Keymap Reference

**Keyboard:** Keebio Iris Rev 8 (split, RP2040)
**Keymap:** `apclark31`
**Firmware:** QMK

---

## File Structure

```
keyboards/keebio/iris/keymaps/apclark31/
├── keymap.c              # Main keymap — all layers and key bindings
├── rules.mk              # Build flags (RGB disabled, WS2812 driver)
└── features/
    ├── achordion.c        # Achordion library — refines tap-hold behavior
    └── achordion.h        # Achordion header
```

### rules.mk

```makefile
RGBLIGHT_ENABLE = no
RGB_MATRIX_ENABLE = no
WS2812_DRIVER =
```

RGB lighting is disabled in the build. If you want to re-enable it, set `RGB_MATRIX_ENABLE = yes` and specify the `WS2812_DRIVER`.

---

## Compiling & Flashing

### Compile

```bash
qmk compile -kb keebio/iris/rev8 -km apclark31
```

### Flash

```bash
qmk flash -kb keebio/iris/rev8 -km apclark31
```

To enter bootloader mode on the Iris Rev 8 (RP2040): **double-tap the reset button** on the PCB. The board will mount as a USB drive. `qmk flash` will copy the `.uf2` firmware file automatically.

> **Tip:** Flash each half separately. The side plugged into USB is the one being flashed.

### Full Clean Build

```bash
qmk clean && qmk compile -kb keebio/iris/rev8 -km apclark31
```

---

## Layers Overview

The keymap defines **5 layers** (0–4):

| Layer | Index | Activation                                    | Purpose                          |
|-------|-------|-----------------------------------------------|----------------------------------|
| Base  | 0     | Default                                       | QWERTY + home row mods           |
| Nav   | 1     | Hold Enter (left thumb)                       | Numpad (right) + editing (left)  |
| Fn    | 2     | Hold MO(2) (left thumb)                       | F-keys + symbols + arrows + RGB  |
| Sym   | 3     | Hold Space (right thumb) / Z / Slash          | Symbols + F-keys                 |
| NumFn | 4     | Hold MO(4) (right thumb)                      | F-keys (left) + numpad (right)   |

---

## Layer 0 — Base (QWERTY + Home Row Mods)

```
╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮                    ╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮
│  ESC/`   │    1     │    2     │    3     │    4     │    5     │                    │    6     │    7     │    8     │    9     │    0     │   DEL    │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│   TAB    │    Q     │    W     │    E     │    R     │    T     │                    │    Y     │    U     │    I     │    O     │    P     │  BSPC    │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│   GUI    │  A/LCTL  │  S/LALT  │  D/LGUI  │  F/LSFT  │    G     │                    │    H     │  J/RSFT  │  K/RGUI  │  L/LALT  │  ;/RCTL  │    '     │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────╮╭──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  LSFT    │  Z/Sym   │    X     │    C     │    V     │    B     │  HYPER   ││  xxxxx   │    N     │    M     │    ,     │    .     │  / /Sym  │  RSFT    │
╰──────────┴──────────┴──────────┴──────────┼──────────┼──────────┼──────────╯╰──────────┼──────────┼──────────┼──────────┴──────────┴──────────┴──────────╯
                                            │   GUI    │  MO(2)   │ ENT/Nav  ││ SPC/Sym  │  MO(4)   │  RALT    │
                                            ╰──────────┴──────────┴──────────╯╰──────────┴──────────┴──────────╯
```

**Home Row Mods (GASC):** `A=LCTL`, `S=LALT`, `D=LGUI`, `F=LSFT` | `J=RSFT`, `K=RGUI`, `L=LALT`, `;=RCTL`

**Notable keys:**
- `QK_GESC` — Escape normally, backtick/grave with Shift
- `Z` / `/` — tap for letter, hold for Layer 3 (Sym)
- Left encoder key: `KC_HYPR` (Ctrl+Shift+Alt+GUI)
- Right encoder key: disabled (`XXXXXXX`)

---

## Layer 1 — Nav (Numpad + Editing)

```
╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮                    ╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮
│  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │                    │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │   DEL    │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │                    │  xxxxx   │   P7     │   P8     │   P9     │  BSPC    │  BSPC    │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  xxxxx   │  LCTL    │  LALT    │  LGUI    │  LSFT    │    [     │                    │  xxxxx   │   P4     │   P5     │   P6     │    +     │    |     │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────╮╭──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  xxxxx   │  UNDO    │  CUT     │  COPY    │  PASTE   │    {     │    (     ││    )     │  xxxxx   │   P1     │   P2     │   P3     │    -     │  _____   │
╰──────────┴──────────┴──────────┴──────────┼──────────┼──────────┼──────────╯╰──────────┼──────────┼──────────┼──────────┴──────────┴──────────┴──────────╯
                                            │  _____   │  _____   │   DEL    ││   DEL    │   P0     │  _____   │
                                            ╰──────────┴──────────┴──────────╯╰──────────┴──────────┴──────────╯
```

**Left hand:** Bare modifiers + editing shortcuts (Undo, Cut, Copy, Paste) + brackets
**Right hand:** Numpad (7-8-9 / 4-5-6 / 1-2-3 / 0)

---

## Layer 2 — Fn (F-Keys + Symbols + Arrows + RGB)

```
╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮                    ╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮
│   F12    │   F1     │   F2     │   F3     │   F4     │   F5     │                    │   F6     │   F7     │   F8     │   F9     │   F10    │   F11    │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│ RGB_TOG  │    !     │    @     │    #     │    $     │    %     │                    │    ^     │    &     │    UP    │    (     │    )     │  _____   │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│ RGB_NXT  │  LCTL    │  LALT    │  LGUI    │  LSFT    │    _     │                    │    =     │   LEFT   │   DOWN   │  RIGHT   │ RGB_VAI  │    \     │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────╮╭──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  MUTE    │  STOP    │  PLAY    │ VOL_DN   │  PGDN    │    -     │    (     ││  LALT    │    +     │   END    │ RGB_HUD  │ RGB_SAD  │ RGB_VAD  │  _____   │
╰──────────┴──────────┴──────────┴──────────┼──────────┼──────────┼──────────╯╰──────────┼──────────┼──────────┼──────────┴──────────┴──────────┴──────────╯
                                            │  _____   │  _____   │  _____   ││  _____   │  _____   │  _____   │
                                            ╰──────────┴──────────┴──────────╯╰──────────┴──────────┴──────────╯
```

**Left hand:** F1–F5, symbols (shifted numbers), media controls
**Right hand:** F6–F11, arrow keys, RGB controls

---

## Layer 3 — Sym (Symbols)

```
╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮                    ╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮
│   F12    │   F1     │   F2     │   F3     │   F4     │   F5     │                    │   F6     │   F7     │   F8     │   F9     │   F10    │   F11    │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│ RGB_TOG  │    `     │    <     │    >     │    -     │    |     │                    │    ^     │    {     │    }     │    $     │    _     │  _____   │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│ RGB_NXT  │    !     │    *     │    /     │    =     │    &     │                    │    #     │    (     │    )     │    ;     │    "     │    \     │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────╮╭──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  MUTE    │    ~     │    +     │    [     │    ]     │    %     │    (     ││  _____   │    @     │    :     │    ,     │    .     │    '     │  xxxxx   │
╰──────────┴──────────┴──────────┴──────────┼──────────┼──────────┼──────────╯╰──────────┼──────────┼──────────┼──────────┴──────────┴──────────┴──────────╯
                                            │  _____   │  _____   │  _____   ││  _____   │  _____   │  _____   │
                                            ╰──────────┴──────────┴──────────╯╰──────────┴──────────┴──────────╯
```

**Full symbol layer** — accessed via hold-Z (left), hold-/ (right), or hold-Space (right thumb).

---

## Layer 4 — NumFn (F-Keys Left + Numpad Right)

```
╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮                    ╭──────────┬──────────┬──────────┬──────────┬──────────┬──────────╮
│  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │                    │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │  xxxxx   │   DEL    │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  xxxxx   │   F9     │   F10    │   F11    │   F12    │  xxxxx   │                    │  xxxxx   │   P7     │   P8     │   P9     │  BSPC    │  BSPC    │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤                    ├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  xxxxx   │   F5     │   F6     │   F7     │   F8     │    [     │                    │  xxxxx   │   P4     │   P5     │   P6     │    +     │    |     │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────╮╭──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│  xxxxx   │   F1     │   F2     │   F2*    │   F4     │    {     │    (     ││    )     │  xxxxx   │   P1     │   P2     │   P3     │    -     │  _____   │
╰──────────┴──────────┴──────────┴──────────┼──────────┼──────────┼──────────╯╰──────────┼──────────┼──────────┼──────────┴──────────┴──────────┴──────────╯
                                            │  _____   │  _____   │   DEL    ││   DEL    │   P0     │  _____   │
                                            ╰──────────┴──────────┴──────────╯╰──────────┴──────────┴──────────╯
```

> **Note:** Row 3, column 4 has `KC_F2` — this looks like a typo and should probably be `KC_F3`.

---

## Key Features

### Home Row Mods

Layer 0 uses mod-tap on the home row (GASC layout, matching your ZMK Sofle):

| Key | Tap | Hold  |
|-----|-----|-------|
| A   | a   | LCTL  |
| S   | s   | LALT  |
| D   | d   | LGUI  |
| F   | f   | LSFT  |
| J   | j   | RSFT  |
| K   | k   | RGUI  |
| L   | l   | LALT  |
| ;   | ;   | RCTL  |

### Achordion (Tap-Hold Refinement)

The `features/achordion.c` library (by Google's Pascal Getreuer) is included to improve home row mod behavior. It uses a **bilateral combinations** rule: a tap-hold key is only considered "held" when the next key pressed is on the **opposite hand**. This prevents accidental mod triggers during fast same-hand typing.

### Layer-Tap Keys

| Key Position       | Tap      | Hold         |
|--------------------|----------|--------------|
| Z (base)           | z        | Layer 3 (Sym)|
| / (base)           | /        | Layer 3 (Sym)|
| Enter (left thumb) | Enter    | Layer 1 (Nav)|
| Space (right thumb)| Space    | Layer 3 (Sym)|

---

## Legend

| Symbol  | Meaning                                |
|---------|----------------------------------------|
| `xxxxx` | `XXXXXXX` — key does nothing           |
| `_____` | `_______` — transparent (falls through)|
| `A/LCTL`| Mod-tap: tap=A, hold=Left Control      |
| `MO(n)` | Momentary layer switch                 |
| `LT(n)` | Layer-tap: tap=key, hold=layer n       |

---

## Potential Issues

1. **Layer 4, row 3, col 4:** `KC_F2` appears twice — the second instance (row 3, col 4) is likely meant to be `KC_F3`.
2. **`WS2812_DRIVER`** in `rules.mk` has no value assigned — this is fine since RGB is disabled, but would need a value (e.g., `vendor`) if RGB is re-enabled.

---

## ZMK Comparison Notes

Your ZMK Sofle keymap uses the same home row mod layout (GASC) with ASCII art borders for readability. For the next task — reformatting this QMK keymap — we'll adopt that visual style using C comments with box-drawing characters to make the layers scannable at a glance, similar to:

```c
// ╭──────────┬──────────┬──────────╮   ╭──────────┬──────────┬──────────╮
     KC_ESC,    KC_1,      KC_2,          KC_9,      KC_0,      KC_DEL,
// ├──────────┼──────────┼──────────┤   ├──────────┼──────────┼──────────┤
```
