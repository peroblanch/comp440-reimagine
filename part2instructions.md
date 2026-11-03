# Part 2 (reimagined): Direct Claude to build a retrieval report

**Replaces Parts 2 to 5 of the original activity. Individual · graded for good faith plus the evidence in your repo.**

## Why this part is different

In the original activity, Claude asked you one question at a time. That is easy to follow
and easy to finish without ever seeing the whole project: each answer is a small problem-set item.

Here you sit in the driver's seat. **You decide what you are trying to find out, you tell Claude
what to build, and you check what it hands back.** Claude is a fast, tireless builder who
will happily produce confident-sounding output. Your job is to be the one who knows what the
finished thing should look like, and who can tell whether Claude got there.

Before you start, copy [suggested_claude_md.md](suggested_claude_md.md) into this repo as
`CLAUDE.md`. It tells Claude to build from your brief, show its assumptions, and keep a log of
who made each decision.

## Where you start

Parts 0 and 1 are done: the repo, packages and embedding model are installed, and the three
TF-IDF settings in `tfidf_settings.py` are chosen (with reasons in `WRITEUP.md`). You still
need your own predictions (Milestone 0), because a prediction only counts if it comes first. If
you would choose different settings, change them now and say why in `WRITEUP.md`.

## The final goal

By the end you have a repository containing four things:

1. **`results/tfidf.json` and `results/embed.json`**: the question bank scored with both methods
   (made by `evaluate.py`; Claude runs it).
2. **`analysis.py`**: a small script, built by Claude to your specification, that prints a
   breakdown of where each method wins, and lists the question ids in each group.
3. **`REPORT.md`**: a one-page answer, in your words, to this question:

   > *For which kinds of movie questions should we trust keyword search (TF-IDF), which
   > embeddings, and why? And what is one concrete way to combine them?*

   Every claim in it cites question ids and numbers that you checked yourself.
4. **`SPEC.md` and `DECISIONS.md`**: your brief to Claude (written before it built anything) and
   the log of every choice point, saying who decided and why. These show the work was
   yours.

The point is not that Claude writes good code. The point is that you could stand in front of
someone and defend every sentence of `REPORT.md`, and explain what `analysis.py` does.

## What you are building on

* `data/movies.jsonl`: 4,309 movies with plot summaries.
* `data/questions.jsonl`: 518 questions, each with one right movie. 100 written questions
  (kinds: detail, reworded, vague) and 418 real tip-of-the-tongue requests.
* `top_terms.py "<title>"`: a movie's highest-weight TF-IDF words.
* `search.py "<question>" [--method tfidf|embed|both]`: the top 10 for a question.
* `evaluate.py --method tfidf|embed`: scores the whole bank and writes `results/<method>.json`.
  `evaluate.py --show <id>`: prints the saved results for a question.
* `site.py`: the comparison site at http://localhost:8765.

## Milestones

Each milestone has a **you do** part, a **Claude does** part and a **check** that proves you
did not just accept the output. Commit at the end of each one (`Milestone N done`) and push.

### Milestone 0: Brief and predictions (before anything runs)

**You do:**
* Write `SPEC.md` in your words, with:
  * the question you are answering (in your own words, not copied from this page);
  * what each deliverable must contain and how you will know it is right ("acceptance checks",
    for example "every number in the report can be reproduced by a command");
  * two predictions: which method does better on questions that name a character, place or
    thing, and which on vague descriptions, and why.
* Do not run `search.py`, `evaluate.py` or `site.py` yet.

**Claude does:** nothing, or reads your brief and tells you any ambiguity that would change
what it builds. It does not rewrite your brief.

**Check:** `SPEC.md` is committed before `results/` exists.

### Milestone 1: Get the evidence

