# Working with Parts

The `Part` class is the primary entry point for working with NX part files. It provides methods for opening, saving, querying, and accessing geometry within a part.

## Name Filtering

Most object query methods accept an optional name parameter. NXOpenKit uses wildcard pattern matching to filter objects by name.

### Wildcard Patterns

The `*` character matches any number of characters.

Examples:

| Pattern | Description |
|----------|-------------|
| `BLOCK*` | Matches names that start with `BLOCK`. |
| `*SURFACE` | Matches names that end with `SURFACE`. |
| `*HOLE*` | Matches names that contain `HOLE` anywhere. |

If no name is supplied, the method returns all matching objects. If a name pattern finds no objects, the method raises `ValueError`. For scripts that may contain no matches, catch this error or verify known object names before indexing.

### Examples

Get bodies whose names start with `BLOCK`:

```python
bodies = Part.get_bodies("BLOCK*")
```

Matching names include:

```text
BLOCK
BLOCK_1
BLOCK_MAIN
BLOCK_TEST
```

Get faces whose names end with `SURFACE`:

```python
faces = Part.get_faces("*SURFACE")
```

Matching names include:

```text
TOP_SURFACE
BOTTOM_SURFACE
INNER_SURFACE
```

Get features whose names contain `HOLE`:

```python
features = Part.get_features("*HOLE*")
```

Matching names include:

```text
HOLE
THREAD_HOLE
MOUNTING_HOLE_01
HOLE_PATTERN
```

Wildcard filtering is supported by these object query methods:

```python
Part.get_bodies("BLOCK*")
Part.get_faces("*SURFACE")
Part.get_edges("EDGE*")
Part.get_points("POINT*")
Part.get_curves("CURVE*")
Part.get_features("*HOLE*")
Part.get_datum_planes("DATUM*")
Part.get_datum_axes("AXIS*")
Part.get_datum_csys("CSYS*")
Part.get_displayable_objects("BLOCK*")
```

## Opening and Saving Parts

### Open a Part

```python
from nxopenkit import Part

part = Part.open_part(r"C:\Models\block.prt")
```

### Get the Current Work Part

```python
from nxopenkit import Part

part = Part.work_part()

print(part.name)
```

### Save the Current Part

```python
from nxopenkit import Part

Part.save()
```

### Save As

```python
from nxopenkit import Part

Part.save_as(r"C:\Models\block_copy.prt")
```

`save_as()` defaults to replacing an existing file with the same path. Pass `deleteExistingFile=False` to disable replacement.

### Close All Parts

```python
from nxopenkit import Part

Part.close_all()
```

## Working with Bodies

Bodies represent solid or sheet geometry in a part.

### Get All Bodies

```python
from nxopenkit import Part

bodies = Part.get_bodies()

for body in bodies:
    print(body.name)
```

### Get Bodies by Name

```python
from nxopenkit import Part

bodies = Part.get_bodies("BLOCK*")
```

Additional examples:

```python
Part.get_bodies("BLOCK*")
Part.get_bodies("*BODY")
```

## Working with Faces

Faces represent individual surfaces of a body.

### Get All Faces

```python
from nxopenkit import Part

faces = Part.get_faces()

print(len(faces))
```

### Get Faces by Name

```python
from nxopenkit import Part

faces = Part.get_faces("TOP*")
```

Additional examples:

```python
Part.get_faces("TOP*")
Part.get_faces("*SURFACE")
```

## Working with Edges

Edges represent boundaries between faces.

### Get All Edges

```python
from nxopenkit import Part

edges = Part.get_edges()

for edge in edges:
    print(edge.name)
```

### Get Edges by Name

```python
from nxopenkit import Part

edges = Part.get_edges("EDGE*")
```

Additional examples:

```python
Part.get_edges("EDGE*")
Part.get_edges("*OUTER")
```

## Working with Points

### Get All Points

```python
from nxopenkit import Part

points = Part.get_points()

print(len(points))
```

### Get Points by Name

```python
from nxopenkit import Part

points = Part.get_points("POINT*")
```

Examples:

```python
Part.get_points("POINT*")
Part.get_points("*ORIGIN")
```

## Working with Curves

### Get All Curves

```python
from nxopenkit import Part

curves = Part.get_curves()

for curve in curves:
    print(curve.name)
```

### Get Curves by Name

```python
from nxopenkit import Part

curves = Part.get_curves("CURVE*")
```

Examples:

```python
Part.get_curves("CURVE*")
Part.get_curves("*PLINE*")
```

## Working with Features

### Get All Features

```python
from nxopenkit import Part

features = Part.get_features()

for feature in features:
    print(feature.name)
```

### Get Features by Name

```python
from nxopenkit import Part

features = Part.get_features("BLOCK*")
```

Examples:

```python
Part.get_features("BLOCK*")
Part.get_features("*HOLE*")
```

## Working with Datum Planes

### Get All Datum Planes

```python
from nxopenkit import Part

planes = Part.get_datum_planes()

print(len(planes))
```

### Get Datum Planes by Name

```python
from nxopenkit import Part

planes = Part.get_datum_planes("DATUM*")
```

Examples:

```python
Part.get_datum_planes("DATUM*")
Part.get_datum_planes("*XY*")
```

## Working with Datum Axes

### Get All Datum Axes

```python
from nxopenkit import Part

axes = Part.get_datum_axes()

print(len(axes))
```

### Get Datum Axes by Name

```python
from nxopenkit import Part

axes = Part.get_datum_axes("AXIS*")
```

Examples:

```python
Part.get_datum_axes("AXIS*")
Part.get_datum_axes("*CENTER*")
```

## Working with Coordinate Systems

### Get All Coordinate Systems

```python
from nxopenkit import Part

csys = Part.get_datum_csys()

print(len(csys))
```

### Get Coordinate Systems by Name

```python
from nxopenkit import Part

csys = Part.get_datum_csys("CSYS*")
```

Examples:

```python
Part.get_datum_csys("CSYS*")
Part.get_datum_csys("*MACHINE*")
```

## Working with Displayable Objects

### Get All Displayable Objects

```python
from nxopenkit import Part

objects = Part.get_displayable_objects()

print(len(objects))
```

### Get Displayable Objects by Name

```python
from nxopenkit import Part

objects = Part.get_displayable_objects("BLOCK*")
```

Examples:

```python
Part.get_displayable_objects("BLOCK*")
Part.get_displayable_objects("*REFERENCE*")
```

## Example: Inspect a Part

```python
from nxopenkit import Part

part = Part.open_part(r"C:\Models\block.prt")

print(f"Bodies   : {len(Part.get_bodies())}")
print(f"Faces    : {len(Part.get_faces())}")
print(f"Edges    : {len(Part.get_edges())}")
print(f"Curves   : {len(Part.get_curves())}")
print(f"Features : {len(Part.get_features())}")
```

## Example: Inspect the Active Work Part

```python
from nxopenkit import Part

part = Part.work_part()

print(part.name)

print("Bodies   :", len(Part.get_bodies()))
print("Faces    :", len(Part.get_faces()))
print("Edges    :", len(Part.get_edges()))
print("Features :", len(Part.get_features()))
```

## Next Steps

After learning how to work with parts, continue with:

- Creating Geometry
- Feature Builders
- Selection Rules
- Measurements
- Attributes
- Examples
