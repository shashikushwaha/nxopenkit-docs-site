# Installation

## Requirements

NXOpenKit requires:

- Python 3.8 or later
- A Siemens NX version that uses Python 3.8
- A valid Siemens NX installation

## Configure the UGII_BASE_DIR Environment Variable

Before installing NXOpenKit, set the `UGII_BASE_DIR` environment variable to your Siemens NX installation directory.

### Windows

```cmd
set UGII_BASE_DIR=C:\Program Files\Siemens\NXXXXX
```

### PowerShell

```powershell
$env:UGII_BASE_DIR="C:\Program Files\Siemens\NXXXXX"
```

Replace the path with the location of your NX installation.

## Install NXOpenKit

Activate the Python environment associated with your NX installation and install the package:

```powershell
pip install nxopenkit
```

## Verify the Installation

Run the following script from an NX-compatible Python environment:

```python
from nxopenkit import Point3d

point = Point3d(1.0, 2.0, 3.0)

print(point)
```

Expected output:

```text
Point3d(1, 2, 3)
```

## Troubleshooting

### NXOpen Module Cannot Be Imported

If Python cannot import the `NXOpen` module:

1. Verify that the `UGII_BASE_DIR` environment variable points to the correct NX installation.
2. Ensure you are using the Python version supplied with, or compatible with, your Siemens NX installation.
3. Confirm that the NX Open libraries are available in your Python environment.

For more information, refer to your Siemens NX installation documentation.