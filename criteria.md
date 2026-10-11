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

## 1.

<!-- Your criterion. It must name a number. -->

Test accuracy is at least **0.60** on all three seeds.

**Why this target:**

Always guessing `critique` already scores 0.48, since critique is 48% of my 202 posts, so the real floor is 0.48, not 0.33. 0.60 is about four more correct posts out of a ~31-post test split. That's a clear step above the majority guess, and it's low enough to hold on the worst of three seeds rather than just the best.

---

## 2.

<!-- Your criterion. -->

F1 is at least **0.65** for `critique` and at least **0.45** for each of `hot_take` and `reaction`, on all three seeds.

**Why this target:**

`critique` has about 15 posts in the test split, so its F1 is stable enough to hold to a higher bar. `hot_take` and `reaction` have about 8 each, so one wrong post moves recall by about 12 points. Their targets are looser because their scores will swing between seeds.

---

## 3.

<!-- Your criterion. -->

The model predicts `critique` for **no more than 65%** of test posts, on all three seeds.

**Why this target:**

`critique` is 48% of the data. If predictions drift far above that share, the model is falling back on the majority label instead of learning the other two, even if accuracy still looks acceptable.

---

## 4.

<!-- Your criterion. -->

My labels agree with the staff labels on at least **70%** of the 30 agreement posts (21 of 30).

**Why this target:**

Three labels plus a `hot_take`/`reaction` boundary that needed a written rule means some disagreement is expected. Below 70% would mean my definitions don't transfer to someone else's labeling, and the model is learning my habits rather than the taxonomy.

---

## 5.

<!-- Your criterion. -->

Accuracy on the model's most-confident third of test posts is at least **0.20** higher than on its least-confident third, on all three seeds.

**Why this target:**

Each third is about 10 posts, so 0.20 means at least two more right answers. A gap smaller than that could come from one lucky post. A real gap means the model's confidence is worth looking at when deciding which predictions to trust.

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
