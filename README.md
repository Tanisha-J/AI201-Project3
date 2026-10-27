# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

TakeMeter sorts comments from **r/nba**, Reddit's main basketball community,
by what kind of contribution they make. Every comment gets one of three
labels: `evidence` (a claim backed by something you could check), `take` (an
opinion or prediction with nothing checkable behind it), or `banter` (jokes,
reactions and chatter that aren't trying to persuade anyone). The point is to
separate the comments that bring something verifiable to a thread from the
much larger volume of confident opinion and noise around them.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `evidence`

**Definition:** Makes a claim about players, teams or the league and backs it
with something a reader could check: a stat, date, score, contract or trade
detail, or named source.

**Example 1:**
> Lou Williams never scored more than 21 points in a 4th quarter for the Clippers. Took me 20 seconds.

**Example 2:**
> Nigeria to Heat is a serious pipeline, in 2021 we had 6 players who’s had a parent from Nigeria. Gabe Vincent, Precious Achiuwa, Bam, Victor Oladipo, Andre Iguodala, Kz Okpala.

### `take`

**Definition:** States an opinion or prediction about players, teams or the
league, with no checkable fact doing the arguing.

**Example 1:**
> Lmao if KD proved anything it's that he will never win a ring outside of GSW. Elite scorer but that's where it ends. Alwayzs #2.

**Example 2:**
> He's a dirty player that purposely tries to injure opponents. Funny tweets don't make up for that.

### `banter`

**Definition:** A joke, reaction or chatter that isn't trying to convince
anyone of anything.

**Example 1:**
> GM during his early Cavs years: Bron, it's hard for us to get you great teammates. Bron: Fine, I'll do it myself.

**Example 2:**
> Shoot for the moon, because even if you miss, you might end up in LeBron's grandpa arms

### The hardest boundary

**Which two labels:** `evidence` and `take`: posts that are half fact, half
opinion. For example:

> Kat was a different player the last two years than he was in mn. He out on weight and stopped playing like he was 6'6"

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

If a post is mostly opinion and the fact is only a side detail, it's `take`.
It's `evidence` only when the checkable fact is what carries the argument —
take the fact out and the claim would be left with no support. The KAT post
above is `take`: "put on weight" and "stopped playing like he was 6'6"" are
impressions, not something you could look up.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Public comments from r/nba, collected in
October 2026 from the subreddit's public RSS feeds. They come from 32 threads
from the subreddit's top and hot listings: trade and injury news, the 2026
playoffs and Finals, game highlights, and post-game threads. I left out 12
threads that were mostly about politics or off-court topics, took up to 8
comments of at least 80 characters from each of the rest, and dropped
anything that wasn't about basketball (tangents about Halloween decorations,
F1, cartoons and so on), plus one comment that was only an image link. That
left 202 posts, kept whole.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

I labelled 20 posts cold first, by hand, from my definitions only (`cold` in
the note column). Claude pre-labelled the other 182 using my definitions and
decision rule. I then read every one of those 182 and corrected the ones I
disagreed with (`pre-labelled by Claude, checked by me` in the note column).

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `take` | 95 | 47.0% |
| `banter` | 79 | 39.1% |
| `evidence` | 28 | 13.9% |
| **Total** | 202 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* I’m obviously biased but I’d argue that Jimmy dragging two separate underdog teams to the finals is much more impressive than Wiggins or KAT winning titles as 2nd options. BUT I’m a huge UK guy so KAT will always be my boy. I can recognize how much of a chode Jimmy was being to those guys. I’m super happy that KAT and Wiggins got rings and were meaningful contributors when they got them.
>
> *Could have been:* `evidence` or `take`
>
> *I chose, because:* `take`. The poster says up front they're biased, and
> bias means opinion. The Finals runs are real, but the post is about which is
> "more impressive", which is a judgment.

**2.**
> *The post:* Studies have been done showing Achilles tears are most common when increasing strain or working out harder after a period of rest or less intense activity. Playoff minutes increases after an easy taper to the season. Or after a lengthy off season of low intensity work.
>
> *Could have been:* `evidence` or `take`
>
> *I chose, because:* `evidence`. It points to studies, which is a source you
> could go and check, and the studies are what the whole argument rests on.

