---
name: skill-manager
description: Use when a project has no assigned power roster, when starting work in an unfamiliar project, or when the user signals a shift in what the project is doing. Surveys the project's environment and stage, decides which of the installed powers are in force for it, and locks that into the project's CLAUDE.md so it stands in every session without the user ever naming a power.
---

# Skill manager

Around 180 powers are installed. A project needs maybe ten. The manager decides
which ten, once, from what the project *is* — and then those powers are simply
in force.

## The point

**The user never names a power.** If they have to say "use the Ghidra skill,"
the manager has failed. They describe the work, or just do the work, and the
right powers are already standing. That is what frees them to think about the
problem instead of the toolbox.

So the assignment is a property of the **project**, not a reaction to a prompt.
It does not wait for a keyword, a trigger, or a matching phrase. Once assigned
it is permanent for that project.

## Assign once

Run when a project has no roster yet.

### 1. Survey the environment

Read the project rather than asking about it. The evidence is there:

| Look at | Tells you |
|---|---|
| Files and extensions on disk | The languages and formats in play |
| `README.md`, existing `CLAUDE.md` | The stated purpose |
| The memory index for this project | What the work has actually been |
| `git log` (last ~30 commits) | The current stage, not the founding intent |
| Which MCP servers are connected | Which surfaces the work runs through |
| Build files, lockfiles, tool configs | The toolchain that is really used |

Ask the user only for what the project genuinely cannot tell you — usually
nothing.

### 2. Read the stage, not just the subject

The same project needs different powers at different points. A binary port
that is still being reversed needs analysis powers; the same port once it
compiles needs debugging and verification powers; once it runs, graphics and
performance powers. Assign for **where it is now**.

Record that stage in the roster. It is what makes a later shift recognisable
rather than a matter of opinion.

### 3. Choose the roster

Six to twelve powers. Fewer is not caution, it is the point — a roster of
forty is the same as no roster.

Include a power when the project will hit its subject **repeatedly**. Exclude
it when the subject is merely adjacent: a reversing project touches binaries,
so binary analysis belongs; it does not do phishing, so initial-access does
not, however plausible the topic sounds.

Where two powers overlap, take the more specific one and say in one line why
the other lost. That line is what stops the question being reopened every
month.

Include the powers that carry *how the work is done here*, not only the
subject ones — the lab operations power, the evidence standard, the debugging
method. Those are the ones a user would never think to name, which makes them
exactly the ones the manager exists to supply.

### 4. Lock it into the project

Write the roster into the project's root `CLAUDE.md`, creating the file if
needed. That file loads into every session for the project, so the assignment
stands on its own — no trigger to fire, no invocation, nothing for the user to
remember.

Use this block verbatim, appended below whatever the file already holds:

```markdown
## Assigned powers

<!-- Assigned by skill-manager on YYYY-MM-DD. Standing for this project.
     Do not re-derive; change only on an explicit shift. -->

**Stage:** <one line: where the project is now>

These powers are in force for this project. Use them when the work touches
their subject, whether or not they are named.

| Power | In force for |
|---|---|
| `plugin:power-name` | What in this project keeps hitting it |

**Not assigned, deliberately:** `power` (why it lost to the one that won).
```

Then tell the user what was assigned and why, in a few lines. Do not ask them
to approve it — they asked for a manager so they would not have to make this
call. If they disagree they will say so, and that is a shift.

## Then leave it alone

The roster is permanent. Do not re-derive it at the start of a session, do not
quietly add a power because a task brushed against one, and do not drop a power
because it has not come up lately. Drift is the failure mode: a roster that
changes on its own is back to being a guess.

If a task genuinely needs a power outside the roster, use it for that task and
carry on. One-off use is not a reassignment and does not touch the file.

## Reassign only on a shift

A shift is the user saying the project's work has changed. It sounds like:

- "we're moving on to the graphics side now"
- "this is a porting project now, not a reversing one"
- "add the defensive/blue-team category"
- "drop the red-team stuff, we're not doing that here"
- "that roster is wrong"

It is **not** a shift when a single task strays outside the roster, when a new
plugin is installed, or when a session starts.

On a shift: re-run the survey, rewrite the block in place, and keep a one-line
history of what changed and when, so the roster's reasoning stays legible:

```markdown
<!-- 2026-09-03 assigned at reversing stage.
     2026-11-12 shifted to porting: dropped X, added Y. -->
```

## Per project

Each project holds its own roster in its own `CLAUDE.md`. Nothing is global,
so two projects on the same machine never fight, and moving between them needs
no switching step.
