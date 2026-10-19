# dna_clean_code

English | [Deutsch](README.de.md)

Gives a repo the rules for writing and testing code, language neutral and
for Dart and TypeScript, and skills that check and review it.

## Motivation

The clean code and test conventions used to live in the ggsuite umbrella
layer, so only ggsuite repos got them. This layer lifts them out, so
every organization can inherit the same rules. What holds in every
language is written once; the Dart and TypeScript specifics live in their
own guides next to it. The skills check a repo and review its changes
against those same rules.

## Usage

Declare it as a dev-dependency and initialize once:

```bash
pnpm add -D @ggdna/dna-clean-code   # TypeScript projects
dart pub add dev:dna_clean_code     # Dart projects
gg dna init
```

`gg dna init` places a test that instantiates and verifies the DNA on
every test run. It copies the guides to `doc/guides/` and
`doc/guides/for-ai/`, and the skills to `.claude/skills/`. Usually the
layer arrives through the umbrella layer of the organization, e.g.
`dna_ggsuite` or `dna_tssuite`.

Type `/code` or `/test` in Claude Code to have the repo checked against
the code or the test guide; both list their findings first and fix only
what you confirm. Type `/review-light` or `/review-heavy` to have the
current changes reviewed.

A later layer can adapt the guides without forking them: a file at the
same path replaces a guide whole, and an `.overrides.md` file next to it,
e.g. `code-guide-typescript.overrides.md`, replaces only the sections it
names. The guides mark their exchangeable sections with `@` tags:
`@layout`, `@imports`, `@api`, `@example` and `@naming` in the code
guide, `@nesting`, `@examples` and `@coverage` in the test guide,
`@performance` and `@security` in the review guide.

## Guides

- `dna/doc/guides/code-guide.md` — layout, license header, member order,
  API shape, example constructors, comment landmarks, documentation,
  small functions, naming, saying it once
- `dna/doc/guides/code-guide-dart.md`,
  `dna/doc/guides/code-guide-typescript.md` — what that means in each
  language, including the mandatory lints
- `dna/doc/guides/test-guide.md` — mirroring the source tree, starting
  from the example, real types over mocks, asserting on structure, 100
  percent coverage
- `dna/doc/guides/test-guide-dart.md`,
  `dna/doc/guides/test-guide-typescript.md` — the same, per language
- `dna/doc/guides/for-ai/ai-review-guide.md` — the review checklist an
  agent works through for every changed file, and the interactive fix
  loop
- `dna/doc/guides/for-ai/review-heavy-guide.md` — the review checklist
  plus `/simplify` and `/code-review`

## Skills

- `/code` — checks the code against the code guide and the guide of the
  repo's language
- `/test` — checks the tests against the test guide and the guide of the
  repo's language, and runs the repo's commit check
- `/review-light` — reviews the current changes; follows the AI review
  guide
- `/review-heavy` — the same, plus `/simplify` and `/code-review`;
  follows the review heavy guide

## Scope

The layer only sets the rules and checks them. The commit check that
enforces coverage, the lint configuration and the CI pipeline come from
the repo and from other layers.

## Layers

The layer is orthogonal: `dna/_dna.json` lists no parent layers. Clean
code stands on its own and is inherited by the umbrella layer of each
organization.

## Variables

- `dnaCopyrightHolder` — the name in the license header of every file;
  `mustOverride` in `dna/_dna.json` marks it, so every repo that lists
  this layer sets it in its own `dna/_vars.json`

## Development

The `dna/` folder is hand-authored source and is never generated. The repo
instantiates its own DNA — run `dart test` after changes; commit first, a
file the DNA would overwrite must not carry uncommitted work.

Edit the files below `dna/`, not their copies in `doc/` and `.claude/`,
which the test overwrites.

## License

MIT — see [LICENSE](LICENSE).
