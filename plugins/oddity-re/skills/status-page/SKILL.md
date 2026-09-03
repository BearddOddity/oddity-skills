---
name: status-page
description: Use when someone asks where a project stands, how far along it is, what has been done, what was learned, or what happens next - and whenever a milestone, investigation or long session ends with a result worth showing. Surveys the project for evidence, works out an honest completion percentage or refuses to invent one, and publishes a visual status page with a progress bar, categorised breakdowns, plain-language findings and a next-steps section.
---

# Status page

A project's real state lives scattered across a git log, a test run, a task
file and somebody's memory. This turns that into **one page a person can look
at and understand in thirty seconds** — with a progress bar, real percentages,
and findings written so that someone who has not been in the code can follow
them.

The page is generated from evidence, published as an artifact, and updated in
place. It is not a summary of the conversation. It is a report on the project.

## What makes this hard, and the rule that fixes it

The tempting failure is a made-up percentage. "About 70% done" with nothing
behind it reads as authoritative and is worthless — worse than worthless, since
it will be quoted back later.

**A percentage needs a denominator you can point at.** Find a real one or show
no percentage. Real denominators look like:

| Denominator | Where it comes from |
|---|---|
| Tests passing / total | The test runner's own output |
| Tasks done / total | An issue tracker, a plan file, a checklist |
| Modules or endpoints implemented / specified | A spec, an interface list, a schema |
| Items processed / items found | A migration list, a scan result |
| Milestones passed / planned | A roadmap with named milestones |
| Coverage | A coverage tool, never an estimate |

If none exists, say so on the page in one line — *"No completion percentage:
this project has no fixed scope to measure against"* — and show **direction**
instead: what moved this week, what the counters were before and after. A trend
is honest where a percentage is not.

**Never mix denominators in one number.** If tests are 80% and tasks are 40%,
that is two bars, not one average. Averaging them invents a measurement nobody
made.

## Gathering the evidence

Read the project. Ask the user only for what the project genuinely cannot tell
you, which is usually nothing.

| Look at | Gives you |
|---|---|
| `git log` (last ~30, plus first-parent for merges) | What was actually done, and when the pace changed |
| `git log --stat` on the recent range | Which areas are moving and which are untouched |
| Test/build command output | The pass/fail denominator, and whether it even runs |
| `README`, `CLAUDE.md`, docs | Stated purpose and definition of done |
| Issue tracker, `TODO`, plan or roadmap files | Task denominator and the next-steps list |
| A project ledger, decision log or progress file | Findings, and which were later overturned |
| CI config and its recent runs | What is verified automatically vs by hand |
| Lockfiles, manifests | The real toolchain, for the "how to run it" line |
| Open branches and their age | Work in flight, and work quietly abandoned |

Prefer a command's own output over your reading of the code. "127 of 149 tests
pass" is evidence; "most tests pass" is not.

**Run the test/build command if it is safe and quick.** A status page built on a
stale number is the exact failure it exists to prevent. If you cannot run it,
label the figure with when it was last measured.

## What the page must contain

Five sections, in this order. The order matters — it answers the reader's
questions in the order they ask them.

### 1. Where it stands

One sentence, first, in plain words: what state is this project in. Then the
progress bar or bars, each labelled with its denominator in full — *"Tests: 127
of 149 passing"*, not *"85%"* on its own.

Add three or four headline counters with their direction of travel. A number
with an arrow from its previous value is worth far more than a number alone.

### 2. What was done

Grouped by area, not by commit. A reader does not care that there were eleven
commits; they care that authentication is finished and the importer is halfway.
Say what changed and what it means, one line each.

### 3. What was found

The part most status reports skip and the part people actually read. Every
non-obvious discovery, explained so a newcomer follows it:

- **What was believed**, then what turned out to be true.
- **The evidence** — the measurement, the log line, the reproduction.
- **What it cost or saved**, if that is known.

Include the things that turned out to be wrong. A finding that was refuted is
as useful as one confirmed, and a page that only lists wins is not trusted.

### 4. What happens next

Ordered, and honest about dependency. Say which step blocks which. For each:
what it is, why it is next, and what "done" looks like for it. If something is
blocked on a person or a decision rather than on work, say so and name what is
needed — that is often the single most useful line on the page.

### 5. The honest caveat

What the numbers do not capture. Known shortcuts, work resting on assumptions,
anything that will make the figures look worse before better. If there is
genuinely nothing, write one line saying so rather than dropping the section —
its absence reads as concealment.

## Writing it

**Plain language throughout.** Assume an intelligent reader who does not know
this codebase. Expand jargon on first use. Prefer "the routine that stores an
entry" over the symbol name, then give the symbol in code font once. Someone
should be able to read the page aloud to a colleague.

**Answer first, then evidence.** Every section opens with the conclusion.

**Short paragraphs, and bold the phrase that carries the point.** Long
undifferentiated prose is where status pages go to die.

**Numbers exact, units explicit, dates absolute.** "Last Tuesday" is useless in
a document that will be read in a month.

## Building and publishing it

Before writing the page, **load the `artifact-design` skill** — it governs how
much design the request warrants and the theme, responsive and accessibility
rules. This skill decides *what goes on the page*; that one decides *how it
looks*. Do not hand-roll a stylesheet in place of reading it.

Then write the HTML to a file and publish it as an artifact, so the user gets a
link they can share rather than a wall of terminal text.

**Charts and bars.** A progress bar is a labelled div, not a charting library.
If the page genuinely needs a chart — a trend over time, a breakdown by
category — load the `dataviz` skill first. Draw a sparkline or a bar series as
inline SVG rather than pulling in a dependency.

**Re-running.** Publish updates to the same artifact URL so the link stays
stable and history accumulates. Keep the title fixed across updates; the user
finds the page by its name.

**Generating it repeatedly.** If the project will want this page often, write a
small script that emits it from the project's own data, and commit that instead
of regenerating by hand. A page that is rebuilt by a command stays true; one
rebuilt by memory drifts. Put the prose that does not change in the script and
let the numbers come from the evidence.

## What not to do

- **No invented percentages.** Covered above; it is the one that matters.
- **No progress bar without its denominator written next to it.**
- **Do not present a conversation summary as project status.** What was
  discussed is not what was done.
- **Do not quietly drop the caveat section** because the news is good.
- **Do not let the page and the project's own record disagree.** If the project
  keeps a ledger or progress file, the page is a view of it, not a rival to it.
  Update the record first, then generate the page from it.
- **Do not overstate a finding's certainty.** If one run showed it, say one run
  showed it.
