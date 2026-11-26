# dna_clean_code

The DNA layer for how code is written and tested — the rules that hold in
every language, plus the specifics of the languages we use.

## Guides

- `dna/doc/en/guides/code-guide.md` — layout, API shape, example
  constructors, placeholder identifiers, naming, small functions, saying
  it once
- `dna/doc/en/guides/code-guide-dart.md`,
  `dna/doc/en/guides/code-guide-typescript.md` — what that means in each
  language
- `dna/doc/en/guides/architecture-guide.md` — starting from the goals and
  the role of the repo, cutting along concepts, hiding the inside,
  treating the public API as a contract, saying each concept once across
  repos, growing by adding, not building for imagined futures, deciding
  where new code goes, structure smells, changing the structure in small
  steps
- `dna/doc/en/guides/test-guide.md` — mirroring the source tree, starting
  from the example, real types over mocks, asserting on structure, 100
  percent coverage
- `dna/doc/en/guides/test-guide-dart.md`,
  `dna/doc/en/guides/test-guide-typescript.md` — the same, per language
- `dna/doc/en/guides/for-ai/ai-review-guide.md`,
  `dna/doc/en/guides/for-ai/review-heavy-guide.md` — the review checklist
  an agent works through

Every file of `dna/doc/en/` also ships in German and Farsi, with the
same name:

- `dna/doc/de/guides/`
- `dna/doc/de/guides/for-ai/`
- `dna/doc/fa/guides/`
- `dna/doc/fa/guides/for-ai/`

## Skills

- `/code` — checks the repo against the code guide
- `/test` — checks the tests against the test guide
- `/review-light` — reviews the current changes
- `/review-heavy` — the same, plus `/simplify` and `/code-review`
- `/gg-review-architecture` — reviews the structure of the whole repo
  against its goals and the architecture guide, proposes a target
  structure and the steps to get there

## Publishing

`dna/dot-pubignore` keeps `/doc/*/guides/` out of the package `dart pub publish`
uploads. helix joins it with the ignore files of the other layers into the
project's `.pubignore`.

## Layers

Orthogonal — it declares no parents and is inherited by the umbrella
layer of each organization.

## Variables

- `dnaCopyrightHolder` — the name in the license header of every file

## Adapting it

The language guides are ordinary DNA files, so a later layer can replace
or patch them:

- a same-path `dna/doc/en/guides/code-guide-typescript.md` replaces the file
- a `dna/doc/en/guides/code-guide-typescript.overrides.md` patches only the
  sections it names

The neutral guides mark their exchangeable sections with `@` tags —
`@layout`, `@imports`, `@api`, `@naming`, `@nesting`, `@coverage`,
`@performance`, `@security`, `@architecture`, and in the architecture
guide `@goals`, `@boundaries`, `@encapsulation`, `@contracts`, `@dry`,
`@extension`, `@yagni`, `@placement`, `@smells`, `@migration` — so an
override can replace one section without restating the rest.

## Usage

Declare it as a dev-dependency and initialize once:

```bash
pnpm add -D @ggdna/dna-clean-code   # TypeScript projects
dart pub add dev:dna_clean_code     # Dart projects
gg dna init
```

The placed test instantiates and verifies the DNA on every test run.

## Development

The `dna/` folder is hand-authored source and is never generated. The repo
instantiates its own DNA — run `dart test` after changes; commit first, a
file the DNA would overwrite must not carry uncommitted work.
