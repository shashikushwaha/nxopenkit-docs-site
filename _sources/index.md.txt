# NXOpenKit

**A modern Python toolkit for Siemens NX Open development.**

NXOpenKit simplifies Siemens NX automation by providing a clean, type-safe, and developer-friendly API on top of NX Open. It reduces boilerplate code, improves readability, and enhances productivity when developing NX automation solutions.

---

## Features

- 🚀 Simplified NX Open workflows
- 📝 Full type hints and IDE autocomplete support
- ⚡ Faster automation development
- 🏗 Modular architecture
- 📦 Easy package installation
- 🔍 Improved code readability and maintainability

---

## Key Benefits

- Write less code
- Improve script readability
- Better IDE support with type hints
- Easier onboarding for new developers
- Consistent API patterns across NX objects

---

## Why NXOpenKit?

Developing directly with NX Open often involves repetitive code and navigating complex object hierarchies. NXOpenKit provides a more intuitive interface while preserving the flexibility of the native NX Open API.

### Example

#### Native NX Open

```python
import math
import NXOpen
import NXOpen.Features
import NXOpen.GeometricUtilities
    
theSession  = NXOpen.Session.GetSession()
results = theSession.Parts.OpenActiveDisplay(filePath, NXOpen.DisplayPartOption.AllowAdditional)
workPart = theSession.Parts.Work
bodiesCol = Session.work_part().Bodies
for body in bodiesCol:
    print(body.Name)

```

#### NXOpenKit

```python
from nxopenkit import Part

part1 = Part.open_part(file_path)
for body in Part.get_bodies():
    print(body.name)
```

---

## Installation

```bash
pip install nxopenkit
```

---

## Quick Start

```python
from nxopenkit import Part

part = Part.work_part()
print(part.name)
```

---

## Documentation


```{toctree}
:maxdepth: 2
:caption: API Reference

doxygen-api
```

```{toctree}
:maxdepth: 2
:caption: Getting Started

Getting Started/installation
Getting Started/quickstart
Getting Started/working_with_parts
Getting Started/creating_geometry
Getting Started/feature_builders_overview
Getting Started/block_builder
Getting Started/cylinder_builder
Getting Started/sphere_builder
Getting Started/extrude_builder
Getting Started/revolve_builder
Getting Started/boolean_builder
Getting Started/datum_builder
Getting Started/point_builder
```

```{toctree}
:maxdepth: 1
:caption: Project Information

README
LICENSE
```

---

## Support

If you encounter any issues or have suggestions for improvements, please create an issue in the project repository.

---

## License

NXOpenKit is distributed under the MIT License. See the LICENSE file for details.