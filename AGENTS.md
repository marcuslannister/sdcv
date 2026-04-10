# Repository Guidelines

## Project Layout

- `src/` contains the `sdcv` C++ command-line application and StarDict parsing code.
- `tests/` contains shell test scripts and fixture dictionaries.
- `doc/` contains man pages.
- `po/` contains gettext translation files.
- `cmake/` contains project-specific CMake helper modules.

## Build

Use an out-of-tree CMake build:

```sh
cmake -S . -B build
cmake --build build
```

To build with tests enabled:

```sh
cmake -S . -B build -DBUILD_TESTS=True
cmake --build build
```

The project requires CMake 3.10 or newer, a C++11 compiler, zlib, and GLib 2.36 or newer. Readline is optional and controlled by `-DWITH_READLINE=True|False`. NLS/gettext support is enabled by default and can be disabled with `-DENABLE_NLS=False`.

## Tests

Run the full test suite from the build directory:

```sh
ctest --test-dir build --output-on-failure
```

CI uses:

```sh
mkdir build
cd build
cmake -DBUILD_TESTS=True ..
make -k -j2 VERBOSE=1
ctest --output-on-failure
```

Most tests are shell scripts in `tests/` and receive the built `sdcv` executable path plus the source `tests` directory.

## Coding Style

- Follow the existing C++11 style in `src/`.
- Use the checked-in `.clang-format` configuration for C++ formatting.
- Keep changes portable across Linux and macOS where practical.
- Prefer GLib utilities where the surrounding code already uses them.
- Avoid broad refactors when making focused fixes.

## Behavior Notes

- Command-line behavior is exercised through shell tests; add or update tests when changing CLI output, search behavior, dictionary loading, JSON output, or return codes.
- JSON output tests depend on `jq`.
- Locale and gettext behavior can affect output, so keep user-visible strings and test expectations aligned.
