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

Overall accuracy on the held-out test set is at least 0.65.

**Why this target:** Three labels means a coin-flip guesser gets about 33%, so 0.65 is meaningfully better than chance while still realistic for a model fine-tuned on roughly 200 hand-labeled examples, not thousands.



---

## 2. Per-label performance

Every label has an F1 of at least 0.55 on the held-out test set.

**Why this target:** I expect `reaction` to be my smallest, noisiest label, since short reaction comments showed up less often in my reading than detailed case write-ups. I set this looser than the overall accuracy target to account for that label's jumpier score with fewer test examples.



---

## 3. Balance

No label makes up more than 60% of my 200 labeled examples.

**Why this target:** The assignment's hard cap is 70%, but `evidenced` is likely to dominate naturally since r/TrueCrime comments skew toward detailed case recaps. Capping it at 60% forces me to genuinely collect enough `speculation` and `reaction` examples instead of letting one label coast to the edge of the limit.



---

## 4. Consistency

I agree with the staff labels on at least 70% (14 of 20) of the cold-labeled posts.

**Why this target:** The hardest boundary in my taxonomy is `evidenced` vs. `reaction`, since a comment can carry both a fact and a feeling at once. I expect real disagreement with staff on exactly that boundary, so 70% is clearly better than chance (33%) without pretending my boundary is cleaner than it actually is.



---

## 5. Confidence

Predictions where the model's confidence is above 0.8 are correct at least 80% of the time.

**Why this target:** If the model's confident predictions aren't noticeably more accurate than its overall average (0.65), the confidence scores would be meaningless noise rather than something a user could actually trust.



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
