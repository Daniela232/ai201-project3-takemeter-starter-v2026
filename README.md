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

This project uses public comments collected from r/TrueCrime. It trains a text classifier to sort each comment into one of three categories: `evidenced`, `speculation`, or `reaction`. The goal is to distinguish fact-supported discussion from unsupported theories and purely emotional responses. The model was trained locally on 200 labeled comments.



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `evidenced`

**Definition:** Makes a claim about a case backed by a specific, checkable fact -- a date, a name, a legal outcome, or a cited source like a podcast or book.

**Example 1:**
> I had only heard about this case from licensed private investigator/advocate Heather Zielinski (formerly Cohen)... Swift a mother of 4 was reported missing on October 30, 2011 & her body was found less than a mile from her home six weeks later... Her husband David Swift was charged with First degree premeditated murder, his trial was in June 2024 in which he was found not guilty of first & second degree murder, the jury was deadlocked for voluntary manslaughter.

**Example 2:**
> Two little know serial killers that were never caught: the Frankford Slasher and the Toledo Clubber. In the Frankford area of Philly possibly nine women were stabbed between 1985 and 1990. There was a really good suspect, but he moved away at the end of the period and another man was arrested and then later acquitted for one of the murders.

### `speculation`

**Definition:** A confident claim or theory about a case with no supporting fact or source offered.

**Example 1:**
> Jelani Day. I fully believe there was foul play involved.

**Example 2:**
> You guys should look up the Gary Wayne Sutton case in Tennessee. He's on death row, but there's a bunch of people who think he's innocent.

### `reaction`

**Definition:** A purely emotional response to a case, with no claim or argument being made at all.

**Example 1:**
> Israel Keyes is living rent free in my head. Normally I'm not into serial killers, but that one gives me the heebie jeebies in a really intense way.

**Example 2:**
> Heard many cases over the years different Podcasts but the case of Lauren Agee still makes my blood boil.

### The hardest boundary

**Which two labels:** `evidenced` and `reaction`

**The decision rule I used every time:** If the post contains a specific checkable fact about the case (a date, a name, a location, a legal detail), it's `evidenced`, even if it also contains emotional language or a feeling by the end. The factual content takes priority over tone. For example, one comment about Geoffrey Spangler's disappearance gives exact dates, named locations, and a named podcast source, then ends with "I felt so sad for her. I can't imagine the not knowing" -- that comment is `evidenced`, not `reaction`, because the fact content came first and the feeling was incidental to it.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** I collected 200 public comments from r/TrueCrime. The comments discuss different cases, legal outcomes, investigations, theories, and emotional reactions.

**How I labelled them:** I labeled the first 20 comments cold, without AI assistance, and marked them `cold` in the note column. An AI tool pre-labeled the remaining 180 comments using my definitions. I personally reviewed every pre-labeled comment before marking it `reviewed`.

**Counts per label:**

| Label | Count | Share |
|---|---:|---:|
| `evidenced` | 79 | 39.5% |
| `reaction` | 71 | 35.5% |
| `speculation` | 50 | 25.0% |
| **Total** | **200** | **100%** |

**Three hard cases**

**1.**
> *The post:* “Great write up! I believe he was married, but he had a pregnant girlfriend that he strung along for years. Just an all around creep.”
>
> *Could have been:* `evidenced` or `reaction`
>
> *I chose, because:* I chose `evidenced` because the comment includes specific, checkable claims about the person’s marriage and pregnant girlfriend. The emotional judgment at the end does not erase the factual claims.

**2.**
> *The post:* “Has to be a cover up for someone in law enforcement. How does someone stab the back of their own head? Unbelievable.”
>
> *Could have been:* `speculation` or `reaction`
>
> *I chose, because:* I chose `speculation` because the writer proposes an unsupported cover-up theory. “Unbelievable” is emotional, but the main purpose of the comment is to make a theory about the case.

**3.**
> *The post:* “I went to school with Deanna. We were on the same volleyball team for a few years. When this happened, it rocked our HS pretty hard.”
>
> *Could have been:* `evidenced` or `reaction`
>
> *I chose, because:* I chose `evidenced` because the writer provides specific firsthand details about attending school and playing volleyball with Deanna. The emotional effect on the school is secondary to those checkable details.
---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:**

- Epochs: 3
- Learning rate: 2e-5
- Batch size: 16
- Maximum length: 128 tokens
- Seed: 42
- Device: CPU
- PyTorch version: 2.2.2
- Training time: approximately 4 minutes and 9 seconds

