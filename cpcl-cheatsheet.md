# CPCL Cheatsheet

A practical reference for CPCL — the language of Zebra
mobile printers. Generated from the command catalog behind the
[Labelixa CPCL tools](https://labelixa.com/tools/cpcl-viewer).

## The smallest useful label

```cpcl
! 0 200 200 210 1
TEXT 4 0 30 40 Hello World
PRINT
```

The opening `!` line carries offset, x/y resolution, label height and
quantity — it is the header every CPCL job starts with. Commands are
space-separated (not comma-separated like TSPL), and `PRINT` ends the
job.


## Commands

| Command | Syntax | Example | Preview coverage |
| --- | --- | --- | --- |
| `!` | `! <ofset> <xres> <yres> <h> <adet>` | `! 0 200 200 210 1` | rendered |
| `BARCODE` | `BARCODE <tip> <dar> <oran> <h> <x> <y> <veri>` | `BARCODE 128 1 1 100 50 150 123456` | rendered (with documented limits) |
| `BOX` | `BOX x1 y1 x2 y2 t` | `BOX 10 10 560 200 2` | rendered |
| `CENTER` | `CENTER [genislik]` | `CENTER` | recognised, not rendered (printer behaviour) |
| `CONTRAST` | `CONTRAST n` | `CONTRAST 0` | recognised, not rendered (printer behaviour) |
| `FORM` | `FORM` | `FORM` | recognised, not rendered (printer behaviour) |
| `JOURNAL` | `JOURNAL` | `JOURNAL` | recognised, not rendered (printer behaviour) |
| `LINE` | `LINE x1 y1 x2 y2 t` | `LINE 0 300 560 300 2` | rendered |
| `PAGE-WIDTH` | `PAGE-WIDTH <w>` | `PAGE-WIDTH 576` | rendered |
| `PRINT` | `PRINT` | `PRINT` | rendered |
| `SETMAG` | `SETMAG w h` | `SETMAG 2 2` | recognised, not rendered (printer behaviour) |
| `SPEED` | `SPEED n` | `SPEED 3` | recognised, not rendered (printer behaviour) |
| `TEXT` | `TEXT <font> <boyut> <x> <y> <veri>` | `TEXT 4 0 30 40 Hello World` | rendered (with documented limits) |
| `TEXT180` | `TEXT180 <font> <boyut> <x> <y> <veri>` | `TEXT180 4 0 30 40 Ters` | rendered (with documented limits) |
| `TEXT270` | `TEXT270 <font> <boyut> <x> <y> <veri>` | `TEXT270 4 0 30 40 Dikey` | rendered (with documented limits) |
| `TEXT90` | `TEXT90 <font> <boyut> <x> <y> <veri>` | `TEXT90 4 0 30 40 Dikey` | rendered (with documented limits) |
| `TONE` | `TONE n` | `TONE 0` | recognised, not rendered (printer behaviour) |

## Notes on honesty

- CPCL uses spaces as separators and a `!` header
  line; mixing it up with TSPL syntax is the most common porting error.
- Rotated text has its own commands (`TEXT90`, `TEXT180`, `TEXT270`)
  rather than a rotation argument.
- Commands marked *recognised, not rendered* are mechanism settings
  (contrast, tone, speed, journal/form mode).

---

Paste a label and see it render: the free [CPCL viewer at labelixa.com](https://labelixa.com/tools/cpcl-viewer).