**3.**
> *The post:* The clippers got hammered hard. Franchise altering. And for what? One conference final appearance
>
> *Could have been:* `evidence` or `take`
>
> *I chose, because:* `evidence`. "Franchise altering" sounds like opinion,
> but the point rests on one checkable fact: the Clippers made one conference
> final in that era. Take the fact out and the post has nothing left, so under
> my decision rule it's `evidence`.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, 128 tokens per post, seed 42.

**Anything I changed from the defaults, and why:** Nothing. Every setting is
the notebook default.

**Trained on:** `mps` (Apple Silicon GPU), torch 2.14.1, as recorded in
`results.json`. Training took about 24 seconds.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

train 140 · val 31 · test 31

| Label | Train | Val | Test |
|---|---|---|---|
| `evidence` | 20 | 4 | 4 |
| `take` | 65 | 15 | 15 |
| `banter` | 55 | 12 | 12 |

`evidence` has only **4** posts in the test split, below the ~8 the notebook
warns about, so its F1 will move a lot between seeds.


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked Claude (Claude Code) to pick a community and
  suggest labels.
- *What came back:* It suggested r/nba, collected the comments with a script
  from Reddit's public RSS feeds, and proposed two taxonomies: three labels
  (`evidence` / `take` / `banter`) or four (`analysis` / `hot_take` /
  `reaction` / `joke`).
- *What I changed:* I chose the three-label version. For the hardest
  boundary, Claude laid out the options and I picked the decision rule: a
  post that's half fact, half opinion is `take` unless the fact carries the
  argument.

**Moment 2**

- *What I asked for:* Help turning my criteria numbers into criteria.md.
- *What came back:* Claude pointed out that my first cap, "no label above
  40%", was already failed by my own data (`take` is 47%), and that a 0.90
  target for `evidence` is very hard with only ~4 test posts. It wrote my
  reasons up as full sentences.
- *What I changed:* I raised the cap to 50%. I raised the `evidence` target
  from 0.57 to 0.90 and kept it there.

**Pre-labelling disclosure:** I labelled 20 posts cold, by hand, before
seeing any AI labels. Claude pre-labelled the other 182 using my definitions
and decision rule, and flagged 7 posts it found borderline. I read all 182
and changed the ones I disagreed with: post 92 ("Studies have been done…")
went from `take` to `evidence`, and post 113 ("One conference final
appearance") went from `evidence` to `take` and back to `evidence`. Claude
also wrote the first draft of most of this README, from my choices and my
reasons.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

Trained on `mps` (Apple Silicon GPU), torch 2.14.1, notebook defaults (3
epochs, lr 2e-5, batch 16). Recorded in `results_three_seeds_before.json`.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 | 0.484 | 0.484 | 0.548 | MISSED |
| 2. F1 on `take` and `banter` | each ≥ 0.70 | take 0.652 · banter 0.000 | take 0.652 · banter 0.000 | take 0.682 · banter 0.286 | MISSED |
| 3. F1 on `evidence` | ≥ 0.90 | 0.000 | 0.000 | 0.000 | MISSED |
| 4. Largest label share of labels.csv | ≤ 50% | 47.0% | 47.0% | 47.0% | MET |
| 5. Agreement with staff on 30 posts | ≥ 70% | — | — | — | *pending agreement check* |

Criterion 4 is a property of `labels.csv`, not of a split, so it's the same
for every seed. Criterion 5 is measured once, on the staff set, not per seed.

The full spread, from section 6:

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.484 | 0.484 | 0.548 | 0.065 |
| Macro F1 | 0.217 | 0.217 | 0.323 | 0.105 |
| F1 — `evidence` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `take` | 0.652 | 0.652 | 0.682 | 0.030 |
| F1 — `banter` | 0.000 | 0.000 | 0.286 | 0.286 |

Accuracy moved 6.5 points across seeds, under the 10-point line, but that
stability is misleading: on seeds 42 and 7 the model predicted `take` for all
31 posts, so both score exactly the share of `take` in the test split (15/31).

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

Seed 42 (section 7 prints the first seed):

| true \ predicted | evidence | take | banter |
|---|---|---|---|
| **evidence** | 0 | 4 | 0 |
| **take** | 0 | 15 | 0 |
| **banter** | 0 | 12 | 0 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
