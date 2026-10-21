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
| `reaction` | 76 | 38.0% |
| `speculation` | 45 | 22.5% |
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

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

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
