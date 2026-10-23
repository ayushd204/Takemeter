# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy

Across each of the three held-out runs, overall accuracy is at least **0.60**.

**Why this target:** The project distinguishes four labels, so a uniform random guess would be right about 0.25 of the time. 0.60 is a meaningful improvement while leaving room for overlap between discussion and opinion, and information and stats/analytics.

---

## 2. Per-label performance

Across each of the three held-out runs, each label has an F1 score of at least **0.45**.

**Why this target:** The current `labels.csv` has only 19 posts, with some labels represented just three times. That makes per-label scores noisy, so 0.45 is a cautious floor; the dataset needs to grow before a strict F1 target is dependable.

---

## 3. Label balance

Before training, each label must make up at least **15%** and no more than **40%** of the labelled posts.

**Why this target:** With four labels, perfectly even data would be 25% each. A 15% floor prevents the smallest category from being too rare to learn, while the 40% cap stops one category from dominating.

---

## 4. Model consistency across runs

Across the three seeded runs, the range between the highest and lowest held-out accuracy must be no more than **0.10** (10 percentage points).

**Why this target:** The notebook trains and evaluates the model three times with different seeds and reports the accuracy spread. A maximum ten-point swing is a practical stability check: it means the result is not changing drastically with the run. With the current small dataset, larger swings may point to too few examples or unstable splits, rather than a dependable model result.

---

## 5. Confidence

Across each held-out run, accuracy on the most confident third of predictions must be at least **0.10 higher** than accuracy on the least confident third.

**Why this target:** A confidence score is useful only if it separates safer predictions from uncertain ones. A ten-point gap is large enough to matter for deciding which posts need human review, without requiring the model to be perfectly calibrated.



---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
