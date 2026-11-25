# Quick Start

NXOpenKit supports both interactive execution inside Siemens NX and standalone batch execution.

---
## Batch Mode

Batch Mode enables NXOpenKit scripts to run without an active Siemens NX session. Create a Python 3.8 environment, install NXOpenKit, and process NX part files directly from Python.

### Create a Python 3.8 Environment

Open a terminal in Visual Studio Code and create a dedicated Conda environment:

```powershell
conda create -n nxopenkit python=3.8
```

Activate the environment:

```powershell
conda activate nxopenkit
```

Verify the Python version:

```powershell
python --version
```

Expected output:

```text
Python 3.8.x
```

### Install NXOpenKit

```powershell
pip install nxopenkit
```

Verify the installation:

```powershell
pip show nxopenkit
```

### Open an Existing Part

```python
from nxopenkit import Part

part = Part.open_part(r"C:\Models\sample.prt")

print(part.name)
```

### Process Bodies

```python
from nxopenkit import Part

part = Part.open_part(r"C:\Models\sample.prt")

for body in Part.get_bodies():
    print(body.name)
```

### Save Changes

```python
from nxopenkit import Part

Part.save()
```

### Close the Part

```python
from nxopenkit import Part

Part.close_all()
```

---

## Interactive Mode

Interactive Mode allows NXOpenKit scripts to run directly inside the Siemens NX user interface. Before launching NX, you must configure NX to use the same Python environment where NXOpenKit is installed.

### Prerequisites

Create a Python 3.8 environment and install NXOpenKit.

#### Conda

```bash
conda create -n test-nxopenkit python=3.8
conda activate test-nxopenkit

pip install nxopenkit
```

### Configure NX Python Environment

Create a batch file (for example, `start_nxopenkit.bat`) and update the paths to match your environment.

```bat
@echo off

set UGII_PYTHON_DLL=python38.dll

set UGII_PYTHON_HOME=C:\path\to\your\conda\envs\nxopenkit-env

set UGII_PYTHON_LIBRARY_DIR=C:\path\to\your\conda\envs\nxopenkit-env

set UGII_PYTHONPATH=%UGII_BASE_DIR%\NXBIN\python;C:\path\to\your\conda\envs\nxopenkit-env;C:\path\to\your\conda\envs\nxopenkit-env\DLLs;C:\path\to\your\conda\envs\nxopenkit-env\Lib;C:\path\to\your\conda\envs\nxopenkit-env\Lib\site-packages;C:\path\to\your\conda\envs\nxopenkit-env\libs;%UGII_PYTHONPATH%

call "%UGII_BASE_DIR%\UGII\ugraf.exe"
```

Launch NX using this batch file instead of starting NX directly.

### Verify the Installation

Create a new journal and run:

```python
from nxopenkit import Part

print("NXOpenKit loaded successfully")
```

If the import succeeds, NX is using the configured Python environment correctly.

### Running a Script

Example:

```python
from nxopenkit import Part, BlockBuilder, Point

point1 = Point.create_from_coordinates(10, 50, 40)
block_feat = BlockBuilder.create_by_origin_lengths(
    point1, 
    10, 
    60, 
    40
).commit_feature()
```

## Known Limitation

> NX Journal execution creates a new Python interpreter each time a journal is executed.
>
> Because NXOpenKit uses compiled extension modules, re-running a journal multiple times within the same NX session may result in module-loading issues.
>
> If this occurs, restart NX and execute the journal again.
>
> Batch Mode is not affected by this limitation and remains the recommended workflow for development and testing.

### Recommended Workflow

For reliable development:

1. Develop and test automation scripts in Batch Mode.
2. Use Interactive Mode for UI-driven workflows and quick validation inside NX.
3. Restart NX before re-running modified journals if module-loading issues occur.

## Troubleshooting

### NXOpenKit cannot be imported

Verify that:

- The Python environment contains NXOpenKit.
- `UGII_PYTHON_HOME` points to the correct environment.
- `UGII_PYTHONPATH` includes `Lib` and `Lib\site-packages`.
- NX is started using the provided batch file.

### Wrong Python Environment

Run the following journal:

```python
import sys

print(sys.executable)
```

The output should point to the configured Python environment.

### Module Import Errors After Re-running Journals

This is a known limitation of the current Interactive Mode implementation. Restart NX and execute the journal again.


### Get the Current Work Part

```python
from nxopenkit import Part

part = Part.work_part()

print(part.name)
```

### List All Bodies

```python
from nxopenkit import Part

for body in Part.get_bodies():
    print(body.name)
```

### Inspect a Part

```python
from nxopenkit import Part

part = Part.work_part()

print("Part:", part.name)
print("Bodies:", len(Part.get_bodies()))
print("Faces:", len(Part.get_faces()))
print("Features:", len(Part.get_features()))
```

---


## Choosing a Mode

| Mode | Use case | Recommendation |
|---|---|---|
| Interactive | Work with the active NX session through journals or UI-driven tools | Use for quick checks and NX-integrated automation |
| Batch | Open and process existing part files from a standalone Python process | Recommended for development, testing, and unattended automation |

For most beginners, start with Interactive Mode while learning NXOpenKit, and use Batch Mode for repeatable scripts once the workflow is stable.

---

## Next Steps

- [Working with Parts](working_with_parts.md)
- [Creating Geometry](creating_geometry.md)
- [Feature Builders Overview](feature_builders_overview.md)
- [Block Builder](block_builder.md)