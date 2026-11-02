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

Review the whole repo, not the current diff. Think like a senior
developer who will still maintain this code in two years.

## 1. Understand the goals

Read the sources the guide names under `@goals`. If the repo has
`doc/guides/pm-repo-guide.md`, find the PM repo of the project and read
its goals, requirements and `architecture/architecture.md`.

Write a short list of where the project is going, each point marked as
stated (with its source) or inferred. Ask the user about the inferred
points before you build the target picture on them.

## 2. Map the structure as it is

List the modules or packages, what concept each one holds, its public
API and what it depends on. Draw the dependencies as a mermaid diagram.

## 3. Check it against the guide

Walk through every section of the guide. Report each finding with the
file and line it shows in, the section it breaks and what it costs once
the goals from step 1 arrive.

Report abstractions without a goal behind them just as firmly as missing
seams.

## 4. Design the target structure

Describe the structure that carries the goals: the concepts to
introduce, what moves where, which seams to add and which to remove.
Draw it as a mermaid diagram next to the current one.

Offer at least one alternative and weigh the two. Say plainly which
goal each change serves; drop a change that serves none.

## 5. Plan the steps

Order the changes into steps that are each shippable on their own, as
the guide describes under `@migration`. Name for each step what it
changes and how it is tested.

## 6. Report before changing

Present the findings, the target picture and the steps. Change nothing
yet.

Once the user confirms, record the target picture: in
`architecture/architecture.md` of the PM repo if there is one, otherwise
in `doc/architecture/architecture.md` of this repo, with the diagrams in
`img` next to it as `.mmd` files. Implement only the steps the user
confirms.
