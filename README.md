# go-txtr

Package **txtr** implements encoding and decoding of a Unicode **text**-based **transmission** protocol, for the Go programming language.

## Documention

Online documentation, which includes examples, can be found at: http://godoc.org/github.com/reiver/go-txtr

[![GoDoc](https://godoc.org/github.com/reiver/go-txtr?status.svg)](https://godoc.org/github.com/reiver/go-txtr)

## Text Transmission

Built into Unicode (and ASCII) are a number of **tranmission** **control codes**:

| Symbol | Name                      | Abbreviation | Hexadecimal | Decimal | Caret | UTF-8        |
|--------|---------------------------|--------------|-------------|---------|-------|--------------|
| ␁      | Start of Heading          | `SOH`        | 0x1         |  1      | ^A    | `0b00000001` |
| ␂      | Start of Text             | `STX`        | 0x2         |  2      | ^B    | `0b00000010` |
| ␃      | End of Text               | `ETX`        | 0x3         |  3      | ^C    | `0b00000011` |
| ␄      | End of Transmission       | `EOT`        | 0x4         |  4      | ^D    | `0b00000100` |
| ␅      | Enquiry                   | `ENQ`        | 0x5         |  5      | ^E    | `0b00000101` |
| ␆      | Acknowledge               | `ACK`        | 0x6         |  6      | ^F    | `0b00000110` |
| ␐      | Data Link Escape          | `DLE`        | 0x10        | 16      | ^P    | `0b00010000` |
| ␕      | Negative Acknowledge      | `NAK`        | 0x15        | 21      | ^U    | `0b00010101` |
| ␖      | Synchronous Idle          | `SYN`        | 0x16        | 22      | ^V    | `0b00010110` |
| ␗      | End of Transmission Block | `ETB`        | 0x17        | 23      | ^W    | `0b00010111` |

Also, by tradition (i.e,. a de facto standard), Unicode (and ASCII) `DC1` & `DC3` are treated as `XON` & `XOFF` respectively.

| Symbol | Name                      | Alternative Name | Abbreviation |Alt. Abbr. | Hexadecimal | Decimal | Caret | UTF-8        |
|--------|---------------------------|------------------|--------------|-----------|-------------|---------|-------|--------------|
| ␑      | Device Control One        | Transmit Off     | `DC1`        | `XON`     | 0x11        | 17      | ^Q    | `0b00010001` |
| ␓      | Device Control Three      | Transmit On      | `DC3`        | `XOFF`    | 0x13        | 19      | ^S    | `0b00010011` |


So, the full list (which adds `XON` & `XOFF`) of **tranmission** **control codes** would be:

| Symbol | Name                      | Alternative Name | Abbreviation |Alt. Abbr. | Hexadecimal | Decimal | Caret | UTF-8        |
|--------|---------------------------|------------------|--------------|-----------|-------------|---------|-------|--------------|
| ␁      | Start of Heading          |                  | `SOH`        |           | 0x1         |  1      | ^A    | `0b00000001` |
| ␂      | Start of Text             |                  | `STX`        |           | 0x2         |  2      | ^B    | `0b00000010` |
| ␃      | End of Text               |                  | `ETX`        |           | 0x3         |  3      | ^C    | `0b00000011` |
| ␄      | End of Transmission       |                  | `EOT`        |           | 0x4         |  4      | ^D    | `0b00000100` |
| ␅      | Enquiry                   |                  | `ENQ`        |           | 0x5         |  5      | ^E    | `0b00000101` |
| ␆      | Acknowledge               |                  | `ACK`        |           | 0x6         |  6      | ^F    | `0b00000110` |
| ␐      | Data Link Escape          |                  | `DLE`        |           | 0x10        | 16      | ^P    | `0b00010000` |
| ␑      | Device Control One        | Transmit Off     | `DC1`        | `XON`     | 0x11        | 17      | ^Q    | `0b00010001` |
| ␓      | Device Control Three      | Transmit On      | `DC3`        | `XOFF`    | 0x13        | 19      | ^S    | `0b00010011` |
| ␕      | Negative Acknowledge      |                  | `NAK`        |           | 0x15        | 21      | ^U    | `0b00010101` |
| ␖      | Synchronous Idle          |                  | `SYN`        |           | 0x16        | 22      | ^V    | `0b00010110` |
| ␗      | End of Transmission Block |                  | `ETB`        |           | 0x17        | 23      | ^W    | `0b00010111` |

Some of the Unicode (and ASCII) **tranmission** **control codes** are used by this package (package **txtr**) for its Unicode **text**-based **transmission** protocol.

## Escaping

Also, it is necessary to have an "escape" character, in case certain of these **control codes** appear in the heading, payload, or trailer.

The **control code used for that is `ESC`:

| Symbol | Name                      | Alternative Name | Abbreviation |Alt. Abbr. | Hexadecimal | Decimal | Caret | UTF-8        |
|--------|---------------------------|------------------|--------------|-----------|-------------|---------|-------|--------------|
| ␛      | Escape                    |                  | `ESC`        |           | 0x1b        | 27      | ^[    | `0b00011011` |

So the total list of relevant Unicode (and ASCII) **control codes** is:

| Symbol | Name                      | Alternative Name | Abbreviation |Alt. Abbr. | Hexadecimal | Decimal | Caret | UTF-8        |
|--------|---------------------------|------------------|--------------|-----------|-------------|---------|-------|--------------|
| ␁      | Start of Heading          |                  | `SOH`        |           | 0x1         |  1      | ^A    | `0b00000001` |
| ␂      | Start of Text             |                  | `STX`        |           | 0x2         |  2      | ^B    | `0b00000010` |
| ␃      | End of Text               |                  | `ETX`        |           | 0x3         |  3      | ^C    | `0b00000011` |
| ␄      | End of Transmission       |                  | `EOT`        |           | 0x4         |  4      | ^D    | `0b00000100` |
| ␅      | Enquiry                   |                  | `ENQ`        |           | 0x5         |  5      | ^E    | `0b00000101` |
| ␆      | Acknowledge               |                  | `ACK`        |           | 0x6         |  6      | ^F    | `0b00000110` |
| ␐      | Data Link Escape          |                  | `DLE`        |           | 0x10        | 16      | ^P    | `0b00010000` |
| ␑      | Device Control One        | Transmit Off     | `DC1`        | `XON`     | 0x11        | 17      | ^Q    | `0b00010001` |
| ␓      | Device Control Three      | Transmit On      | `DC3`        | `XOFF`    | 0x13        | 19      | ^S    | `0b00010011` |
| ␕      | Negative Acknowledge      |                  | `NAK`        |           | 0x15        | 21      | ^U    | `0b00010101` |
| ␖      | Synchronous Idle          |                  | `SYN`        |           | 0x16        | 22      | ^V    | `0b00010110` |
| ␗      | End of Transmission Block |                  | `ETB`        |           | 0x17        | 23      | ^W    | `0b00010111` |
| ␛      | Escape                    |                  | `ESC`        |           | 0x1b        | 27      | ^[    | `0b00011011` |

## Message

This package's Unicode **text**-based **transmission** protocol is a **message** based protocol.

Each message looks like:

```
DLE SOH
	<heading>
DLE STX
	<payload>
DLE ETX
	<trailer>
DLE ETB
```
