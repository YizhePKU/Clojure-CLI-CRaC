# Clojure-CLI-CRaC

A drop-in Clojure-CLI replacement that gets you to the REPL fast.

Clojure is known for its slow start. This project utilizes Linux's [Checkpoint/Restore](https://criu.org) functionality to cache JVM state to disk after Clojure finishes initialization. Subsequent Clojure launches are much faster by restoring from the cached state.

Read more about the design process: [part 1](https://yizhepku.github.io/clojure-crac/) [part 2](https://yizhepku.github.io/clojure-crac-part2/).

## Installation

Clojure-CLI-CRaC requires `bash` and `rlwrap`. It can be used alongside the offical Clojure CLI.

1. Clone this repo

```bash
git clone https://github.com/YizhePKU/Clojure-CLI-CRaC.git
```

2. Download [the latest build of JDK with CRaC](https://github.com/CRaC/openjdk-builds/releases). Unpack with `sudo` (`sudo` is necessary because it contains a setuid executable for triggering Checkpoint/Restore)

```bash
wget https://github.com/CRaC/openjdk-builds/releases/download/17-crac%2B5/openjdk-17-crac+5_linux-x64.tar.gz
sudo tar xf openjdk-17-crac+5_linux-x64.tar.gz
```

3. Move the unpacked directory into this repo. Rename it to `openjdk-17-crac`

```bash
sudo mv openjdk-17-crac+5_linux-x64 Clojure-CLI-CRaC/openjdk-17-crac
```

4. (Optional) add the `bin/` directory to your PATH

## Usage

Clojure-CLI-CRaC is a drop-in Clojure CLI replacement. Run `clojure` and `clj` as you normally would.

* JVM options can only be set on the first launch (otherwise they have no effect)
* Will automatically invalidate the checkpoint when `deps.edn` changes (e.g. new dependencies)

## Limitations and known issues

* Currently Linux-x86_64 only
* Will print a bunch of nonsense when creating a checkpoint
* There's currently [an upstream bug](https://mail.openjdk.org/pipermail/crac-dev/2023-August/001391.html) that causes whitespace in command line arguments to be misinterpreted
