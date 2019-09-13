[![Build Status](https://travis-ci.org/Moguri/panda3d-gltf.svg?branch=master)](https://travis-ci.org/Moguri/panda3d-gltf)
[![](https://img.shields.io/pypi/pyversions/panda3d_gltf.svg)](https://pypi.org/project/panda3d-gltf/)
[![Panda3D Versions](https://img.shields.io/badge/panda3d-1.10%2C%201.11-blue.svg)](https://www.panda3d.org/)
[![](https://img.shields.io/github/license/Moguri/panda3d-gltf.svg)](https://choosealicense.com/licenses/bsd-3-clause/)

# panda3d-gltf
This project adds glTF loading capabilities to Panda3D.
One long-term goal for this project is to be used as a reference for adding a builtin, C++ glTF loader to Panda3D.
If and when Panda3D gets builtin support for glTF, this module will go into maintenance mode and be used to backport glTF support to older versions of Panda3D.

## Features
* Adds support for native loading of glTF files
* Supports glTF 2.0 excluding morph targets
* Supports binary glTF
* Includes support for the following extensions:
  * KHR_lights (deprecated in favor of KHR_lights_punctual)
  * KHR_lights_punctual
  * BLENDER_physics
* Ships with a `gltf2bam` cli-tool for converting glTF files to BAM
* Ships with `gltf-viewer` for viewing files (including glTF) with a simple PBR renderer

## Installation

Use pip to install the `panda3d-gltf` package:

```bash
pip install panda3d-gltf
```

To grab the latest development build, use:

```bash
pip install git+https://github.com/Moguri/panda3d-gltf.git

```

## Usage

### Native loading

`panda3d-gltf` ships with a Python file loader (requires Panda3D 1.10.4+), which seamlessly adds glTF support to Panda3D's `Loader` classes.
This *does not* add support to `pview`, which is a C++ application that does not support loading Python file loaders.
Instead of `pview`, use the `gltf-viewer` that ships with `panda3d-gltf`.
For those that need to support Panda3D 1.10.3 or lower, `panda3d-gltf` also supplies a `patch_loader()` function  to monkey-patch glTF support to `ShowBase.loader`:

```python
import gltf

class App(ShowBase):
    def __init__(self):
        ...
        gltf.patch_loader(self.loader)
        ...
```

On Panda3D 1.10.4+, this function will leave `self.loader` alone in favor of relying on the Python file loader.

### Command Line

To convert glTF files to BAM via the command line, use the supplied `gltf2bam` tool:

```bash
gltf2bam source.gltf output.bam
```

### Viewer

`panda3d-gltf` ships with `gltf-viewer`.
This is a simple viewer (like `pview`) to view glTF (or any other file format support by Panda3D) with a simple, PBR renderer.

## API Stability

Since `panda3d-gltf` has not reached a 1.0 release, its API should not be considered "stable."
However, this mostly applies to internals, and effort will be put into keeping the `gltf2bam` API from breaking.
`patch_loader()` will also be kept stable, but will eventually be phased out in favor of the Python file loader.

## Running tests
```bash
python setup.py test
```

## License
[B3D 3-Clause](https://choosealicense.com/licenses/bsd-3-clause/)
