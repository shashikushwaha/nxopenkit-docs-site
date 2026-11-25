# NXOpenKit

This repository contains documentation for NXOpenKit.

The NXOpenKit source code is maintained separately and is not included in this repository.

**A Python toolkit for working with Siemens NX through the NX Open API.**

NXOpenKit provides lightweight Python wrappers for common NX objects and feature-builder workflows. It does not replace NX Open or Siemens NX; instead, it makes tasks such as querying parts and creating geometry easier to read and reuse.

## Highlights

- Python wrapper objects for parts, bodies, faces, edges, points, curves, and datum geometry
- Simplified block, cylinder, sphere, extrude, revolve, Boolean, datum, and point builders
- Fluent feature-builder configuration and `commit_feature()` workflows
- Direct access to native NXOpen objects through `to_nx`
- Direct access to the underlying feature builder through `nx_builder`
- Support for interactive NX scripting and standalone batch processing

## Why NXOpenKit?

NX Open may require several low-level setup steps for basic modeling operations. NXOpenKit keeps the NX workflow familiar while reducing wrapper and builder boilerplate.

```python
from nxopenkit import BlockBuilder, Point

origin = Point.create_from_coordinates(0, 0, 0)

block_feature = BlockBuilder.create_by_origin_lengths(
    origin,
    xc_length=100,
    yc_length=60,
    zc_length=40,
).commit_feature()
```

For advanced NX functionality, access the underlying builder or committed native object:

```python
block = BlockBuilder.create_by_origin_lengths(
    origin,
    xc_length=100,
    yc_length=60,
    zc_length=40,
)

nx_builder = block.nx_builder

feature = block.commit_feature()
nx_feature = feature.to_nx
```

## Requirements

- Python 3.8 or a Python version compatible with the installed NX release
- A licensed Siemens NX installation
- NX Open Python support
- The `UGII_BASE_DIR` environment variable set to the Siemens NX installation directory

`UGII_BASE_DIR` is typically the NX base directory, for example `C:\Program Files\Siemens\NX2007`.

```powershell
$env:UGII_BASE_DIR="C:\Program Files\Siemens\NX2007"
```

## Installation

Create a Python environment compatible with your Siemens NX installation, activate it, and install the package:

```powershell
conda create -n nxopenkit python=3.8
conda activate nxopenkit
pip install nxopenkit
```

The package must be run with NX Open libraries available. For interactive NX setup, follow the [Quick Start guide](docs/Getting%20Started/quickstart.md).

## Quick Start

### Batch Mode: Open a Part

```python
from nxopenkit import Part

part = Part.open_part(r"C:\Models\sample.prt")

print(part.name)

for body in Part.get_bodies():
    print(body.name)

Part.save_as(r"C:\Models\sample_output.prt")
Part.close_all()
```

### Interactive Mode: Query the Active Work Part

```python
from nxopenkit import Part

part = Part.work_part()

print(part.name)
print("Bodies:", len(Part.get_bodies()))
print("Faces:", len(Part.get_faces()))
print("Features:", len(Part.get_features()))
```

---

## Documentation

The documentation is included in the repository under `docs/`.

- [Installation](docs/Getting%20Started/installation.md)
- [Quick Start](docs/Getting%20Started/quickstart.md)
- [Working with Parts](docs/Getting%20Started/working_with_parts.md)
- [Creating Geometry](docs/Getting%20Started/creating_geometry.md)
- [Feature Builders Overview](docs/Getting%20Started/feature_builders_overview.md)
- [Block Builder](docs/Getting%20Started/block_builder.md)
- [Cylinder Builder](docs/Getting%20Started/cylinder_builder.md)
- [Sphere Builder](docs/Getting%20Started/sphere_builder.md)
- [Extrude Builder](docs/Getting%20Started/extrude_builder.md)
- [Revolve Builder](docs/Getting%20Started/revolve_builder.md)
- [Boolean Builder](docs/Getting%20Started/boolean_builder.md)
- [Datum Builders](docs/Getting%20Started/datum_builder.md)
- [Point Builder](docs/Getting%20Started/point_builder.md)

## Public API Examples

### Create a Point

```python
from nxopenkit import Point

point = Point.create_from_coordinates(100, 50, 25)
nx_point = point.to_nx
```

### Create a Datum Axis

```python
from nxopenkit import DatumAxisBuilder

axis = DatumAxisBuilder.create_along_z_direction()
feature = axis.commit_feature()
```

### Create an Extrusion

```python
from nxopenkit import ExtrudeBuilder, Part, Vector3d

profiles = Part.get_curves("PROFILE*")

feature = (
    ExtrudeBuilder.create_extrude(
        profiles,
        start_distance=0,
        end_distance=50,
        direction=Vector3d(0, 0, 1),
    )
    .commit_feature()
)
```

## Project Status

NXOpenKit is under active development. Feature coverage and public APIs may change while the package moves toward a stable release.

## Contributing

Contributions are welcome. Please include tests or documentation updates when changing public behavior.

Before submitting changes:

```powershell
pytest
```

Build the documentation from the `docs` directory:

```powershell
python -m sphinx -b html . _build/html
```

---

## Disclaimer

NXOpenKit is an independent open-source project and is not affiliated with, endorsed by, or maintained by Siemens Digital Industries Software.

Siemens NX and NX Open are trademarks of Siemens Digital Industries Software or their respective owners.

---

## Vision

NXOpenKit aims to become the easiest and most productive way to automate Siemens NX with Python.

Our mission is simple:

> Focus on engineering problems, not NXOpen boilerplate.

---

## License

NXOpenKit is licensed under the MIT License. See the LICENSE file included with the package distribution for details.

---

# Write less NXOpen. Build more with NXOpenKit.
