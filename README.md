# CPSC 1710 — Lab 2: From One Pixel to Your Classifier

Starter project for Lab 2. Right now this repository holds the unmodified
`one-pixel.html` starter page; the classifier I build on top of it will be added
as a second page later in the lab.

## What is here

| File | Purpose |
| --- | --- |
| `one-pixel.html` | Provided starter page. Open it to see how a single pixel becomes a prediction. |
| `README.md` | This file: project notes, development log, and reflection. |

## How to open and use the page

There is no build step and nothing to install.

1. Download or clone this repository.
2. Double-click `one-pixel.html`, or drag it into Chrome, Safari, or Firefox.

Avoid VS Code's **Open Preview** for this file — its internal
`file+.vscode-resource` links do not behave like normal browser addresses.

## My classifier

**Croissant Check** — a tiny classifier that looks at a photo of a croissant
and guesses whether it came out undercooked, perfect, or burnt.

### The idea (Stop 3)

1. **My page will classify** photographs of croissants by how well they were
   baked.
2. **The possible labels are** Undercooked, Perfect, and Burnt.
3. **The classifier will look at** two numbers measured from the photo itself:
   - **Crust brightness** — how light or dark the croissant is on average
   - **Char** — the percentage of pixels dark enough to count as burnt
4. **One example it can learn from is** `perfect-2.jpg`, which has brightness
   111 and 9% char, and which I label *Perfect*. Compare `undercooked-3.jpg` at
   brightness 159 with almost no char (1.7%), which I label *Undercooked*.
5. **A visitor should understand that** the page is not recognising a croissant.
   It only measures two colour numbers and compares them to the twelve examples
   I labeled, so a dark photo of almost anything would come back *Burnt*. The
   boundaries reflect my opinions about baking, not a rule of pastry, and photos
   near a boundary are close calls where the page is least trustworthy.

This is the same shape as One Pixel ML — twelve labeled examples, and a
brightness number — just with a real photo instead of a single pixel, a second
measurement alongside brightness, and three labels instead of two.

The labels are in order, which makes mistakes easy to talk about: calling an
Undercooked croissant *Perfect* is a near miss, while calling it *Burnt* is a
real failure.

### The twelve examples

Four photos per label, in [`images/dataset/`](images/dataset/), split out of the
three grids I generated. Measured values:

| Label | Crust brightness | Char |
| --- | --- | --- |
| Undercooked | 127 – 159 | 1.7% – 6.2% |
| Perfect | 100 – 111 | 9.2% – 13.2% |
| Burnt | 64 – 84 | 36.2% – 50.3% |

The three groups do not overlap on either measurement, so even a simple
boundary should separate them. Brightness alone nearly does the whole job; char
is what makes *Burnt* unmistakable.

- **How it makes a prediction (in plain language):** _to fill in after building_
- **One limitation I found:** _to fill in after testing_

## Development log

Moments where I directed the work, in order:

1.
2.
3.

## Reflection

_Written by me, without AI assistance, at the end of the lab._

## Credits

The `one-pixel.html` starter page is by
[Xiuye Chen](https://github.com/xiuyechen), from the
[CPSC 1710 labs](https://xiuyechen.github.io/cpsc1710-labs/), and is shared
under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). My additions to
this repository are coursework for CPSC 1710, Fall 2026.
