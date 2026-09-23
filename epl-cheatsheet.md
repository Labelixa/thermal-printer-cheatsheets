# EPL Cheatsheet

A practical reference for EPL/EPL2 — the older Eltron
language still in service on many installed Zebra printers. Generated from
the command catalog behind the
[Labelixa EPL tools](https://labelixa.com/tools/epl-viewer).

## The smallest useful label

```epl
N
q812
A50,50,0,4,1,1,N,"HELLO"
P1
```

`N` clears the buffer, `q` sets the label width in dots, `A` places text,
and `P` prints. EPL is line-oriented and terse: one command per line, the
first character is the command.


## Commands

| Command | Syntax | Example | Preview coverage |
| --- | --- | --- | --- |
| `?` | `?` | `?` | recognised, not rendered (stored form / variable — a preview has no printer memory) |
| `A` | `Ax,y,rot,font,hm,vm,N/R,"text"` | `A50,50,0,4,1,1,N,"HELLO"` | rendered (with documented limits) |
| `B` | `Bx,y,rot,type,narrow,wide,h,B/N,"data"` | `B50,150,0,1,3,7,120,B,"123456"` | rendered (with documented limits) |
| `D` | `D<density>` | `D8` | recognised, not rendered (printer behaviour) |
| `FE` | `FE` | `FE` | recognised, not rendered (stored form / variable — a preview has no printer memory) |
| `FI` | `FI` | `FI` | recognised, not rendered (stored form / variable — a preview has no printer memory) |
| `FK` | `FK<"FORMNAME" | "*">` | `FK"FORM1"` | recognised, not rendered (stored form / variable — a preview has no printer memory) |
| `FR` | `FR<"FORMNAME">` | `FR"FORM1"` | recognised, not rendered (stored form / variable — a preview has no printer memory) |
| `FS` | `FS<"FORMNAME">` | `FS"FORM1"` | recognised, not rendered (stored form / variable — a preview has no printer memory) |
| `I` | `I8,A,001` | `I8,A,001` | recognised, not rendered (printer behaviour) |
| `JF` | `JF` | `JF` | recognised, not rendered (printer behaviour) |
| `LO` | `LOx,y,w,h` | `LO50,300,700,4` | rendered |
| `LW` | `LWx,y,w,h` | `LW60,310,100,4` | rendered |
| `N` | `N` | `N` | rendered |
| `P` | `P<n>` | `P2` | rendered |
| `Q` | `Q<h>,<gap>` | `Q1218,24` | rendered |
| `R` | `Rx,y` | `R16,0` | rendered |
| `S` | `S<speed>` | `S3` | recognised, not rendered (printer behaviour) |
| `V` | `V<nn>,<len>,<L|R|C|N>,<"PROMPT">` | `V00,15,N,"Product:"` | recognised, not rendered (stored form / variable — a preview has no printer memory) |
| `X` | `Xx1,y1,t,x2,y2` | `X40,40,4,760,400` | rendered |
| `ZB` | `ZB` | `ZB` | recognised, not rendered (printer behaviour) |
| `ZT` | `ZT` | `ZT` | recognised, not rendered (printer behaviour) |
| `q` | `q<w>` | `q812` | rendered |

## Notes on honesty

- EPL is dots throughout; there is no unit-bearing
  size command like TSPL's `SIZE`.
- Text and barcode arguments are positional and unforgiving — a missing
  comma shifts every following value.
- Commands marked *recognised, not rendered* are printer-side settings
  (density, speed, form handling).

---

Paste a label and see it render: the free [EPL viewer at labelixa.com](https://labelixa.com/tools/epl-viewer).
