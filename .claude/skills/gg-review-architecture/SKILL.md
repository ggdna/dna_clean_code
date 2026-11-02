---
name: gg-review-architecture
description: Reviews the architecture of this repo against its goals and the architecture guide, and proposes a target structure and the steps to get there. Use when the user says "/gg-review-architecture" or asks for an architecture review, a restructuring or how the structure should evolve.
---

<!--
@license
Copyright (c) ggsuite

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# GG Review Architecture

Read `doc/guides/architecture-guide.md` and follow it. Read
`doc/guides/code-guide.md` as well; this review works one level above it.
If the architecture guide is missing, the DNA of this repo is out of
date: say so and stop.

Review the whole repo, not the current diff, and look beyond it where the
guide says so. Think like a senior developer who will still maintain
this code in two years.

## 1. Understand the goals and the role

Determine the role of the repo as `@goals` describes, with evidence. If
the ticket names a project management repo, or a repo in `.ocean` covers
this project (see its `index.jsonc`), read its goals, requirements and
the architecture of the current quarter
(`doc/<yyyy>-Q<n>/architecture/architecture.md`).

Write a short list of where the project is going, each point marked as
stated (with its source) or inferred (I1, I2, ...). Carry inferred points
on as hypotheses and make every recommendation that rests on one
conditional ("only if I1 holds"). Collect the questions for the top of
the report. Stop and ask before step 2 only when one answer would turn
the whole review around, such as whether this repo may diverge from the
repo it follows.

## 2. Measure and map the structure

Measure before you read in depth: lines per file, commits per file (for
example `git log --since=1.year --name-only --format=`, counted per
file; take the whole history when the repo has fewer than about 50
commits), files that change together, and how many files import each
one. Read the largest, the most changed and the most imported files
first. When the history says nothing (a port, a squashed import), say so
and use the upstream history.

Group the files into the concepts they hold; in a small package a concept
may be one file. List for each what it holds and what it depends on.
Take the public API at the package level. Draw the dependencies between
concepts as a mermaid diagram of at most fifteen nodes, only when there
are five concepts or more. Mark cycles that exist only through type
imports apart from runtime cycles.

Check what `CLAUDE.md`, the README and existing architecture docs claim
about the structure against the code; a false claim is a finding.

## 3. Look at the neighbours

For each concept in the map and each concept you are about to propose,
check whether a dependency (`pubspec.yaml`, `package.json`,
`interfaces.uses` in `index.jsonc`) or a repo in `.ocean` whose `summary`
or `domain` names it already holds it. For a library, find its consumers
as `@contracts` describes. Search only for the concepts of this repo. A
fix that belongs in another repo goes into the report as a proposal for
a workspace ticket; do not plan it in detail.

## 4. Check and weigh

Check the structure against every section of the guide, but report by
weight, not by section. Open with a verdict of two or three sentences:
is the structure the bottleneck for the goals at all, and if so, which
one thing to fix first.

Report only what costs something against a goal from step 1 or today
(see `@yagni`). Back each cost with evidence: churn, the size of the last
comparable change, the number of places a new variant would touch. Rank
by that cost times the likelihood of the goal. Show the top three in
detail and at most five more in one line each. Anchor each finding where
it can be checked: file and line or range with the symbol named, a count
of places for a spread finding, or the search you ran for something
missing. Cite only lines you opened in this session. List the sections
without a finding in one line ("checked, none: ..."); never write a
finding to fill a section.

## 5. Design the target structure

Describe the structure that carries the goals: what moves where, which
seams to add or remove, and a new concept only where a goal asks for it.
The target may be the current structure. Draw it next to the current
diagram when it differs.

Weigh it against at least one alternative; changing nothing always
counts as one. Vary where the change lives (here, in a dependency,
upstream) as well as how far it goes. Give each option its cost
(packages, releases, breaking changes for consumers, test rework), the
quality attribute it trades against, and the point from which it would
win. Say which goal or present cost each change serves; drop a change
that serves none.

## 6. Plan the steps

Order the changes into steps that are each shippable on their own, as
the guide describes under `@migration`. Name for each step what it
changes, which tests move, are added or pin behaviour, whether it breaks
a consumer, and which repo has to release first.

## 7. Report before changing

Present the verdict, the questions, the findings, the target picture and
the steps. Change nothing yet.

Once the user confirms, record the target picture only if it differs
from the current structure, where the project already keeps it: this
repo's section in the project management repo's current
`architecture/architecture.md` (follow its `pm-repo-guide.md`, and only
if it is checked out locally; otherwise hand the section over in the
report), else an existing `doc/architecture/architecture.md`; create
`doc/architecture/architecture.md` only if none exists, with the
diagrams as `.mmd` files in `img` next to it. Let `CLAUDE.md` link to it
instead of repeating it. Record each confirmed decision, including
tempting options you rejected, in the project management repo's
`concepts/decisions`, else in a "Decisions" section of that file.

Implement the confirmed steps one at a time, each as its own commit with
the commit check of the repo; stop when a check fails or a step turns
out different from the plan.
