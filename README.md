# setup-dart

[![Dart](https://github.com/dart-lang/setup-dart/workflows/Dart/badge.svg)](https://github.com/dart-lang/setup-dart/actions?query=workflow%3A%22Dart%22+branch%3Amain)

This [GitHub Action]() installs and sets up of a Dart SDK for use in actions by:

* Downloading the Dart SDK
* Adding the `dart` command and `pub` cache to path

# Usage

## Basic

Install the latest stable SDK, and run Hello World.

```
name: Dart

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - uses: dart-lang/setup-dart@v1

      - name: Install dependencies
        run: dart pub get

      - name: Hello world
        run: dart bin/hello_world.dart
```

## Check static analysis, formatting, and tests

Various static checks:

  1) Check static analysis with the Dart analyzer
  2) Check code follows Dart idiomatic formatting
  3) Check that unit tests pass

```
...
    steps:

      - name: Install dependencies
        run: dart pub get

      - name: Verify formatting
        run: dart format --output=none --set-exit-if-changed .

      - name: Analyze project source
        run: dart analyze

      - name: Run tests
        run: dart test
```

## Matrix testing

Double matrix across two dimensions:

  - All three major operating systems: Linux, macOS, and Windows.
  - Dart SDK: stable, beta, dev, or a specific version string.

```
name: Dart

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        sdk: [stable, 2.7.2, 2.12.0-1.4.beta, beta, dev]
    steps:
      - uses: actions/checkout@v2
      - uses: dart-lang/setup-dart@v1
        with:
          sdk: ${{ matrix.sdk }}

      - name: Install dependencies
        run: pub get

      - name: Run tests
        run: pub run test
```

> __NOTE:__ The above example is using the deprecated global `pub` executable instead of `dart pub` - which was released in `2.12.0`.
> 
> When specifying a version that precedes the `2.12.0` release _(like `2.7.2` from the above example)_ - the commands need to work across all versions in the matrix.
> 
> If you are not testing version(s) that precede the `2.12.0` release, use `dart pub get` / `dart test` instead of `pub get` / `pub run test`.

# License

See the [`LICENSE`](LICENSE) file.