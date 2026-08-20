# dna_clean_code

The DNA layer for how code is written and tested — the rules that hold in
every language, plus the specifics of the languages we use.

## Guides

- `dna/doc/guides/code-guide.md` — layout, API shape, naming, small
  functions, saying it once
- `dna/doc/guides/code-guide-dart.md`,
  `dna/doc/guides/code-guide-typescript.md` — what that means in each
  language
- `dna/doc/guides/test-guide.md` — mirroring the source tree, real types
  over mocks, asserting on structure, 100 percent coverage
- `dna/doc/guides/test-guide-dart.md`,
  `dna/doc/guides/test-guide-typescript.md` — the same, per language
- `dna/doc/guides/for-ai/ai-review-guide.md`,
  `dna/doc/guides/for-ai/review-heavy-guide.md` — the review checklist
  an agent works through

## Skills

- `/code` — checks the repo against the code guide
- `/test` — checks the tests against the test guide
- `/review-light` — reviews the current changes
- `/review-heavy` — the same, plus `/simplify` and `/code-review`

## Layers

Orthogonal — it declares no parents and is inherited by the umbrella
layer of each organization.

## Variables

- `dnaCopyrightHolder` — the name in the license header of every file

## Adapting it

The language guides are ordinary DNA files, so a later layer can replace
or patch them:

- a same-path `dna/doc/guides/code-guide-typescript.md` replaces the file
- a `dna/doc/guides/code-guide-typescript.overrides.md` patches only the
  sections it names

The neutral guides mark their exchangeable sections with `@` tags —
`@layout`, `@imports`, `@api`, `@naming`, `@nesting`, `@coverage`,
`@performance`, `@security` — so an override can replace one section
without restating the rest.

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
