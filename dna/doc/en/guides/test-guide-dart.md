<!--
@license
Copyright (c) dnaCopyrightHolder

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Test Guide — Dart

What the [Test Guide](./test-guide.md) means in a Dart or Flutter
package. Read that one first; this only fills in the specifics.

## @source Mirror the source tree

- `lib/src/foo.dart` goes to `test/foo_test.dart`
- `lib/src/sub/bar.dart` goes to `test/sub/bar_test.dart`
- Open the file with the license header and a top level `main()`

## @import Import through the public API

- Import `package:test/test.dart`, or
  `package:flutter_test/flutter_test.dart` in a Flutter package
- Import your own package through its barrel file
- Reach into `package:<pkg>/src/…` only to test a helper that is
  deliberately not exported

## @group Nest with group and test

- `group(…)` by the member under test, `test(…)` per case

## @closures Set up with closures

- Reset shared state in `setUp`
- Release external resources in `tearDown`
- Put reusable setup into a local closure inside `main()`

## @examples Start from the example

- `final user = User.example(age: 17);`

## @real Prefer real types over mocks

- Reach for `mockito` or `mocktail` only when nothing else works

## @structure Assert on structure

- Compare whole lists with `equals([…])`
- Check exceptions with `throwsA(isA<X>().having(…))`
- Never `print` in a test

## @coverage Reach 100 percent coverage

- Mark a genuinely unreachable line with `// coverage:ignore-line`, a
  block with `// coverage:ignore-start` and `// coverage:ignore-end`

## @validate Validate before you commit

```bash
gg can commit
```

`dart analyze`, `dart format` and `dart test` all have to be clean.
