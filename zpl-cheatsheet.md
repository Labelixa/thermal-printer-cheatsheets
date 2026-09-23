# ZPL Cheatsheet

A practical reference for ZPL (Zebra Programming Language) — the command
set thermal label printers from Zebra and many compatibles understand.
Generated from the same command catalog that powers the
[Labelixa command reference](https://labelixa.com/zpl-commands), so the
syntax and parameter notes here match a maintained, tested implementation
rather than folklore.

## The smallest useful label

```zpl
^XA
^FO50,50^A0N,40,40^FDHello label^FS
^XZ
```

Every label lives between `^XA` and `^XZ`. `^FO` positions a field in
printer dots (resolution-dependent: 812 dots = 4 inches at 203 dpi but
only 2.7 inches at 300 dpi), `^FD` carries the data, `^FS` closes the
field — forget it and the following commands are swallowed as field data.


## Format control

| Syntax | Command | Example |
| --- | --- | --- |
| `^XA` | **Start Format** — Marks the beginning of a label format. Every label starts with ^XA. | `^XA` |
| `^XZ` | **End Format** — Ends the label format and prints or renders the label. | `^XZ` |
| `^FS` | **Field Separator** — Ends a field. Every ^FD, ^GB or barcode field is closed with ^FS. | `^FDMerhaba^FS` |
| `^FXc` | **Comment** — A comment. It has no effect on rendering and is skipped. | `^FXBu bir yorumdur^FS` |
| `^CFf,h,w` | **Change Default Font** — Sets the default font, height and width for the ^FD fields that follow. | `^CF0,50,40` |
| `^CIa` | **Character Set** — Selects the international character encoding. The engine assumes UTF-8 (^CI28). | `^CI28` |
| `^PWa` | **Print Width** — Sets the print width of the label in dots. | `^PW812` |
| `^LLy` | **Label Length** — Sets the label length in dots. | `^LL1218` |
| `^LHx,y` | **Label Home** — Moves the top-left (home) reference point of the label. | `^LH30,30` |
| `^LTx` | **Label Top** — Shifts the whole label vertically. | `^LT10` |
| `^POa` | **Print Orientation** — Selects the print orientation: ^POI prints the whole label rotated 180°. The flip happens inside the printer; the preview shows the label upright. | `^POI` |
| `^LRa` | **Label Reverse** — Prints following fields reversed (negative); ^LRN turns it off. | `^LRY` |
| `^LSa` | **Label Shift** — Shifts ALL fields horizontally. Shipping labels usually carry ^LS0 (no effect); a non-zero value moves every field. | `^LS0` |

## Positioning and text

| Syntax | Command | Example |
| --- | --- | --- |
| `^FOx,y,z` | **Field Origin** — Places the next field at position x,y measured from the top-left corner. | `^FO100,120` |
| `^FTx,y,z` | **Field Typeset** — Positions the next field from the baseline of the text rather than its top edge. | `^FT100,220` |
| `^FDveri` | **Field Data** — Carries the text or barcode data to be printed. | `^FDMerhaba Dünya` |
| `^FHa` | **Field Hexadecimal Indicator** — Enables _XX hex escapes inside the following ^FD (for special characters). | `^FH^FDTa_C4_9Fdelen^FS` |
| `^Afo,h,w` | **Font Selection** — Selects the font, orientation and size. ^A0 is the scalable font. | `^A0N,50,40` |
| `^FBa,b,c,d,e` | **Field Block** — Lays text out as a word-wrapped block of a given width, with justification. | `^FB600,3,0,C` |
| `^FWr,z` | **Field Orientation** — Sets the default rotation for the fields that follow (N/R/I/B). | `^FWR` |
| `^FR` | **Field Reverse Print** — Prints the next field in reverse: light on a dark background. | `^FR^GB100,100,100^FS` |
| `^FVa` | **Field Variable** — Carries field data as a variable; the engine treats it the same way as ^FD. | `^FO50,50^FVSIPARIS-1^FS` |

## Graphics

| Syntax | Command | Example |
| --- | --- | --- |
| `^GBw,h,t,c,r` | **Graphic Box** — Draws a rectangular box or a line (width, height, thickness). | `^GB200,80,3` |
| `^GCd,t,c` | **Graphic Circle** — Draws a circle (diameter, thickness). | `^GC120,4` |
| `^GDw,h,t,c,o` | **Graphic Diagonal Line** — Draws a diagonal line. | `^GD100,100,3` |
| `^GEw,h,t,c` | **Graphic Ellipse** — Draws an ellipse (width, height, thickness). | `^GE120,80,3` |
| `^GFa,b,c,d,veri` | **Graphic Field** — Prints a bitmap graphic (logo or image) embedded in the label. | `^GFA,50,50,5,...` |
| `^IMd:o.x` | **Image Move** — Places a graphic from memory at the current ^FO position (does the same job as ^XG). | `^FO50,50^IMR:LOGO.GRF^FS` |

## Barcodes

| Syntax | Command | Example |
| --- | --- | --- |
| `^BYw,r,h` | **Barcode Field Defaults** — Sets the module width, ratio and height for the barcodes that follow. | `^BY3,2,90` |
| `^BCo,h,f,g,e,m` | **Code 128** — A Code 128 barcode — the most common linear barcode in shipping and logistics. | `^BCN,100,Y,N,N` |
| `^BEo,h,f,g` | **EAN-13** — An EAN-13 retail product barcode (the last digit is calculated automatically). | `^BEN,90,Y,N` |
| `^BQa,b,c,d` | **QR Code** — A QR code. The data is normally given with the ^FDQA,... prefix. | `^BQN,2,6` |
| `^BXo,h,s,c,r,f,g,a` | **Data Matrix** — A Data Matrix 2D barcode — a lot of data in a very small area. | `^BXN,4,200` |
| `^B3o,e,h,f,g` | **Code 39** — Code 39 — a linear barcode encoding uppercase letters and digits. | `^B3N,N,60,Y,N` |
| `^B7o,h,s,c,r,t` | **PDF417** — A PDF417 stacked barcode — high data capacity across multiple rows. | `^B7N,2,5` |
| `^B1o,e,h,f,g` | **Code 11** — Code 11 (USD-8) — telecom and laboratory labels. | `^B1N,N,100,Y,N^FD12345678^FS` |
| `^B2o,h,f,g,e` | **Interleaved 2/5** — Interleaved 2 of 5 — numeric data in digit pairs. | `^B2N,100,Y,N,N^FD12345678^FS` |
| `^B8o,h,f,g` | **EAN-8** — EAN-8 — short retail code for small packaging. | `^B8N,100,Y,N^FD12345678^FS` |
| `^B9o,h,f,g,e` | **UPC-E** — UPC-E — compressed UPC for small packaging. | `^B9N,100,Y,N,N^FD12345678^FS` |
| `^BAo,h,f,g,e` | **Code 93** — Code 93 — the denser successor to Code 39. | `^BAN,100,Y,N,N^FD12345678^FS` |
| `^BIo,h,f,g` | **Industrial 2/5** — Industrial 2 of 5 — numeric only, industrial use. | `^BIN,100,Y,N^FD12345678^FS` |
| `^BJo,h,f,g` | **Standard 2/5** — Standard 2 of 5 — numeric only. | `^BJN,100,Y,N^FD12345678^FS` |
| `^BKo,e,h,f,g,k,l` | **Codabar** — Codabar — blood banks, libraries, courier labels. | `^BKN,N,100,Y,N,A,A^FD12345678^FS` |
| `^BMo,e,h,f,g,e2` | **MSI** — MSI Plessey — shelf and inventory labels. | `^BMN,N,100,Y,N,N^FD12345678^FS` |
| `^BUo,h,f,g,e` | **UPC-A** — UPC-A — the North American retail standard. | `^BUN,100,Y,N,N^FD12345678^FS` |
| `^BRa,b,c,d,e,f` | **GS1 DataBar** — GS1 DataBar (RSS Expanded) — small retail and fresh food. | `^BRN,1,3,1,100^FD0123456789012^FS` |
| `^B0o,m,c,d,e,f,g` | **Aztec** — Aztec Code — 2D, needs no quiet zone. | `^B0N^FDLABELIXA^FS` |
| `^BOo,m,c,d,e,f,g` | **Aztec** — Aztec Code (^BO spelling) — produces the same symbology as ^B0. | `^BON^FDLABELIXA^FS` |
| `^BFo,h,m` | **MicroPDF417** — MicroPDF417 — PDF417 compressed for narrow space. | `^BFN^FDLABELIXA^FS` |
| `^BDm,n,t` | **MaxiCode** — MaxiCode — the symbology on UPS shipping labels. | `^BD2,1,1^FDLABELIXA^FS` |

## Advanced

| Syntax | Command | Example |
| --- | --- | --- |
| `^SNv,n,z` | **Serialisation Data** — Serialises the field data across a batch with a start value, an increment and padding. | `^SN1000,1,Y` |
| `^PQq,p,r,o,e` | **Print Quantity** — Sets how many copies of the same label are produced. | `^PQ4` |
| `^MUa,b,c` | **Set Units of Measurement** — Selects the unit used by the commands that follow; with a base and target dot density the printer rescales the format between resolutions. | `^MUd,203,300` |
| `^FNn` | **Field Number** — A variable field placeholder in templates (used with ^DF and ^XF). | `^FN1` |
| `^DFd:o.x` | **Download Format** — Stores a label format in memory as a template. | `^DFR:SABLON.ZPL` |
| `^XFd:o.x` | **Recall Format** — Recalls a stored template and fills its ^FN fields with values. | `^XFR:SABLON.ZPL` |
| `^XGd:o.x,mx,my` | **Recall Graphic** — Recalls a graphic loaded into memory (~DG or ^GF) and prints it. | `^XGR:LOGO.GRF,1,1` |
| `~DGd:o.x,t,w,veri` | **Download Graphic** — Loads a bitmap graphic into printer memory (a control command with the ~ prefix). | `~DGR:LOGO.GRF,...` |
| `^DUd:o.x,t,data` | **Download Font** — Loads a TrueType font into virtual printer memory; fields bound to it with ^A@ use this data. | `^DUR:ARIAL.TTF,12345,~*` |
| `^DYd:o,f,x,t,w,data` | **Download Object** — Loads a graphic or font object into virtual printer memory (the general form of ^DG). | `^DYR:LOGO,B,G,1024,64,~*` |
| `^EG` | **Erase Graphics** — Erases ALL graphics from virtual printer memory. | `^EG` |
| `^IDd:o.x` | **Delete Object** — Deletes a SINGLE object from memory (^EG erases all). | `^IDR:LOGO.GRF` |
| `^SFa,b` | **Serialization Field** — Increments field data automatically according to a mask (the mask-based form of ^SN). | `^FO50,50^FD0001^SFdddd,1^FS` |

## RFID

| Syntax | Command | Example |
| --- | --- | --- |
| `^RFo,f,b,n,m` | **RFID Read/Write** — Writes data to or reads data from the RFID tag. It does not affect the preview image; encoding is done by a real RFID printer. | `^RFW,H^FD112233445566778899AABBCC^FS` |
| `^RSt,p,v,n,e` | **RFID Setup** — Configures RFID tag type, programming position and error handling. | `^RS8,,,3,N` |
| `^RBn,p1,p2,...` | **EPC Bit Partitions** — Defines the bit partitions of the EPC data; subsequent ^RF writes use this layout. | `^RB96,8,3,3,20,24,38` |
| `^RRn` | **RFID Retries** — Sets the number of retries for failed RFID read/write operations. | `^RR3` |

## Labelixa extension (preview)

| Syntax | Command | Example |
| --- | --- | --- |
| `~LCrrggbb` | **Label Stock Colour** — Simulates pre-printed or coloured label stock in the preview. Physical printers ignore this command. | `~LCFFF0C0` |

## Runnable examples

Complete labels (not fragments) that lint clean and render without
warnings against the Labelixa engine — paste one into any ZPL viewer:

### `^A`

```zpl
^XA^PW609^LL406^FO40,40^A0N,60,60^FDBIG^FS^FO40,120^A0N,28,28^FDsmall line^FS^XZ
```

### `^B0`

```zpl
^XA^PW609^LL406^FO60,60^B0N,6,N^FDAZTEC-DEMO^FS^XZ
```

### `^B1`

```zpl
^XA^PW609^LL406^FO40,60^BY2^B1N,N,120,Y,N^FD123456^FS^XZ
```

### `^B2`

```zpl
^XA^PW609^LL406^FO40,60^BY2^B2N,120,Y,N,N^FD1234567890^FS^XZ
```

### `^B3`

```zpl
^XA^PW609^LL406^FO40,60^BY2^B3N,N,120,Y,N^FDSKU12345^FS^XZ
```

### `^B7`

```zpl
^XA^PW609^LL406^FO40,60^B7N,5,5,,,N^FDPDF417-DEMO^FS^XZ
```

### `^B8`

```zpl
^XA^PW609^LL406^FO120,60^BY3^B8N,120,Y,N^FD1234567^FS^XZ
```

### `^B9`

```zpl
^XA^PW609^LL406^FO120,60^BY3^B9N,120,Y,N^FD123456^FS^XZ
```

### `^BA`

```zpl
^XA^PW609^LL406^FO40,60^BY2^BAN,120,Y,N,N^FDCODE93DEMO^FS^XZ
```

### `^BC`

```zpl
^XA^PW609^LL406^FO40,60^BY2^BCN,140,Y,N,N^FDCODE128-DEMO^FS^XZ
```

### `^BD`

```zpl
^XA^PW609^LL406^FO60,60^BD2,1,1^FD001840152382802^FS^XZ
```

### `^BE`

```zpl
^XA^PW609^LL406^FO120,60^BY2^BEN,140,Y,N^FD123456789012^FS^XZ
```

### `^BF`

```zpl
^XA^PW609^LL406^FO40,60^BFN,5,1^FDMICROPDF^FS^XZ
```

### `^BI`

```zpl
^XA^PW609^LL406^FO40,60^BY2^BIN,120,Y,N^FD1234567890^FS^XZ
```

### `^BJ`

```zpl
^XA^PW609^LL406^FO40,60^BY2^BJN,120,Y,N^FD1234567890^FS^XZ
```

### `^BK`

```zpl
^XA^PW609^LL406^FO40,60^BY2^BKN,N,120,Y,N,A,A^FDA12345B^FS^XZ
```

### `^BM`

```zpl
^XA^PW609^LL406^FO40,60^BY2^BMN,B,120,Y,N,N^FD1234567^FS^XZ
```

### `^BO`

```zpl
^XA^PW609^LL406^FO60,60^BON,6,N^FDAZTEC-DEMO^FS^XZ
```

### `^BQ`

```zpl
^XA^PW609^LL406^FO180,60^BQN,2,6^FDQA,SAMPLE-QR-DATA^FS^XZ
```

### `^BR`

```zpl
^XA^PW609^LL406^FO40,30^A0N,24,24^FDb=1 Omnidirectional^FS^FO40,60^BRN,1,3,1,90^FD0123456789012^FS^FO40,190^A0N,24,24^FDb=6 Expanded (AI)^FS^FO40,220^BRN,6,3,1,90^FD[01]12345678901231^FS^XZ
```

### `^BU`

```zpl
^XA^PW609^LL406^FO100,60^BY3^BUN,120,Y,N^FD12345678901^FS^XZ
```

### `^BX`

```zpl
^XA^PW609^LL406^FO60,60^BXN,6,200^FDDATAMATRIX-DEMO^FS^XZ
```

### `^BY`

```zpl
^XA^PW609^LL406^FO40,60^BY3^BCN,140,Y,N,N^FDBY-DEMO-123^FS^XZ
```

### `^CF`

```zpl
^XA^PW609^LL406^CF0,36^FO40,40^FDLINE ONE^FS^FO40,90^FDLINE TWO^FS^XZ
```

### `^CI`

```zpl
^XA^PW609^LL406^CI28^FO40,60^A0N,40,40^FDGuvenli Olcum - Grosse^FS^XZ
```

### `^FB`

```zpl
^XA^PW609^LL406^FO40,50^A0N,28,28^FB520,4,0,L,0^FDUzun bir metin blogu otomatik olarak satirlara bolunur ve verilen genislige sigar.^FS^XZ
```

### `^FD`

```zpl
^XA^PW609^LL406^FO50,50^A0N,40,40^FDHELLO LABEL^FS^XZ
```

### `^FH`

```zpl
^XA^PW609^LL406^FO40,60^A0N,40,40^FH^FD_48_45_58 HEX^FS^XZ
```

### `^FO`

```zpl
^XA^PW609^LL406^FO50,50^A0N,40,40^FDSAMPLE TEXT^FS^XZ
```

### `^FR`

```zpl
^XA^PW609^LL406^FO30,30^FR^GB540,110,110^FS^FO60,60^A0N,50,50^FDREVERSED^FS^XZ
```

### `^FS`

```zpl
^XA^PW609^LL406^FO40,40^A0N,40,40^FDBIRINCI^FS^FO40,120^A0N,40,40^FDIKINCI^FS^XZ
```

### `^FT`

```zpl
^XA^PW609^LL406^FT40,200^A0N,44,44^FDTYPESET TABANI^FS^XZ
```

### `^FV`

```zpl
^XA^PW609^LL406^FO40,60^A0N,40,40^FVDEGISKEN ALAN^FS^XZ
```

### `^FW`

```zpl
^XA^PW609^LL406^FWR^FO60,40^A0N,40,40^FDDONDURULMUS^FS^FWN^FO300,40^A0N,40,40^FDNORMAL^FS^XZ
```

### `^FX`

```zpl
^XA^PW609^LL406^FXBu bir yorumdur ve basilmaz^FS^FO40,60^A0N,40,40^FDYORUM SONRASI^FS^XZ
```

### `^GB`

```zpl
^XA^PW609^LL406^FO30,30^GB540,340,4^FS^FO60,60^GB200,100,2^FS^XZ
```

### `^GC`

```zpl
^XA^PW609^LL406^FO120,60^GC220,6,B^FS^XZ
```

### `^GD`

```zpl
^XA^PW609^LL406^FO60,60^GD400,240,6,B,R^FS^XZ
```

### `^GE`

```zpl
^XA^PW609^LL406^FO60,60^GE400,240,6,B^FS^XZ
```

### `^LH`

```zpl
^XA^PW609^LL406^LH30,20^FO40,60^A0N,40,40^FDBASLANGIC KAYDIRILDI^FS^XZ
```

### `^LL`

```zpl
^XA^PW609^LL300^FO40,60^A0N,40,40^FDUZUNLUK 300^FS^XZ
```

### `^LR`

```zpl
^XA^PW609^LL406^FO30,30^GB540,120,120^FS^LRY^FO60,60^A0N,50,50^FDTERS^FS^LRN^XZ
```

### `^LS`

```zpl
^XA^PW609^LL406^LS20^FO40,60^A0N,40,40^FDSOLA KAYDIRMA^FS^XZ
```

### `^PW`

```zpl
^XA^PW480^LL406^FO40,60^A0N,40,40^FDGENISLIK 480^FS^XZ
```

### `^XA`

```zpl
^XA^PW609^LL406^FO40,60^A0N,40,40^FDFORMAT BASLANGICI^FS^XZ
```

### `^XZ`

```zpl
^XA^PW609^LL406^FO40,60^A0N,40,40^FDFORMAT SONU^FS^XZ
```

## Notes on honesty

- Dots, not millimetres: every coordinate depends on the printer's
  resolution. The same file prints ~1.5x smaller when a 203 dpi label
  lands on a 300 dpi head.
- Commands marked as printer-side (media tracking, darkness, `^LT` top
  offset) change physical behaviour a software preview cannot fully
  simulate.
- EAN-13 wants exactly 12 digits (the printer computes the check digit);
  classic Code 39 has no lowercase.

---

Try any of these live — paste, render, lint: the free
[ZPL viewer at labelixa.com](https://labelixa.com/tools/zpl-preview).
