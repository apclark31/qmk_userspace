# Iris Rev 8 Keymap — Working Plan

This file tracks the ongoing keymap redesign so any conversation can pick up where we left off.

---

## Goals

1. **Streamline number input** — numpad layer should allow typing numbers with spaces
2. **Streamline Excel workflows** — numbers, arrows, tab, and enter need to coexist better
3. **Clean up redundancy** — Layers 1 and 4 share an identical right-hand numpad
4. **Fix missing keys** — HOME, PGUP, VOLU, PDOT are absent from all layers
5. **Remove unintentional firmware dump hotkey** — resolved by reflashing (see below)
6. **Maintain ZMK-style ASCII art formatting** for readability

---

## Layer Architecture (current)

| Layer | Index | Activation | Purpose |
|-------|-------|------------|---------|
| Base  | 0 | Default | QWERTY + home row mods (GASC) |
| Nav   | 1 | Hold Enter (left thumb, LT(1, KC_ENT)) | Numpad (right) + editing shortcuts (left) |
| Fn    | 2 | Hold MO(2) (left thumb) | F-keys + symbols + arrows + RGB |
| Sym   | 3 | Hold Space (right thumb, LT(3, KC_SPC)) / Hold Z / Hold / | Symbols |
| NumFn | 4 | Hold MO(4) (right thumb) | F-keys (left) + numpad (right) |

### Key insight: Layer stacking

- Holding **Enter** (Layer 1) + tapping **Space** should give space, then holding Space should activate Layer 3 (Sym) on top — giving access to symbols while in numpad mode.
- This only works if Layer 1's Space position is **transparent** (`_______`), letting the base layer's `LT(3, KC_SPC)` fall through.

### Encoders (handled at board level in rev8.c, not in keymap)

- Left encoder: Volume Up / Volume Down
- Right encoder: Page Down / Page Up

This means **PGUP and VOLU are available via encoders** even though they're not in the keymap grid.

---

## Changes Made

### Round 1 (initial formatting)
- [x] Reformatted keymap.c with ZMK-style ASCII art borders
- [x] Fixed KC_F2 → KC_F3 typo in Layer 4, row 3, col 4
- [x] Converted KC_HYPR → KC_MEH on Layer 0 encoder key

### Round 2 (numpad/space fix)
- [x] Layer 1 right thumb: `KC_DEL` → `_______` (transparent, so Space falls through from base)
- [x] Layer 4 right thumb: `KC_DEL` → `_______` (same fix for consistency)
- This preserves `LT(3, KC_SPC)` behavior: tap=Space, hold=Sym layer

### Round 3 (infrastructure)
- [x] Set up QMK external userspace repo with GitHub Actions cloud builds
- [x] Confirmed Achordion files are unused — native QMK `CHORDAL_HOLD` is active instead
- [x] Dropped `features/` directory from userspace (not needed)

### Firmware dump hotkey
- **Resolved:** No `QK_BOOT` keycode exists in the source. The old firmware on the board likely had one. Flashing the current keymap removes it.

---

## Known Issues (still to address)

### Missing keys (from grid — some available via encoders)
- **KC_HOME** — KC_END is on Layer 2 but Home is not in the key grid
- **KC_PDOT** — numpad 0-9 exist but no numpad decimal point
- **KC_PGUP** — available via right encoder, but not in key grid
- **KC_VOLU** — available via left encoder, but not in key grid

### Redundancy
- **Layers 1 & 4 right side are identical** — same numpad layout, same surrounding keys. One could be repurposed or differentiated.
- **F-keys on 3 layers** (L2, L3, L4) — triple coverage may be excessive
- **KC_BSPC x5** across layers — L1 and L4 both have it twice in adjacent positions on row 1

### Excel workflow
- **Problem:** Numpad (Layer 1) and arrow keys (Layer 2) are on different layers — can't use both simultaneously
- **Potential solutions (to be discussed):**
  - Add arrow cluster to Layer 1 (replace some XXXXXXX positions on left hand)
  - Rework Layer 4 to be an "Excel mode" with numpad + arrows + tab
  - Use Layer 1 + Layer 3 stacking (now that Space is transparent, Sym layer is reachable)

### Backtick accessibility
- Available on Layer 3 at Q position (KC_GRAVE) and via QK_GESC (Shift+ESC)
- User to decide if this is accessible enough or needs a better spot

---

## Research Needed (user)

- What specific Excel shortcuts/workflows need optimization?
- Desired behavior for Layer 4 — keep as numpad+fkeys, or repurpose?
- Backtick: keep on Layer 3, or find a more accessible position?