**Anything I changed from the defaults, and why:** I did not change any training hyperparameters. I used the notebook’s default settings so the first result would be a consistent starting point.

**Split sizes:**

- Training set: 139 comments
- Validation set: 31 comments
- Test set: 30 comments

| Label | Train | Validation | Test |
|---|---:|---:|---:|
| `evidenced` | 55 | 12 | 12 |
| `speculation` | 31 | 7 | 7 |
| `reaction` | 53 | 12 | 11 |

The `speculation` label has only 7 examples in the test split. Its F1 score may therefore change substantially between seeds because each individual prediction has a large effect on the metric.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I gave the AI my three label definitions and asked it to pre-label the remaining 180 collected r/TrueCrime comments.
- *What came back:* It returned one of the three labels for each comment based on whether the comment contained checkable evidence, an unsupported theory, or only an emotional reaction.
- *What I changed:* I read every pre-labeled comment and checked it against my definitions and decision rule. After confirming the labels, I marked those rows as `reviewed` in the note column.

**Moment 2**

- *What I asked for:* I asked the AI to help troubleshoot errors that appeared when I ran the training notebook on my Intel Mac.
- *What came back:* It identified compatibility conflicts between NumPy, PyTorch, pandas, and pyarrow.
- *What I changed:* I installed compatible versions of NumPy 1.26.4, pandas 2.2.3, and pyarrow 17.0.0 inside the project’s virtual environment. I did not change the notebook’s training code or hyperparameters.

**Pre-labelling disclosure:** An AI tool pre-labeled 180 of the 200 comments. I personally reviewed every AI-labeled row against my written taxonomy before marking it `reviewed`. The first 20 comments were labeled cold without AI assistance.

**Moment 3**

- *What I asked for:* I asked the AI to help interpret the three-seed results, confusion matrix, and agreement report and identify one controlled improvement.
- *What came back:* It suggested reviewing the boundary between `reaction` and `speculation` and identified five comments that made unsupported claims rather than purely emotional responses.
- *What I changed:* I personally reviewed those five comments against my taxonomy, changed their labels from `reaction` to `speculation`, and reran the three-seed experiment. I reported that the change did not improve performance consistently.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---:|---:|---:|
| Overall accuracy | 0.400 | 0.567 | +0.167 |
| Macro F1 | 0.255 | 0.420 | +0.165 |
| F1 — `evidenced` | 0.564 | 0.688 | +0.123 |
| F1 — `reaction` | 0.000 | 0.571 | +0.571 |
| F1 — `speculation` | 0.200 | 0.000 | -0.200 |

**What I predicted before I looked:**

I predicted that my trained model would have higher overall accuracy than the baseline because it learned from comments labeled under my specific taxonomy. I expected it to perform best on `evidenced` and `reaction`, while struggling with `speculation` because that label had the fewest examples.

**What the gap actually means:**

The trained model improved overall accuracy by 0.167 and macro F1 by 0.165, so fine-tuning added useful learning. The largest improvement was on `reaction`, but the trained model failed to identify `speculation`, showing that it did not learn that label’s boundary well.


---

## Run Log — Before

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---:|---:|---:|---|
| 1. Overall accuracy | At least 0.65 | 0.567 | 0.500 | 0.633 | MISSED |
| 2. Every label F1 | At least 0.55 | Minimum: 0.000 | Minimum: 0.000 | Minimum: 0.000 | MISSED |
| 3. Largest label share | No more than 60% | 39.5% | 39.5% | 39.5% | MET |
| 4. Staff agreement | At least 70% | 76.7% | 76.7% | 76.7% | MET |
| 5. Most-confident third accuracy | At least 80% | 90% | 90% | 90% | MET |

Criteria 3 and 4 are dataset-level measurements, so their values do not change between training seeds. The accuracy ranged from 0.500 to 0.633, a spread of 0.133. Because the spread is greater than 0.10, the model’s performance is not stable across different data splits.

### Confusion matrix

| true \ predicted | evidenced | speculation | reaction |
|---|---:|---:|---:|
| **evidenced** | 11 | 0 | 1 |
| **speculation** | 4 | 0 | 3 |
| **reaction** | 5 | 0 | 6 |

**My biggest off-diagonal number, and what it means:**

