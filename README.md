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



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### Candidate distinctions (Milestone 1)

Things I could point at while reading r/books comments, before settling on labels:

1. **Gives a reason or evidence, or just asserts.** Some comments back a claim
   with an anecdote, a price, a named character, or a list of reasons ("not the
   best reading experience, not the best option for convenience, and not
   competitive enough on price"). Others state the claim and stop ("Missouri is
   governed by ghouls").
2. **About me, or about the world.** Some comments are mostly the writer's own
   feelings or experience ("I miss paperbacks", "it makes me sad tbh"). Others
   make a claim about books, readers, or the industry.
3. **Hedged, or stated as fact.** Some comments soften their claim ("I think",
   "I wonder if", "maybe"). Others don't. In my data, 29% of critiques hedge
   but only 4% of hot takes (2 of 46) do.
4. **Length.** Supported arguments take more words. Median length is 58 words
   for critique, 36 for reaction, and 22 for hot_take.

One distinction I expected and didn't find: absolute words ("never", "always",
"only", "every") seemed like a hot-take marker, but they appear in 19% of hot
takes and 38–39% of critiques and reactions. Tone words alone don't separate
the labels. Whether there's support behind the claim does.

### `critique`

**Definition:** Makes a claim about a book, author, or reading and backs it with
at least one specific reason: a named feature of the text (a character, scene,
style, or structure), a fact, or a firsthand example.

**Example 1:**
> I normally love Backmann, but My Friends was disappointing. His books dint always have a plot but make me laugh while breaking my heart. My Friends did neither.

**Example 2:**
> I don't like the wheel of time. I think Rand is too whiny and I really dislike how all the women fall in love with him immediately and that practically all they talk about is Rand.

### `hot_take`

**Definition:** States a confident verdict or generalization about a book,
author, genre, or readers, with no specific reason offered.

**Example 1:**
> there are no flaws in Catch-22

**Example 2:**
> Margaret Atwood has seriously lost her touch.

### `reaction`

**Definition:** Describes the writer's own feelings, experience, or reading
habits, without judging the quality of a work or author and without making a
claim about the world that someone else could argue with.

**Example 1:**
> Same. I cried for days already after the first few chapters

**Example 2:**
> For me I just can't read books written in the present tense no matter how good they are.

### The hardest boundary

**Which two labels:** `hot_take` vs. `reaction`

**The borderline post:**
> I have tried several times and just cannot read Sally Rooney. I hate the way she writes.

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

If the post judges the quality of a work or author, good or bad ("amazing",
"perfect", "boring", "overrated", "I hate the way she writes"), with no reason
given, it's `hot_take`, even when it's framed personally with "I" or "to me".
If it only reports what happened to the reader (cried, couldn't finish, didn't
click, read it in two days) and passes no judgment on the work, it's
`reaction`.

So the borderline post above is `hot_take`: "I hate the way she writes" judges
Rooney's writing and gives no reason. "I've tried several times and just
haven't fallen in love" is `reaction`, because it only reports the reader's
experience.

Applying this rule to my existing labels moved five posts from `reaction` to
`hot_take`: verdicts like "so so so good!", "amazing", "perfect", "brilliant",
and "to me they're just…boring". I had been calling enthusiasm `reaction` and
negative verdicts `hot_take`. The rule treats both the same way.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Public comment threads on r/books: discussions
of specific books and authors, "unpopular opinion" threads, and threads about
reading habits. 202 whole comments, copied by hand and not trimmed.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I labelled the posts by hand in an earlier pass, writing a short reason for
each in the `note` column. After writing the `hot_take`/`reaction` decision
rule in Milestone 2, I used an AI tool to recheck my labels against it. That
moved five posts from `reaction` to `hot_take` and one hard case from
`reaction` to `critique`. I read and approved each change, and those six rows
say "AI-assisted, checked" in the note column. I also removed two duplicate
fragments of one Sanderson comment so the same text couldn't land in both the
training split and the test split.

**Cold pass:** 20 randomly chosen rows were relabelled blind, from the written
definitions and decision rule only, and are marked `cold` in the note column.
This was a TA walkthrough of the milestone, so the cold pass was done by an AI
tool, not a person. The cold labels matched the earlier pass on 19 of 20.
The one disagreement was the Rothfuss post ("never going to finish… teasing
for 12 years"). The earlier pass said `hot_take`, but the 12 years of teasing
is a fact backing the claim, so the cold pass said `critique`, and that label
stands.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `critique` | 98 | 48.5% |
| `reaction` | 54 | 26.7% |
| `hot_take` | 50 | 24.8% |
| **Total** | 202 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "I have tried several times and just cannot read Sally Rooney. I hate the way she writes."
>
> *Could have been:* `hot_take` or `reaction`
>
> *I chose, because:* `hot_take`. It's personal ("I have tried", "I cannot
> read"), but "I hate the way she writes" passes a verdict on Rooney's writing
> and gives no reason, which is exactly what my decision rule sends to
> `hot_take`. A near-identical post that only says "I just haven't fallen in
> love" with Rooney stays `reaction`, because it judges nothing.

**2.**
> *The post:* "Oh absolutely. It upset me for days, but it was written so beautifully. As the Swedish Academy put it, 'intense poetic prose that confronts historical traumas and exposes the fragility of human life.'"
>
> *Could have been:* `reaction` or `critique` (and arguably `hot_take`)
>
> *I chose, because:* `critique`. "It upset me for days" is pure `reaction`,
> but the post goes on to judge the writing ("so beautifully"). Under my rule
> that rules out `reaction`. It's not `hot_take` either, because it backs the
> verdict with a quote that names a feature of the text: poetic prose that
> confronts historical trauma. I originally had it as `reaction` and changed it
> once the rule was written.

**3.**
> *The post:* "I think you might be on to something. I have a friend whose son loves to read, but she won't allow video games in the house. He can only play at his friends' houses. I wonder if that's why he's such a dedicated reader."
>
> *Could have been:* `critique` or `reaction`
>
> *I chose, because:* `critique`. It's mostly a personal anecdote, which
> reads like `reaction`, but the anecdote is offered as evidence for a claim
> about the world (that no video games makes kids into readers). A firsthand
> example backing a claim is one of the kinds of support in my `critique`
> definition, even though the claim is hedged ("I wonder if").

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, 128 max tokens, seed 42. Trained
on CPU (torch 2.14.1+cpu), about 4 minutes.

**Anything I changed from the defaults, and why:** Nothing. The only edit was
setting `LABELS` to `["critique", "hot_take", "reaction"]` to match my CSV.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
train 140 · val 31 · test 31

| Label | Train | Val | Test |
|---|---|---|---|
| `critique` | 68 | 15 | 15 |
| `hot_take` | 34 | 8 | 8 |
| `reaction` | 38 | 8 | 8 |

`hot_take` and `reaction` each have exactly 8 posts in the test split, right
at the edge of what's measurable. One misclassified post moves either label's
recall by 12.5 points, so expect their F1 to swing between seeds next unit.


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

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
