# Example

`dna_clean_code` is a DNA layer: it ships the code and test guides plus
the skills that check a repo against them, no code. Declare it as a dev
dependency and instantiate it once:

```bash
dart pub add dev:dna_clean_code            # Dart projects
pnpm add -D @ggdna/dna-clean-code          # TypeScript projects
gg dna init
gg dna build
```

Usually it arrives through the umbrella layer of your organization, e.g.
`dna_ggsuite` or `dna_tssuite`. The guides then ask, among other things,
for an `example` constructor on every class:

```dart
final user = User.example(age: 17);
```
