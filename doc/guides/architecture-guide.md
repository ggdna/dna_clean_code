<!--
@license
Copyright (c) ggsuite

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Architecture Guide

How a repo is cut into parts, and how those parts grow. The
[Code Guide](./code-guide.md) holds the rules for a single file and
function; this guide holds the rules above them. Shape the structure the
way a senior developer would: from where the project is going, not only
from what it does today.

## Start from where the project is going

- Read the goals before the code: the PM repo of the project
  (`goals`, `requirements`, `architecture/architecture.md`) if there is
  one, the concept in `CLAUDE.md`, the README, open tickets and the
  CHANGELOG
- Derive what will be added next and where the system will vary: new
  data sources, new output formats, new platforms, more users, more of
  the same kind of thing
- Keep what the sources state apart from what you infer; an inferred
  goal is a hypothesis until someone confirms it
- Judge every structure by how well it carries those goals, not by how
  well it carries today's code

## Cut along concepts

- One concept per module, one top level concept per package
- Name a module after the concept it holds, never after its technique
  (`helpers`, `utils`, `misc`, `common`)
- Let dependencies point one way: from the edges (UI, CLI, IO, network,
  framework) towards the core that holds the domain
- Keep the core free of IO, UI and framework types, so it can be tested
  and reused on its own
- Allow no cycles between modules or packages

## Hide the inside

- Keep the public API small; everything a caller does not need is
  private
- Give every piece of state exactly one owner; others ask the owner
  instead of reaching into its data
- Hand out immutable values, not references to internal collections
- Hide a third party library behind an interface of your own once more
  than one module depends on it

## Say each concept once

- The same idea implemented in two modules is one concept that is
  missing; give it a home and let both use it
- Lift a generic piece into its own package as soon as a second consumer
  exists, or the goals name one
- Two places that change together for every new feature are one concept
  split in two
- Not every likeness is duplication: two pieces that look alike but
  change for different reasons stay apart

## Grow by adding, not by editing

- Where the goals predict variation, put a seam: an interface, a
  strategy, a registry, a plugin point
- Add a new kind of thing as a new type that fulfils the interface,
  instead of a new branch in every switch over the kinds
- Keep the seam where the variation is, not one level above or below
- Configure behaviour through data where the variants differ only in
  values

## Do not build for imagined futures

- A seam needs a goal behind it; an abstraction with a single
  implementation and no goal that asks for a second one is a finding
- Prefer the simplest structure that carries the goals over the most
  general one
- Generic frameworks inside an application, layers that only pass calls
  through and options nobody sets are cost without return

## Watch for structure that grew by accretion

Code grown feature by feature, especially by an agent, tends to bolt new
behaviour onto what is already there. Look for:

- A class or file that grows with every feature and knows a little of
  everything
- A switch or `if` chain over a type or kind, repeated in several places
- A boolean or mode parameter that turns a function into two functions
- A whole module copied and slightly changed instead of shared
- UI or CLI code that reads storage, network or files directly
- Shared mutable state that several modules write to
- A dumping ground module that everybody imports
- A change that touches many unrelated files for one feature

## Change the structure in small steps

- Draw the target picture first, then plan the way there
- Make every step shippable: the tests stay green and the release works
  after each one
- Put the new structure next to the old one, move the callers over, then
  delete the old one
- Change the structure and the behaviour in separate steps, never in the
  same commit
- Record the target picture where the project keeps it, so the next
  change follows it