Five real `reaction` posts were predicted as `evidenced`, while only one real `evidenced` post was predicted as `reaction`. This lopsided error shows that the model leans toward `evidenced` when it is uncertain about the boundary between those two labels. The model also never predicted `speculation`, which shows that it did not learn that label’s boundary.

---

** Verdicts and Diagnoses**

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | At least 0.65 on every seed | MISSED | The three accuracies were 0.567, 0.500, and 0.633. None reached 0.65. |
| 2 | Every label F1 | At least 0.55 on every seed | MISSED | `speculation` had an F1 of 0.000 on all three seeds. `reaction` also fell to 0.526 on seed 7. |
| 3 | Largest label share | No label above 60% | MET | The largest label was `evidenced` with 79 of 200 examples, or 39.5%. |
| 4 | Staff agreement | At least 70% of the 30 official posts | MET | I agreed with staff on 23 of 30 posts, or 76.7%. |
| 5 | Most-confident third accuracy | At least 80% on every seed | MET | The most-confident third was 90% accurate on all three seeds. |

**Diagnoses**

**Criterion 1, overall accuracy:** The model missed the 0.65 accuracy target on every seed. Accuracy changed from 0.500 to 0.633, a spread of 0.133, showing that results are unstable across data splits. The dataset has only 200 examples, and `speculation` has only 45 total examples and 7 examples in each test split. The small dataset and weak performance on the smallest label pulled down overall accuracy.

**Criterion 2, every label F1:** The clearest problem is `speculation`, which had an F1 of 0.000 on all three seeds. In the seed 42 confusion matrix, all seven real `speculation` posts were misclassified: four as `evidenced` and three as `reaction`. The model never predicted `speculation` at all. My agreement report also showed that I inconsistently decided when a fact truly supported an argument and when a post was only an unsupported claim. This suggests that the problem is both too few `speculation` examples and an inconsistently applied boundary, not only random model error.

---



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 23 / 30 = 76.7%

**Disagreements**

**1.**

> *The post:* Everyone blames the hill at 18 miles. I split the field into people who ran the first half under 1:45 and over, and the slowdown after 18 was almost identical for both groups. If the hill were the problem the fast starters would fall apart harder.
>
> *Staff said / I said:* Staff said `analysis`; I said `hot_take`.
>
> *My call, and why:* Staff has the stronger label. The writer compares two groups and uses their nearly identical slowdown to argue that the hill was not the main cause. The comparison directly supports the conclusion, so the facts are load-bearing.
>
> *Which pile:* (a) The rule covered it, but I applied it too loosely.

**2.**

> *The post:* Ran the same loop in 4C and in 24C within a fortnight, same effort by heart rate. 90 seconds slower in the heat over six miles. People keep saying heat costs you “a bit” and it’s more than that.
>
> *Staff said / I said:* Staff said `analysis`; I said `hot_take`.
>
> *My call, and why:* Staff has the stronger label. The writer compares the same route and effort under two temperatures and uses the 90-second difference to support the claim that heat has a meaningful effect.
>
> *Which pile:* (a) The rule covered it, but I applied it too loosely.

**3.**

> *The post:* Went back through the club results going back to 2012. Every single year the number of people who enter the spring marathon and don’t start it sits between 18 and 23 percent. It’s not a bad-luck year, it’s the base rate.
>
> *Staff said / I said:* Staff said `analysis`; I said `hot_take`.
>
> *My call, and why:* Staff has the stronger label. The results from 2012 onward and the repeated range of 18 to 23 percent directly support the conclusion that the current year is not an unusual one-time event.
>
> *Which pile:* (a) The rule covered it, but I applied it too loosely.

**4.**

> *The post:* Heart rate zones are astrology for runners.
>
> *Staff said / I said:* Staff said `hot_take`; I said `reaction`.
>
> *My call, and why:* Staff has the stronger label. This is a general unsupported claim about heart rate zones. It does not describe something that just happened to the writer or the writer’s immediate feelings.
>
> *Which pile:* (a) The rule covered it, but I applied it too loosely.

**5.**

> *The post:* Times are down about 3% since the shoe rules changed, so obviously the shoes are the whole story and the sport has a problem.
>
> *Staff said / I said:* Staff said `hot_take`; I said `analysis`.
>
> *My call, and why:* Staff’s label is stronger because the writer jumps from a 3 percent change to saying that shoes are the entire cause. The word “obviously” replaces the missing reasoning. However, my original label is defensible because the percentage and timing of the rule change could be read as evidence supporting the claim, even though the conclusion is too strong.
>
> *Which pile:* (c) The example is genuinely ambiguous, and my reading is defensible, although the staff interpretation follows Rule 1 more closely.

