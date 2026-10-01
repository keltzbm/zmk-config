# zmk-config

Firmware for a 42-key split Corne (3x6 and three thumb keys a side) on two
nice!nano v2 controllers with nice!view displays. The left half is the
central and has ZMK Studio on. Built by GitHub Actions against ZMK `v0.3`.

## The wiring fix

This board has one change from a stock Corne. Matrix row 1, the home row,
was shorted on its stock pin (D5, P0.24), so a wire takes it to D9, which is
P1.06 on the nice!nano. `config/boards/shields/corne/corne.dtsi` reads that
row from `&gpio1 06`; everything else is the stock shield, copied here so the
change can live in it. When taking changes from upstream's Corne shield, keep
that line.

Debounce is 15 ms for press and release (`config/corne.conf`), raised while
the short was being found. The stock value is 5; try 10 if keys feel slow.

## Layers

`config/corne.keymap` is the only keymap.

- **Base:** QWERTY. The key left of A is Ctrl when held and Esc when tapped.
  Shift on both bottom corners; both together turn on Caps Word.
- **Lower** (left inner thumb): numbers on the top row; arrows on the right
  home row as in vim (left, down, up, right); Home, Page Down, Page Up and
  End under them; Delete.
- **Raise** (right inner thumb): symbols.
- **Adjust** (Lower and Raise together): F1 to F12, Bluetooth profiles 1 to 5,
  clear this profile, clear all profiles, USB or Bluetooth output, media
  keys, the ZMK Studio unlock, and a bootloader and a reset key on each half.
  The Bluetooth keys are here, behind two keys, so a slip on Lower can't
  unpair the board.

## Flashing

1. Push, or run the workflow by hand; the Actions run keeps a `firmware`
   artifact with `corne_left`, `corne_right` and `settings_reset`.
2. Put a half in its bootloader (Adjust and its corner key, or double-tap
   the reset button), and copy that half's `.uf2` to the drive that appears.
3. If the halves stop pairing, flash `settings_reset` to both, then the
   firmware again.

ZMK Studio keeps its changes on the keyboard, and they win over this
keymap. After flashing a new keymap, use Studio's "Restore Stock Settings"
if the keys don't change.

## Power

The board sleeps after 15 minutes idle and wakes on any key. The left half
reports both halves' battery levels to the host.
