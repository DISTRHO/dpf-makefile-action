
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
      - uses: actions/checkout@v4
        with:
          submodules: recursive
      - uses: distrho/dpf-makefile-action@v1
        with:
          target: ${{ matrix.target }}

  macos:
    strategy:
      matrix:
        target: [macos-intel, macos-universal]
    runs-on: macos-13
    steps:
      - uses: actions/checkout@v4
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
      - uses: actions/checkout@v4
        with:
          submodules: recursive
      - uses: distrho/dpf-makefile-action@v1
        with:
          target: ${{ matrix.target }}

  pluginval:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: recursive
      - uses: distrho/dpf-makefile-action@v1
        with:
          target: pluginval

  source:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: recursive
      - uses: distrho/dpf-makefile-action@v1
        with:
          target: source
```

## Documentation

The table below contains all possible properties for use with `distrho/dpf-makefile-action@v1` action.

| Property  | Required? | Description                                                |
| --------- | --------- | ---------------------------------------------------------- |
| target    | Yes       | The target platform and architecture to build for          |
| debug     | No        | Whether to build in debug mode, defaults to no             |
| lto       | No        | Whether to enable Link-Time-Optimizations, defaults to no  |
| dpf_path  | No        | Where DPF is located within your repo, defaults to "./dpf" |
| extraargs | No        | Extra arguments to pass into `make`                        |
| faust     | No        | Whether to install `faust`                                 |
| hvcc      | No        | Whether to install `hvcc`                                  |
| pawpaw    | No        | Whether to use [PawPaw](https://github.com/DISTRHO/PawPaw/) to install and setup extra libraries, defaults to no |
| release   | No        | Whether to automatically upload releases, defaults to yes  |
| suffix    | No        | Artifact and release filename suffix                       |

The table below contains all possible targets and supported runners.

| Target          | Aliases               | Allowed runners          |
| --------------- | --------------------- | ------------------------ |
| linux-arm64     |                       | ubuntu-20.04,22.04,24.04 |
| linux-armhf     |                       | ubuntu-20.04,22.04,24.04 |
| linux-i686      | linux-i386            | ubuntu-20.04,22.04,24.04 |
| linux-riscv64   |                       | ubuntu-20.04,22.04,24.04 |
| linux-x86_64    | linux                 | ubuntu-20.04,22.04,24.04 |
| macos-intel     |                       | macos-13,14           |
| macos-universal | macos                 | macos-13,14           |
| macos-10.15     | macos-universal-10.15 | macos-13,14           |
| win32           |                       | ubuntu-20.04,22.04,24.04 |
| win64           |                       | ubuntu-20.04,22.04,24.04 |
| pluginval       | plugin-validation     | ubuntu-20.04,22.04,24.04 |
| source          |                       | ubuntu-20.04,22.04,24.04 |

Notes:
 - ubuntu-24.04 runner has [broken 32bit support](https://bugs.launchpad.net/ubuntu/+source/linux-signed-azure/+bug/2071445), which breaks wine and i686 builds
 - macos-intel uses 10.8 as minimum version
 - macos-universal uses 10.12 as minimum version
 - Windows builds use Ubuntu runners in cross-compilation instead of Windows ones

## Changelog

### 1

Initial release

## Future Plans

- Set up MOD Audio builds