**You do:** Tell Claude to run the tools you need to see how each method behaves. At a minimum:
* top terms for two movies you know;
* three searches in your own words (not Claude's), at least two different kinds of question;
* the full question bank with both methods.

Before each run, say aloud (type it) what you expect to see.

**Claude does:** runs the commands and pastes the output unedited.

**Check:** in `DECISIONS.md`, one line per run: what you expected, what you got, and whether it
surprised you. At least one entry must be a surprise or a miss that you can explain.

### Milestone 2: Specify and build the analysis tool

**You do:** Describe to Claude, in a paragraph, what `analysis.py` must print. Your
specification must include at least:
* top-1 and top-10 counts per method for each question kind (detail, reworded, vague, real);
* the ids where only TF-IDF found the movie in the top 10, where only embeddings did, and where
  both missed.

You choose the output format. Claude may suggest additions; you decide which to keep.

**Claude does:** states its understanding and assumptions, then builds it. It tells you where
you should verify it.

**Check (you must do this one yourself, not Claude):** take one number from the output, for
example "TF-IDF top-10 on vague questions", and recompute it by a different route
(a grep or `jq` on `results/tfidf.json`, or counting by hand on `evaluate.py --show`). Record
the number, the route and whether they match in `DECISIONS.md`. If they do not, find out which
is wrong.

### Milestone 3: Investigate disagreements

**You do:**
* Pick at least three questions from "only TF-IDF" and three from "only embeddings", plus two
  where both missed. You pick them; do not ask Claude which are interesting.
* For each, write a **hypothesis** about why, in your words, before asking Claude anything
  about it.
* Test at least two hypotheses with a new search of your own: change a word in the question and
  see whether the rank moves the way you predicted.

**Claude does:** runs what you ask, shows the top 10, and answers factual questions (what a
score is, what a matched word was). It labels anything that is its own interpretation as such.

**Check:** a table in `REPORT.md` (or an appendix) with columns *id · your hypothesis · what
the test showed · hypothesis held, partly, or not*. At least one hypothesis should be wrong or
only partly right.

### Milestone 4: Challenge Claude

**You do:** Write your draft conclusion in two or three sentences. Ask Claude to argue the
strongest case *against* it, and to name any data in `results/` that would undercut it. Then
decide: keep, change or drop, and why.

Separately, choose **one factual claim Claude made earlier** in the session (about the code, a
score or a method) and verify it against the code or the raw data.

**Check:** `DECISIONS.md` has the argument against, your decision, and the claim you verified
with the result (confirmed, wrong, or could not tell).

### Milestone 5: Report and a combination you can defend

**You do:** Write `REPORT.md`, in your words:
* the answer to the question, with at least three claims, each tied to question ids and numbers;
* what you could not conclude, and one way the evidence could mislead (small samples per kind,
  one right movie per question, 100 written questions versus 418 real ones, and so on);
* a "how it works" paragraph on `analysis.py` that someone who did not see the session could
  follow;
* one concrete way to combine the two methods, and what you would measure to know if it helped.

**Claude does:** checks every number in your report against `results/` and flags any that do not
match or any claim not backed by an id. It does not write the claims for you. It may fix
formatting if you ask.

**Stretch (Thursday's topic):** have Claude implement your combination and score it on the bank.
Report whether it beat both methods, and on which kinds.

## Sample rubric

Graded for good faith, so this shows what a complete submission looks like rather than a
points race. A milestone is **complete** when the evidence is in the repo.

| Milestone | Complete when | Evidence of your own thinking |
|---|---|---|
| 0. Brief | `SPEC.md` committed before any results exist, with question, acceptance checks, two predictions | Brief is in your words, and the predictions have reasons |
| 1. Evidence | Both results files exist; two top-term runs, three searches of your own | Expected-versus-got line for each run, with at least one surprise |
| 2. Tool | `analysis.py` runs and prints the specified breakdown | One number recomputed independently, and the result recorded |
| 3. Disagreements | Eight or more questions examined, two or more hypotheses tested | Hypotheses written before asking Claude; one that failed or only partly held |
| 4. Challenge | Argument against your conclusion, plus a Claude claim you verified | Your decision (keep, change or drop) and why |
| 5. Report | `REPORT.md` with three or more evidence-backed claims, limits, combination idea | You can explain `analysis.py` and each claim without looking at the chat |

**Strong submissions** also: reject or modify at least one Claude suggestion with a reason;
catch an error or an unsupported claim; tie the combination idea to a specific pattern in the
results; and say, in `DECISIONS.md`, which choices were left to Claude's defaults.

**Not enough:** a report whose claims you cannot trace to ids; numbers pasted from Claude's
summary without checking; a `DECISIONS.md` with every line saying "accepted".

## Starting prompts (adapt, do not paste)

These show the posture: you state the outcome and the constraints, Claude builds. Rewrite
them in your own words.

* "I'm answering this question: <yours>. Here is my SPEC.md. Tell me any ambiguity that would
  change what you build, then wait."
* "Build analysis.py to print <your spec>. Before you write it, list your assumptions. After,
  tell me where I should check it."
* "Here's my conclusion: <yours>. Argue the strongest case against it using the results files."
* "Check every number in REPORT.md against results/. List mismatches and unsupported claims.
  Don't fix the claims."

## AI guidelines

The class's [AI agreements](https://docs.google.com/document/d/1JhAzzTbECGEE5IdOwabVJUh36YZ5DD6ozG_cvodqc9s/edit)
apply. Here Claude writes and runs code and explains anything you ask. The question, the
hypotheses, the conclusions and the report's claims are yours. Keep the chat: it is how
`DECISIONS.md` can be checked against what happened.

## Submit

Push the repo with `SPEC.md`, `DECISIONS.md`, `analysis.py`, `REPORT.md`, `WRITEUP.md` and both
`results/*.json`, then submit the Work Submission Form with your repository's URL, as in the
original assignment.
