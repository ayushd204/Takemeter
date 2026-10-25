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

This project is about football posts and how they communicate. TakeMeter is
intended to sort them into `information`, `stats and analytics`, `discussion`,
and `opinion`. The labels distinguish news, evidence or figures, questions,
and subjective claims.

---

## Label Taxonomy

<!-- Definitions are copied from label_definitions.txt. Examples are from
     labels.csv. -->

### `information`

**Definition:** A neutral argument conveying certain news.

**Example 1:**

> “Anthony Taylor has been appointed as the chairman of the Central Referee Board of the Turkish Football Federation.”

**Example 2:**

> “Manchester City have been found guilty on all but one of the 115 charges, according to sources briefed on the matter.”

### `stats and analytics`

**Definition:** Posts backed by facts, numbers and figures.

**Example 1:**

> “Harry Kane has now scored 102 goals in his first 100 Bundesliga appearances.”

**Example 2:**

> “Barcelona have won their first 9 LaLiga games for only the second time in competition history.”

### `discussion`

**Definition:** Questions on a specific topic, looking for definite answers.

**Example 1:**

> “When did Ronaldo's form notably decline? Was it the Euros 2024?”

**Example 2:**

> “Would you rather your club loan a young player out or keep them training with the first team?”

### `opinion`

**Definition:** A subjective claim with no direct evidence.

**Example 1:**

> “Upamecano is nowhere near Pacho and neither are Gabriel and Saliba.”

**Example 2:**

> “Lamine Yamal is THE superstar of futbol.”

### The hardest boundary

**Which two labels:** `information` and `stats and analytics`

**The decision rule I used every time:** The boundary is not fully resolved in
the current definitions: news can include numbers, and stats posts can report
news. As a provisional rule, I use `stats and analytics` when the post's main
point is its figures or comparison; I use `information` when it primarily
announces an event or development.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** The subreddits `r/football`,
`r/SpicyFootballTakes`, and `r/Referees` and so on.

**How I labelled them:** The file contains 33 posts marked `cold` and 177
marked `pre-labelled, checked`.

**Counts per label:**

| Label               | Count   | Share    |
| ------------------- | ------- | -------- |
| information         | 52      | 24.8%    |
| stats and analytics | 49      | 23.3%    |
| discussion          | 58      | 27.6%    |
| opinion             | 51      | 24.3%    |
| **Total**           | **210** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**

> _The post:_ “Barcelona have won their first 9 LaLiga games for only the second time in competition history. Barcelona’s start to the 2026/27 LaLiga season: 9 games, 9 wins, 39 goals scored, 9 conceded.”
>
> _Could have been:_ `information` or `stats and analytics`
>
> _I chose, because:_ `stats and analytics`; the record and figures are the main point.

**2.**

> _The post:_ “Would you rather your club loan a young player out or keep them training with the first team?”
>
> _Could have been:_ `discussion` or `opinion`
>
> _I chose, because:_ It asks readers to answer a specific question, so I labelled it `discussion`.

**3.**

> _The post:_ “Lamine Yamal is THE superstar of futbol, then they have half the world cup winning squad.”
>
> _Could have been:_ `opinion` or `information`
>
> _I chose, because:_ It is a subjective evaluation without evidence, so I labelled it `opinion`.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate `2e-5`, batch size 16, seed 42.

**Anything I changed from the defaults, and why:** I used the notebook's
configured settings.

**Split sizes:** The notebook uses stratified 70% / 15% / 15% train,
validation, and test splits. The saved test split has 32 posts: 8
`information`, 7 `stats and analytics`, 9 `discussion`, and 8 `opinion`.
The `stats and analytics` test group has fewer than 8 examples, so its score
may vary more from run to run.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- _What I asked for:_ Fill the note field in `labels.csv` with `pre-labelled, checked` for the posts other than manually labeled.
- _What came back:_ The CSV notes were updated to follow that instruction;
  the narratives and classifier labels were kept as provided.
- _What I changed:_ I reviewed the note counts and kept the requested split:
  33 `cold` and 177 `pre-labelled, checked`.

**Moment 2**

- _What I asked for:_ format the README using the files already in
  the project, while preserving its existing structure and most of its text.
- _What came back:_ A well structured readme.
- _What I changed:_ Manully formatted the tables for better structure.

**Pre-labelling disclosure:** The notes record 33 posts as `cold` and 177 as
`pre-labelled, checked`. AI helped apply these note values at my direction;
it did not create the narratives or choose their classifier labels.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure                    | Baseline | Trained | Difference |
| -------------------------- | -------- | ------- | ---------- |
| Overall accuracy           | 0.719    | 0.750   | +0.031     |
| Macro F1                   | 0.628    | 0.698   | +0.070     |
| F1 — `discussion`          | 1.000    | 0.696   | -0.304     |
| F1 — `information`         | 0.636    | 0.941   | +0.305     |
| F1 — `opinion`             | 0.000    | 0.222   | +0.222     |
| F1 — `stats and analytics` | 0.875    | 0.933   | +0.058     |

**What I predicted before I looked:**

<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I did predict that the overall accuracy of the model has to be increased with fine tuning of the model.

**What the gap actually means:**

