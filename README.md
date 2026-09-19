# CPSC 1710 — Lab 2: From One Pixel to Your Classifier

**Croissant Check** hands you twelve unlabeled croissant photos and asks *you*
to sort them into undercooked, perfect, and burnt. It then trains a tiny
classifier on **your** labels, and you can upload a new croissant to see how it
gets judged.

Like One Pixel ML, the measurements are already there and the labels come from
the visitor. Label the twelve differently and you get a different machine out of
the same twelve photos.

![The Croissant Check page](images/croissant-check-page.png)

## What is here

| File | Purpose |
| --- | --- |
| `croissant-check.html` | **My classifier.** Open this one. |
| `one-pixel.html` | Provided starter page, unchanged, for comparison. |
| `images/dataset/` | The twelve labeled croissant photos. |
| `README.md` | This file: project notes, development log, and reflection. |

## How to open and use the page

No installation, no build step, no accounts, no internet connection.

1. Download or clone this repository.
2. **Double-click `croissant-check.html`.** It opens in Chrome, Edge, Safari, or
   Firefox. You can also drag the file onto a browser window.

Avoid VS Code's **Open Preview** — its internal `file+.vscode-resource` links do
not behave like normal browser addresses. Open the file in a real browser.

### How to test it

**Step 1 — you label the twelve.** Each photo shows the two numbers your browser
just measured from its pixels, and three buttons: Under, Perfect, Burnt. Click
one on every card. Clicking the same button again clears it. A progress bar
tracks you, and the train button stays greyed out until all twelve are labeled
with at least two different labels.

**Step 2 — train on your labels.** Press **"Train on my labels"**. A chart
appears: every photo is a dot in your colour, and the pale regions are the
boundaries your labels produced. Underneath, it tells you how many of the twelve
you and I agreed on, and lists any we disagreed about. Neither of us is correct —
they are opinions about baking.

**Step 3 — judge a new croissant.** This section is locked until you train.
Then you can:
- **Drop a photo** on the dashed box, or click it to pick a file. Nothing is
  uploaded anywhere; it never leaves your computer.
- Press **🎲 Use one of the twelve** to re-test a training photo.
- **Drag the two dials** to set brightness and char by hand, with no photo at
  all. The verdict updates live as you drag.

The **blue ring** on the chart shows where your croissant landed.

Three experiments worth running, which the page also lists at the bottom:

| Kind of case | What to do | What happens |
| --- | --- | --- |
| Teach it wrong | Label the twelve **backwards** — pale ones Burnt, dark ones Under — then re-train | It cheerfully agrees with you. A charred croissant now reads *Undercooked* |
| Close call | Set char to 5%, then slide brightness to **121** | The verdict flips Perfect → Undercooked with scores at 46% vs 44% — the classifier admitting it is guessing |
| Strange | Upload something that is not a croissant | It answers confidently anyway — see the limitation below |

The whole page is one file. The twelve photos are stored inside it, so it works
offline, from a USB stick, or emailed to someone.

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

### How it makes a prediction (in plain language)

1. **Measure.** The photo is shrunk to a small grid and the middle of it — where
   the croissant sits — is scanned pixel by pixel. Two numbers come out: the
   average brightness of those pixels, and the percentage of them that are very
   dark, which the page calls *char*.
2. **Learn.** The visitor labels the twelve. For each label they used, the page
   averages those photos' two numbers into one "typical" croissant. Label them
   the way I did and you get these three:

   | Label | Typical brightness | Typical char |
   | --- | --- | --- |
   | Undercooked | 141 | 4.6% |
   | Perfect | 104 | 10.8% |
   | Burnt | 74 | 41.1% |

3. **Decide.** A new photo is measured the same way and gets whichever typical
   point it lands nearest. The confidence bars are just how much nearer the
   winner was than the others.

Labeled my way, it gets **12 out of 12** of the training photos right — which
sounds impressive until you remember it has seen all twelve.

### One limitation I found

The page never learns what a croissant *is*, and it has no opinion of its own
about baking. Two ways to see this:

- **It believes whatever you teach it.** Label the twelve backwards — pale ones
  Burnt, charred ones Undercooked — re-train, and a blackened croissant comes
  back **Undercooked**. It has no idea what the words mean; it just copies the
  boundary you drew.
- **It does not need a croissant at all.** A plain grey rectangle returns
  **Perfect** with high confidence. A dark rectangle returns **Burnt**, a white
  one **Undercooked**. Shape, layers, and crumb are invisible to it — it only
  ever sees two numbers about colour.

It also has no way to say "I don't know," so it always commits to one of the
labels no matter what you show it.

## Development log

Moments where I directed the work, in order:

1. I started with a cookie idea and switched it to croissants, because the
   failure modes I actually care about are dryness and crustiness.
2. I noticed that "too fluffy" and "not crusty enough" were describing the same
   thing, so I collapsed four labels into three: undercooked, perfect, burnt.
   That also let me drop butter as an input, since butter does not change
   whether something is cooked.
3. I generated twelve photos myself — four per label — instead of using sliders
   for oven temperature and time. That changed the project: the classifier now
   measures real pixels from a photo rather than reading numbers I typed.
4. Once the page worked, I realised I had built the wrong thing: it was handing
   visitors my labels instead of asking for theirs. I had it rebuilt so the
   visitor labels all twelve photos first, trains on their own answers, and only
   then uploads a croissant to judge. That is much closer to One Pixel ML, where
   the numbers are given and the labels come from you.
5. _(add your own here as you keep working)_

## Reflection

_Written by me, without AI assistance, at the end of the lab._

## Credits

The `one-pixel.html` starter page is by
[Xiuye Chen](https://github.com/xiuyechen), from the
[CPSC 1710 labs](https://xiuyechen.github.io/cpsc1710-labs/), and is shared
under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). My additions to
this repository are coursework for CPSC 1710, Fall 2026.
