## Steam Frame Dependency

This is the game-source dependency for my
[F-Zero X VR port](https://github.com/astromaddie/fzerox-vr).
The `vr/openxr-m0` branch adds port-gated stereo-rendering hooks and cockpit
integration. It lives in the parent checkout's `decomp/` directory.

For the complete Linux ARM64 application, use the parent checkout and its
[Steam Frame build and install guide](https://github.com/astromaddie/fzerox-vr/blob/vr/openxr-m0/docs/STEAM-FRAME-BUILD-INSTALL.md):

```sh
git clone --recurse-submodules --branch vr/openxr-m0 https://github.com/astromaddie/fzerox-vr.git
cd fzerox-vr
git submodule update --init --recursive
```

The parent records the exact dependency commits. Keep those revisions together;
don't use `git submodule update --remote` for a reproducible build. Its Docker
toolchain builds the ARM64 app and ROM-free package. Playing needs your own
supported game, Expansion Kit and IPL data, as described in the parent guide.

The upstream README follows.

---

[![Discord Channel][discord-badge]][discord]

[![US Bytes Progress]](https://decomp.dev/inspectredc/fzerox/us) [![US Functions Progress]](https://decomp.dev/inspectredc/fzerox/us)
[![Jp Bytes Progress]](https://decomp.dev/inspectredc/fzerox/jp) [![Jp Functions Progress]](https://decomp.dev/inspectredc/fzerox/jp)

[US Bytes Progress]: https://decomp.dev/inspectredc/fzerox/us.svg?mode=shield&label=US%20Bytes%20Progress&measure=matched_code_percent
[US Functions Progress]: https://decomp.dev/inspectredc/fzerox/us.svg?mode=shield&label=US%20Functions%20Progress&measure=matched_functions

[JP Bytes Progress]: https://decomp.dev/inspectredc/fzerox/jp.svg?mode=shield&label=JP%20Bytes%20Progress&measure=matched_code_percent
[JP Functions Progress]: https://decomp.dev/inspectredc/fzerox/jp.svg?mode=shield&label=JP%20Functions%20Progress&measure=matched_functions

# F-Zero X

This is a WIP Matching Decompilation for F-Zero X.

It currently builds the following ROMs:

* fzerox.us.rev0.z64 `MD5: 753437d0d8ada1d12f3f9cf0f0a5171f`
* fzerox.jp.rev0.z64 `MD5: 58d200d43620007314304f4e6c9e6528`

**This repo does not include any assets or assembly code necessary for compiling the ROM. A prior copy of the game is required to extract the required assets.**

## Installation

#### 1. Install build dependencies

### Windows

For Windows 10, install WSL and a distribution by following this
[Windows Subsystem for Linux Installation Guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
We recommend using Debian or Ubuntu 22.04 Linux distributions.

### Linux (Native or under WSL / VM)

The build process has the following package requirements:

* make
* git
* build-essential
* binutils-mips-linux-gnu
* python3
* pip3
* libpng-dev

Under Debian / Ubuntu (which we recommend using), you can install them with the following commands:

```bash
sudo apt update
sudo apt install make cmake git build-essential binutils-mips-linux-gnu python3 python3-pip clang-format-14 clang-tidy
```

### MacOS

Install [Homebrew](https://brew.sh) and the following dependencies:
```
brew update
brew install coreutils make pkg-config tehzz/n64-dev/mips64-elf-binutils
```

#### 2. Clone the repository

Create your own fork of the repository at `https://github.com/inspectredc/fzerox`. Then clone your fork where you wish to have the project, with the command:

```bash
git clone https://github.com/<YOUR_USERNAME>/fzerox.git
```

This will copy the GitHub repository contents into a new folder in the current directory called `fzerox`. Change into this directory before doing anything else:

```bash
cd fzerox
```

#### 3. Install python dependencies

The build process has a few python packages required that are located in `/tools/requirements-python.txt`.

To install them simply run in a terminal:

```bash
python3 -m pip install -r ./tools/requirements-python.txt
```
* Depending on your python version, you might need to add  --break-system-packages, or use venv.

#### 4. Update submodules & build toolchain

```bash
git submodule update --init --recursive
make toolchain
```

#### 5. Prepare a base ROM

Copy your ROM to the root of this new project directory, and rename the file of the baserom to reflect the version of ROM you are using. ex: `baserom.us.rev0.z64`
* Make sure the ROM is the US or JP version, revision 1.0

#### 6. Make and Build the ROM

To start the extraction/build process, run the following command:

```bash
make init
```
This will create the build folders, a new folder with the assembly as well as containing the disassembly of nearly all the files containing code.

this make target will also build the ROM. If all goes well, a new ROM called "fzerox.us.rev0.z64" should be built and the following text should be printed:

```bash
Calculating Rom Header Checksum... build/fzerox.us.rev0.z64
fzerox.us.rev0.z64: OK
```

If you instead see the following:

```bash
Calculating Rom Header Checksum... build/fzerox.us.rev0.z64
fzerox.us.rev0.z64 FAILED
```

This means that something is wrong with the ROM's contents. Either the base files are incorrect due to a bad ROM, or some of the code is not matching.

From now on you should be able to build the rom by running `make`.

For building the japanese version, make commands must all contain this additional define `VERSION=jp`.

[discord]: https://discord.gg/f2gmBaHySA
[discord-badge]: https://img.shields.io/discord/1252984267471061053?logo=discord&logoColor=ffffff
