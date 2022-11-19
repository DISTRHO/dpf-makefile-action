
This repository contains a GitHub Action specialized for building DPF-based audio plugins that use Makefile as their build system.

It supports:

- building Linux, macOS and Windows in various architectures
- perform plugin validation for all formats supported by DPF, including run-time memory checks
- upload "nightly" artifact archives for each build
- upload release archives

## Template setup

The latest recommended setup is:

```yaml
jobs:
  linux:
    strategy:
      matrix:
        target: [linux-arm64, linux-armhf, linux-i686, linux-riscv64, linux-x86_64]
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: recursive
      - uses: distrho/dpf-makefile-action@v1
        with:
          target: ${{ matrix.target }}

  macos:
    strategy:
      matrix:
        target: [macos-intel, macos-universal]
    runs-on: macos-11
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: recursive
      - uses: distrho/dpf-makefile-action@v1
        with:
          target: ${{ matrix.target }}

  windows:
    strategy:
      matrix:
        target: [win32, win64]
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: recursive
      - uses: distrho/dpf-makefile-action@v1
        with:
          target: ${{ matrix.target }}

  pluginval:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: recursive
      - uses: distrho/dpf-makefile-action@v1
        with:
          target: pluginval
```

## Documentation

The table below contains all possible properties for use with `distrho/dpf-makefile-action@v1` action.

| Property | Required? | Description                                                |
| -------- | --------- | ---------------------------------------------------------- |
| target   | Yes       | The target platform and architecture to build for          |
| dpf_path | No        | Where DPF is located within your repo, defaults to "./dpf" |
| lto      | No        | Whether to enable Link-Time-Optimizations, defaults to no  |

The table below contains all possible targets and supported runners.

| Target          | Aliases           | Allowed runners            |
| --------------- | ----------------- | -------------------------- |
| linux-arm64     |                   | ubuntu-20.04, ubuntu-22.04 |
| linux-armhf     |                   | ubuntu-20.04, ubuntu-22.04 |
| linux-i686      | linux-i386        | ubuntu-20.04, ubuntu-22.04 |
| linux-riscv64   |                   | ubuntu-20.04, ubuntu-22.04 |
| linux-x86_64    | linux             | ubuntu-20.04, ubuntu-22.04 |
| macos-intel     |                   | macos-11, macos-12         |
| macos-universal | macos             | macos-11, macos-12         |
| win32           |                   | ubuntu-20.04, ubuntu-22.04 |
| win64           |                   | ubuntu-20.04, ubuntu-22.04 |
| pluginval       | plugin-validation | ubuntu-20.04, ubuntu-22.04 |

NOTE: Windows builds use ubuntu runners in cross-compilation instead of Windows ones

## Changelog

### 1

Initial release

## Future Plans

- Integrate with [PawPaw](https://github.com/DISTRHO/PawPaw/) to allow plugins to use many opensource libs seamlessly
- Set up MOD Audio builds