<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
The gap means that there is some improvement in the model.
---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
| --------- | ------ | ------- | ------ | --------- | ------- |
| 1. Overall accuracy | ≥ 0.60 each run | 0.750 | 0.719 | 0.719 | MET |
| 2. Every label F1 | ≥ 0.45 each label and run | Lowest: 0.222 (`opinion`) | Lowest: 0.222 (`opinion`) | Lowest: 0.000 (`opinion`) | MISSED |
| 3. Label balance | Each label 15%–40% before training | 23.3%–27.6% | 23.3%–27.6% | 23.3%–27.6% | MET |
| 4. Accuracy consistency | Accuracy range ≤ 0.10 | 0.750 | 0.719 | 0.719 | MET |
| 5. Confidence separation | Most confident third ≥ 0.10 above least confident third | 1.000 vs 0.200 (gap 0.800) | 1.000 vs 0.300 (gap 0.700) | 1.000 vs 0.300 (gap 0.700) | MET |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted        | information | stats and analytics | discussion | opinion |
| ----------------------- | ----------- | ------------------- | ---------- | ------- |
| **information**         | 8           | 0                   | 0          | 0       |
| **stats and analytics** | 0           | 7                   | 0          | 0       |
| **discussion**          | 0           | 1                   | 8          | 0       |
| **opinion**             | 1           | 0                   | 6          | 1       |

**My biggest off-diagonal number, and what it means:**

<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

6 real `opinion` posts were called `discussion` and 0 real `discussion` posts were called `opinion` which means the model is misclassifying the real `opinions` classifier.

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| #   | Criterion | Target | Verdict | How I decided |
| --- | --------- | ------ | ------- | ------------- |
| 1 | Overall accuracy | ≥ 0.60 on each run | MET | 0.750, 0.719, and 0.719 all meet the 0.60 floor. |
| 2 | Every label F1 | ≥ 0.45 for every label on each run | MISSED | `opinion` F1 was 0.222, 0.222, and 0.000, below 0.45 in all three runs. |
| 3 | Label balance | Every label is 15%–40% of the training set | MET | The four label shares are 24.8%, 23.3%, 27.6%, and 24.3%; all are within range. |
| 4 | Accuracy consistency | Highest–lowest accuracy range ≤ 0.10 | MET | The range is 0.750 − 0.719 = 0.031, within the 0.10 limit. |
| 5 | Confidence separation | Most confident third is ≥ 0.10 more accurate than least confident third in every run | MET | The accuracy gaps are 0.800, 0.700, and 0.700, each above 0.10. |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

- **Per-label F1 — MISSED:** `opinion` has 51 of 210 training examples (24.3%), so it is not the smallest or underrepresented label in the training set. Its F1 is 0.222, 0.222, and 0.000 across the seeds. The confusion matrices show the same pattern: true `opinion` posts were predicted as `discussion` 6, 1, and 5 times and as `stats and analytics` 0, 6, and 3 times (seeds 42, 7, and 2024). That repeated confusion points to a difficult `opinion` boundary, and suggests the model has not learned it reliably from these 210 examples. The agreement report files are not present, so I cannot tell from this evidence whether inconsistent training labels also contributed.

---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** **_ / 30 = _**%

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

> _The post:_
>
> _Staff said / I said:_
>
> _My call, and why:_
>
> _Which pile:_

**2.**

> _The post:_
>
> _Staff said / I said:_
>
> _My call, and why:_
>
> _Which pile:_

**What the pattern in my disagreements tells me:**

---

## The Improvement

**What I changed:**

I added 50 `opinions` posts to `labels.csv`. This raised the opinion examples from 51 to 101 of 260 total (38.8%), still within the 15%–40% balance target but close to its upper limit.

**Which diagnosis pointed at it:**

The before-run diagnosis showed that `opinion` had only 51 training examples and was repeatedly confused with `discussion` or `stats and analytics`, so I added more opinion examples to address the limited examples for that label.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
| --------- | ------ | ------- | ------ | --------- | ------- |
| 1. Overall accuracy | ≥ 0.60 each run | 0.769 | 0.667 | 0.795 | MET |
| 2. Every label F1 | ≥ 0.45 for every label on each run | Lowest: 0.200 (`discussion`) | Lowest: 0.200 (`discussion`) | Lowest: 0.714 (`discussion`) | MISSED |
| 3. Label balance | Every label 15%–40% of training set | 18.8%–38.8% | 18.8%–38.8% | 18.8%–38.8% | MET |
| 4. Accuracy consistency | Accuracy range ≤ 0.10 | 0.769 | 0.667 | 0.795 | MISSED |
| 5. Confidence separation | Most confident third ≥ 0.10 above least confident third | 0.923 vs 0.538 (gap 0.385) | 0.846 vs 0.538 (gap 0.308) | 0.846 vs 0.769 (gap 0.077) | MISSED |

**Did it help, and how do I know:**

It helped the original `opinion` problem. The improvement was partial, because `discussion` F1 fell to 0.200 in two seeds, the accuracy spread grew to 0.128 (above the 0.10 limit).

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->

---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

**The gap between what I meant my labels to capture and what the model
learned:**

<!-- Two sentences. Your confusion matrix is the evidence. -->

After adding opinion examples, the model recognized all 15 held-out `opinion` posts in each seed, but called 8, 8, and 4 true `discussion` posts `opinion`; this boundary error aligns with the `discussion` F1 misses and the 0.128 accuracy spread, while confidence separation also missed in seed 2024. Next I would review and consistently relabel ambiguous `discussion`/`opinion` examples, then add balanced examples if needed

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
