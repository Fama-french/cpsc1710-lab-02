# Croissant Check 🥐

**A tiny classifier that judges croissants — but only after you teach it what
the words mean.**

It hands you twelve unlabeled croissant photos and asks *you* to sort them into
undercooked, perfect, and burnt. It then trains on **your** labels, and you can
upload a new croissant to see how it gets judged. Label the twelve differently
and you get a different machine out of the same twelve photos.

**▶ Try it live: <https://fama-french.github.io/cpsc1710-lab-02/croissant-check.html>**

Nothing is uploaded anywhere — the photo you test never leaves your browser.

![The Croissant Check page](images/croissant-check-page.png)

Built for CPSC 1710, Lab 2, on top of the provided One Pixel ML starter. Like
One Pixel ML, the measurements come free and the labels come from the visitor.

---

### Contents

1. [What it is and what it is for](#1-what-it-is-and-what-it-is-for)
2. [How to open and use the page](#2-how-to-open-and-use-the-page)
3. [How it makes a prediction](#3-how-it-makes-a-prediction-in-plain-language)
4. [The limitation I found](#4-the-limitation-i-found)
5. [Development log](#5-development-log)

Supporting detail: [the idea](#the-idea-stop-3) ·
[the twelve examples](#the-twelve-examples) · [testing](#testing-it-stop-5) ·
[reflection](#reflection)

---

## 1. What it is and what it is for

**Name:** Croissant Check.

**Purpose:** to show that a classifier has no opinions of its own. It does not
know what a croissant is, what "burnt" means, or that baking exists. It measures
three numbers about colour, copies whatever boundary you draw between your
labels, and then applies that boundary to anything you hand it — confidently,
whether or not it should be.

The croissants are the excuse. The point is to make a visitor feel the moment
where *their* judgement becomes the machine's rule.

| File | What it is |
| --- | --- |
| `croissant-check.html` | **The classifier.** One self-contained file — open this one. |
| `one-pixel.html` | The provided starter page, unchanged, for comparison. |
| `images/dataset/` | The twelve croissant photos. |
| `images/` | Screenshots. |

---

## 2. How to open and use the page

No installation, no build step, no accounts, no API keys, and no internet
connection required.

1. Download or clone this repository.
2. **Double-click `croissant-check.html`.** It opens in Chrome, Edge, Safari, or
   Firefox. You can also drag the file onto a browser window, or use the live
   link above.

> Avoid VS Code's **Open Preview** — its internal `file+.vscode-resource` links
> do not behave like normal browser addresses. Open it in a real browser.

The whole page is one file with the twelve photos stored inside it, so it also
works offline, from a USB stick, or emailed to someone.

### Using it

**Step 1 — label the twelve.** The photos are shuffled on every load, so their
order gives nothing away. Each shows the three numbers your browser has just
measured from its pixels, and three buttons: Under, Perfect, Burnt. Click one on
every card; clicking the same button again clears it. A short guide above the
grid describes what each label looks like. The train button stays greyed out
until all twelve are labeled with at least two different labels.

**Step 2 — train on your labels.** Press **"Train on my labels"**. A chart
appears: every photo is a dot in your colour, and the pale regions are the
boundaries your labels produced. Underneath, it reports how many of the twelve
you and I agreed on and lists any we did not. Neither of us is correct — these
are opinions about baking.

**Step 3 — judge a new croissant.** Locked until you train. Then you can:

- **Drop a photo** on the dashed box, or click it to choose a file.
- Press **🎲 Use one of the twelve** to re-test a training photo.
- **Drag the three dials** to set warmth, char, and brightness by hand with no
  photo at all. The verdict updates live, and the chart redraws as a slice at
  whatever brightness you choose.

The **blue ring** on the chart shows where your croissant landed. **Clear
result** removes a verdict; **Start over** resets everything.

### Four experiments worth running

| Kind of case | What to do | What happens |
| --- | --- | --- |
| Teach it wrong | Label the twelve **backwards** — pale ones Burnt, charred ones Under — and re-train | It agrees with you. A blackened croissant now reads **Undercooked** |
| Close call | Set char to 5% and brightness to 104, then slide crust warmth slowly | The verdict flips Undercooked → Perfect at warmth **1.815**, scores read 41% / 41%, and the page says *"too close to call"* |
| Strange | Upload something that is not a croissant | Grey reads *Undercooked*, brick reads *Perfect*, black reads *Burnt* — each now with a warning |
| The one that gets past the warning | Upload a face, or anything tan: cardboard, a wooden table, a paper bag | Judged with **no warning at all**. See the limitation below |

---

## 3. How it makes a prediction (in plain language)

**1. Measure.** The photo is shrunk to a small grid and the middle of it — where
the croissant sits — is scanned pixel by pixel. Pixels that are bright *and*
colourless are treated as backdrop and skipped, so a cut-out product shot on a
white studio background is not mistaken for a pale croissant. Three numbers come
out of what is left:

- **Warmth** — how much redder than bluer the crust is. Raw dough is nearly
  neutral; a golden croissant is strongly red-over-blue. Turning the lights up
  scales red and blue together, so this number barely moves between a dim bakery
  and a bright studio.
- **Char** — the share of pixels dark enough to read as burnt.
- **Brightness** — the plain average lightness. Useful, but the least
  trustworthy of the three, because it also records how bright the room was.

**2. Learn.** You label the twelve. For each label you used, the page averages
those photos' three numbers into one *typical croissant*. Label them the way I
did and you get these three:

| Label | Typical warmth | Typical char | Typical brightness |
| --- | --- | --- | --- |
| Undercooked | 1.95 | 4.7% | 139 |
| Perfect | 2.24 | 10.9% | 104 |
| Burnt | 2.16 | 41.4% | 74 |

**3. Decide.** A new photo is measured the same way and takes whichever typical
croissant it lands nearest. The confidence bars are simply how much nearer the
winner was than the others.

That is the whole algorithm: three numbers, twelve examples, nearest match.

**Does it work?** On the twelve training photos it scores **12 out of 12** —
which proves little, since it has seen all twelve. The fairer test is
leave-one-out: hide one photo, retrain on the other eleven, then classify the
hidden one. That also scores **12 out of 12**.

**Reading the chart.** It can only draw two of the three numbers, so it shows a
flat slice through the third. The caption tells you which brightness the slice
was cut at, and moving the brightness dial shifts the whole map.

---

## 4. The limitation I found

**Anything golden-brown is a croissant to it.**

A photo of a face is judged **Undercooked at 69% confidence, with no warning at
all.** A mid skin tone measures warmth 1.84, char 0%, brightness 152 — every one
of those sits *inside* the range of my twelve croissants. Deeper skin tones
measure warmth 3.81 and come back **Perfect**. Cardboard, tan walls, and wooden
tables do the same thing.

This is the limitation I cannot fix with a better threshold. The page warns you
when a photo's **numbers** are unusual, but a face is not numerically unusual —
it is unusual for reasons three colour statistics cannot see. Croissant crust and
human skin are both golden-brown, so to this classifier they are the same object.
Shape, layers, and crumb do not exist in its world.

I found this because the first classmate who tried the page uploaded a selfie and
was told he was a perfect croissant. Rather than hide it, the page now says so at
the upload box and invites visitors to try it.

<details>
<summary>Three more limitations worth knowing</summary>

- **It believes whatever you teach it.** Label the twelve backwards and re-train:
  a blackened croissant comes back **Undercooked**. It has no idea what the words
  mean; it copies the boundary you drew.
- **It never needs a croissant.** A plain grey rectangle returns *Undercooked*, a
  red-brick colour returns *Perfect*, a black one returns *Burnt*. It warns on
  all three now, but it still answers.
- **All twelve photos are the same kind of photo** — dim indoor bakery scenes
  from one image generator. The first real croissant photo I tried from the web,
  a brightly-lit studio shot, was called *Undercooked*. Twelve examples from one
  source is a narrow view of the world.

It also has no way to say "I don't know" outright. It always commits to a label.

</details>

---

## 5. Development log

Moments where I directed the work, in order:

1. I started with a cookie idea and switched it to croissants, because the
   failure modes I actually care about are dryness and crustiness.
2. I noticed "too fluffy" and "not crusty enough" were describing the same thing,
   so I collapsed four labels into three: undercooked, perfect, burnt. That also
   let me drop butter as an input, since butter does not change whether something
   is cooked.
3. I generated twelve photos myself — four per label — instead of using sliders
   for oven temperature and time. That changed the project: the classifier now
   measures real pixels rather than numbers I typed.
4. Once the page worked, I realised I had built the wrong thing: it was handing
   visitors my labels instead of asking for theirs. I had it rebuilt so the
   visitor labels all twelve first, trains on their own answers, and only then
   uploads a croissant. That is much closer to One Pixel ML.
5. I uploaded a real croissant photo from the web and it came back *Undercooked*
   when it was obviously perfect. Rather than accept it, I asked why. It was a
   product shot on a white background, and the page was averaging the backdrop
   into its brightness reading. Two fixes: bright colourless pixels are ignored
   as backdrop, and the main measurement changed from raw brightness to **crust
   warmth**, which barely moves when the lighting changes. The photo now reads
   *Perfect*, and leave-one-out stayed at 12/12.
6. I noticed the twelve were shown grouped — four undercooked, then four perfect,
   then four burnt — which handed the visitor the answer. They are now shuffled
   on every load.
7. I tested the page instead of only looking at it. A grey rectangle was
   confidently called a croissant, and a verdict sitting exactly on a boundary
   was displayed as loudly as a confident one. Both are now flagged in the page.
8. A classmate tried it cold. He said the hard part was labelling the twelve, not
   understanding the prediction — the opposite of what I expected — and his
   selfie was declared a perfect croissant. I added a labelling guide for the
   first problem, and for the second I put the finding into the page itself,
   since a classifier that confidently judges a face is the clearest evidence of
   what it is really doing.

---

## The idea (Stop 3)

1. **My page will classify** photographs of croissants by how well they were
   baked.
2. **The possible labels are** Undercooked, Perfect, and Burnt.
3. **The classifier will look at** three numbers measured from the photo itself:
   crust warmth (how much redder than bluer), char (share of very dark pixels),
   and crust brightness.
4. **One example it can learn from is** `perfect-2.jpg`, which measures warmth
   2.15, char 9.3%, brightness 110, labeled *Perfect*. Compare
   `undercooked-3.jpg` at warmth 1.54 with almost no char (1.7%), labeled
   *Undercooked*.
5. **A visitor should understand that** the page is not recognising a croissant.
   It measures three colour numbers and compares them to twelve labeled
   examples, so a dark photo of almost anything comes back *Burnt*. The
   boundaries reflect somebody's opinions about baking, not a rule of pastry, and
   photos near a boundary are close calls where the page is least trustworthy.

This is the same shape as One Pixel ML — twelve examples whose numbers are given,
and labels that come from you — with a real photo instead of a single pixel,
three measurements instead of one, and three labels instead of two.

The labels are in order, which makes mistakes easy to talk about: calling an
Undercooked croissant *Perfect* is a near miss; calling it *Burnt* is a real
failure.

## The twelve examples

Four photos per label in [`images/dataset/`](images/dataset/), split out of three
grids I generated. Measured values:

| Label | Warmth | Char | Brightness |
| --- | --- | --- | --- |
| Undercooked | 1.54 – 2.10 | 1.7% – 6.1% | 126 – 153 |
| Perfect | 2.15 – 2.33 | 8.9% – 13.0% | 98 – 110 |
| Burnt | 1.77 – 2.68 | 36.0% – 49.9% | 62 – 81 |

Char makes *Burnt* unmistakable. Warmth is what separates undercooked from
perfect, and unlike brightness it barely moves when the lighting changes. Note
how narrow *Perfect* is — a 0.18 span of warmth, against 0.56 and 0.91 for the
others. It is a thin strip squeezed between two failure modes, which is part of
why an unfamiliar photo so easily misses it.

## Testing it (Stop 5)

All three cases were run in the page, labelling the twelve my way first.

**1. Easy case.** `perfect-3.jpg` — warmth 2.29, char 13.0%, brightness 101 →
**Perfect, 81%**. *This is the highest confidence the page ever gives, and it
should be: that photo is one of the four that defined what "perfect" means, so it
sits almost on top of its own group's centre. Scoring well here proves little.*

**2. Close case.** No photo — dials at char 5%, brightness 104, crust warmth
walked slowly upward. The verdict flips **Undercooked → Perfect at warmth
1.815**, where scores read 41% / 41% / 18%. *A gap of essentially zero from
moving one dial by 0.005, yet the verdict printed as large and as confidently as
the 81% case — so I added a "too close to call" note whenever the top two are
within 3 points.*

**3. Strange case.** A plain grey rectangle — warmth 0.99, char 0%, brightness
107 → **Undercooked**, originally with no warning. *The page had no concept of
"that is not a croissant"; red brick came back Perfect for the same reason. So I
added a check on whether each number falls outside the range of everything
labeled. Grey now says "its warmth is 0.99, below anything you labeled (1.54 to
2.68)".*

The new warning does not cry wolf: **0 false alarms** across the twelve, since
holding each photo out never puts it more than 0.32 outside the range while the
nearest non-croissant was 0.48 out.

### Showing it to a classmate

I sent a classmate the link with no explanation.

| Question | What he said |
| --- | --- |
| What can you change? | _to fill in_ |
| What is it doing? | _to fill in_ |
| What is confusing? | The classification was hard — deciding which label to give each of the twelve photos, especially the middle ones |

He also uploaded a photo of himself and was told he was a **perfect croissant**.

I had assumed labelling was the easy warm-up and the prediction was the
interesting part. He found the opposite: choosing between *undercooked* and
*perfect* for a borderline photo is genuinely hard, and the page offered no help
and no reassurance that hesitating was normal.

**Improvements made after his feedback:**

1. **A labelling guide** above the twelve photos describing what each label looks
   like, and saying plainly that several photos sit between two labels and that
   disagreeing about them is the task rather than a mistake.
2. **The page now owns the selfie result** — the upload box warns that it will
   judge anything and sound certain, and a fourth experiment invites visitors to
   try a face or anything tan and watch the warning fail to fire.

## Reflection

_To be written by me, without AI assistance, at the end of the lab._

## Credits

The `one-pixel.html` starter page is by
[Xiuye Chen](https://github.com/xiuyechen), from the
[CPSC 1710 labs](https://xiuyechen.github.io/cpsc1710-labs/), shared under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). My additions are
coursework for CPSC 1710, Fall 2026.