**6.**

> *The post:* Road racing peaked in about 2015 and everything since has been marketing.
>
> *Staff said / I said:* Staff said `hot_take`; I said `analysis`.
>
> *My call, and why:* Staff has the stronger label. The year 2015 is part of the unsupported assertion, but the writer provides no results, comparisons, or other evidence showing that racing peaked then.
>
> *Which pile:* (a) The rule covered it, but I applied it too loosely.

**7.**

> *The post:* First run back after eight weeks out. Two miles, slower than my warm-up used to be, and I don’t care even slightly.
>
> *Staff said / I said:* Staff said `reaction`; I said `analysis`.
>
> *My call, and why:* Staff has the stronger label. The post reports something that just happened to the writer and describes the writer’s personal response. The numbers provide context, but they are not used to make a broader argument.
>
> *Which pile:* (a) The rule covered it, but I applied it too loosely.

**What the pattern in my disagreements tells me:**

Most of my disagreements involved the boundary between `analysis` and `hot_take`. I sometimes overlooked facts that directly supported an argument, while other times I treated a number or named year as evidence even when it did not support the conclusion. I also confused a general unsupported claim with a personal reaction once. This shows that I applied the written boundaries inconsistently, especially when deciding whether a fact was actually load-bearing.

---

## The Improvement

**What I changed:**

I reviewed the boundary between reaction and speculation and relabeled five comments from reaction to speculation. Each comment made an unsupported general, legal, or policy claim rather than expressing only an emotion. This changed the dataset counts from 79 evidenced, 76 reaction, and 45 speculation examples to 79 evidenced, 71 reaction, and 50 speculation examples. I kept the model and training settings unchanged so that the label correction was the only experimental change.

**Which diagnosis pointed at it:**

Before the improvement, speculation had an F1 of 0.000 under all three seeds, and the model never predicted that label. My labeling review also showed that I sometimes applied boundaries inconsistently when separating unsupported claims from other posts. These findings suggested that some unsupported claims had been incorrectly included under reaction, weakening the model’s understanding of speculation.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---:|---:|---:|---|
| 1. Overall accuracy | At least 0.65 | 0.500 | 0.467 | 0.667 | MISSED |
| 2. Every label F1 | At least 0.55 | Minimum: 0.000 | Minimum: 0.000 | Minimum: 0.000 | MISSED |
| 3. Largest label share | No more than 60% | 39.5% | 39.5% | 39.5% | MET |
| 4. Staff agreement | At least 70% | 76.7% | 76.7% | 76.7% | MET |
| 5. Most-confident third accuracy | At least 80% | 90% | 90% | 90% | MET |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->

The improvement did not help consistently. Mean accuracy decreased from approximately 0.567 before the change to 0.544 afterward, while mean macro F1 decreased from approximately 0.424 to 0.391. The speculation F1 remained 0.000 under all three seeds because the model still never predicted that label. Seed 2024 improved from 0.633 to 0.667 accuracy, and its reaction F1 increased to 0.800, but the other two seeds became worse. The accuracy spread also increased from 0.133 to 0.200, indicating greater instability. Relabeling only five examples was therefore not enough to teach the model the speculation boundary.


---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

Criteria 1 and 2 are still missed. Overall accuracy passed 0.65 only under seed 2024, while the other two seeds remained below the target. To improve this, I would collect more carefully reviewed examples, especially posts near the boundaries among all three labels, and then test each additional change separately.

The most serious remaining problem is that speculation still has an F1 of 0.000 under every seed. I would collect more varied speculation examples, review the entire dataset for unsupported claims incorrectly placed in another label, and clarify the taxonomy with more boundary examples. After improving the data, I would test class weighting or balanced sampling as a separate experiment.

I stopped after this attempt because the assignment calls for one controlled improvement. Making additional data and training changes simultaneously would make it difficult to determine which change caused the result, and each three-seed CPU run also requires substantial time.


**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->

I intended speculation to represent confident unsupported claims, but the model learned mainly to choose between factual-looking evidenced posts and emotional reaction posts. In the after-change seed 42 confusion matrix, all seven real speculation posts were still misclassified—six as evidenced and one as reaction—so the model never learned the unsupported-claim boundary.

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
