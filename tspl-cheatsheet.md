# TSPL Cheatsheet

A practical reference for TSPL/TSPL2 — the language TSC
label printers and many compatibles speak. Generated from the same command
catalog that powers the
[Labelixa TSPL tools](https://labelixa.com/tools/tspl-viewer), so the
coverage notes below match a maintained, tested implementation.

## The smallest useful label

```tspl
SIZE 100 mm, 150 mm
GAP 3 mm, 0 mm
CLS
TEXT 100, 100, "3", 0, 1, 1, "HELLO"
PRINT 1
```

`SIZE` takes real units — millimetres here, inches when the unit is
omitted (`SIZE 4, 6`). Coordinates inside the label are dots. `CLS` clears
the image buffer and `PRINT` commits it; forget either and the job prints
nothing, or reprints the previous one.


## Commands

| Command | Syntax | Example | Preview coverage |
| --- | --- | --- | --- |
| `BAR` | `BAR x, y, w, h` | `BAR 50, 500, 700, 4` | rendered |
| `BARCODE` | `BARCODE x, y, "type", h, readable, rot, n, w, "data"` | `BARCODE 100, 200, "128", 100, 1, 0, 3, 3, "123456"` | rendered (with documented limits) |
| `BLINE` | `BLINE <b> mm, <o> mm` | `BLINE 3 mm, 0 mm` | recognised, not rendered (printer behaviour) |
| `BOX` | `BOX x1, y1, x2, y2, t` | `BOX 40, 40, 760, 400, 4` | rendered |
| `CLS` | `CLS` | `CLS` | rendered |
| `CODEPAGE` | `CODEPAGE n` | `CODEPAGE 1252` | recognised, not rendered (printer behaviour) |
| `DENSITY` | `DENSITY n` | `DENSITY 8` | recognised, not rendered (printer behaviour) |
| `DIRECTION` | `DIRECTION n[, m]` | `DIRECTION 1` | recognised, not rendered (printer behaviour) |
| `GAP` | `GAP <g> mm, <o> mm` | `GAP 3 mm, 0 mm` | rendered |
| `OFFSET` | `OFFSET <o> mm` | `OFFSET 0 mm` | recognised, not rendered (printer behaviour) |
| `PRINT` | `PRINT n[, copies]` | `PRINT 1` | rendered |
| `QRCODE` | `QRCODE x, y, ecc, cell, mode, rot, "data"` | `QRCODE 100, 400, M, 6, A, 0, "https://..."` | rendered (with documented limits) |
| `REFERENCE` | `REFERENCE x, y` | `REFERENCE 0, 0` | rendered |
| `SET` | `SET <opt> <val>` | `SET TEAR ON` | recognised, not rendered (printer behaviour) |
| `SIZE` | `SIZE <w> mm, <h> mm` | `SIZE 100 mm, 150 mm` | rendered |
| `SPEED` | `SPEED n` | `SPEED 4` | recognised, not rendered (printer behaviour) |
| `TEXT` | `TEXT x, y, "font", rot, xm, ym, "content"` | `TEXT 100, 100, "3", 0, 1, 1, "HELLO"` | rendered (with documented limits) |

## Notes on honesty

- Units differ from ZPL: TSPL declares the physical
  label in millimetres or inches, then positions in dots.
- Commands marked *recognised, not rendered* change the mechanism (feed
  direction, offset, black-mark tracking, darkness, speed, code page); a
  screen render cannot reproduce them honestly.
- The preview's barcode set is Code 128, Code 39, EAN-13 and UPC-A; other
  symbologies are reported rather than faked.

---

Paste a label and see it render: the free [TSPL viewer at labelixa.com](https://labelixa.com/tools/tspl-viewer).
