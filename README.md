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

**Croissant Check** — a tiny classifier that guesses how a batch of croissants
turned out from three baking numbers.

### The idea (Stop 3)

1. **My page will classify** a batch of home-baked croissants by how they turned
   out, using three numbers from the recipe and the oven.
2. **The possible labels are** Just right, Too dry, Too fluffy, and
   Not crusty enough.
3. **The classifier will look at** three inputs:
   - **Butter** — grams of butter per 100 g of flour (10–60)
   - **Proofing time** — minutes rising at room temperature (30–180)
   - **Bake time** — minutes in a 200 °C oven (8–26)
4. **One example it can learn from is** butter 50, proofing 120 minutes, bake
   18 minutes, which I label *Just right*. Drop the butter to 15 and keep
   everything else the same, and I label that one *Too dry*.
5. **A visitor should understand that** the page has never tasted a croissant.
   It only learned the boundaries between labels from the handful of examples I
   labeled myself, so the regions it draws reflect my opinions about baking, not
   a rule of pastry. Batches near a boundary are close calls, and the page is
   least trustworthy there.

Each label comes from one variable drifting out of range: too little butter
makes it dry, too long proofing makes it bready and fluffy instead of layered,
and too short a bake leaves it pale and soft rather than crusty.

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
