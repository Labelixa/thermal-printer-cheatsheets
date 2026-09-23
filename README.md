# Thermal printer cheatsheets

Command references for the four label languages that cover most
thermal printers in the field — ZPL (Zebra), TSPL (TSC and
compatibles), EPL (legacy Eltron) and CPCL (Zebra mobile).

Every table here is **generated from a working implementation**,
not transcribed from a manual: the same command catalogs drive a
renderer that these commands are tested against. Where the
renderer does not draw a command, the sheet says so instead of
implying full support.

| Language | Commands | Sheet |
| --- | --- | --- |
| ZPL | 68 | [zpl-cheatsheet.md](zpl-cheatsheet.md) |
| TSPL | 17 | [tspl-cheatsheet.md](tspl-cheatsheet.md) |
| EPL | 23 | [epl-cheatsheet.md](epl-cheatsheet.md) |
| CPCL | 17 | [cpcl-cheatsheet.md](cpcl-cheatsheet.md) |

## Which language am I looking at?

A quick structural tell, useful when a file arrives with no
context:

| Looks like | Language |
| --- | --- |
| Starts with `^XA`, commands prefixed `^` or `~` | ZPL |
| `SIZE 100 mm, 150 mm` then `CLS` … `PRINT` | TSPL |
| Bare `N`, `q812`, `A50,50,...`, ends with `P1` | EPL |
| First line starts with `!` and ends with `PRINT` | CPCL |

## Coverage labels

- **rendered** — the engine draws it.
- **rendered (with documented limits)** — drawn, but a documented
  parameter limit applies.
- **recognised, not rendered (printer behaviour)** — the command
  changes the mechanism (darkness, speed, feed direction, media
  tracking), which a screen render cannot honestly reproduce.
- **recognised, not rendered (stored form / variable)** — EPL
  `FS`/`FR`/`V`/`?` read or write the printer's form memory; a
  preview has none, so paste the `FS`…`FE` block itself to see it.

## License

MIT — copy these tables into your own docs freely.

---

Paste a label and see it render, no printer and no signup:
[labelixa.com](https://labelixa.com/tools/zpl-preview).
