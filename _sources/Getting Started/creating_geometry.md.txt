# Creating Geometry

NXOpenKit provides a set of geometry classes that simplify the creation and manipulation of NX geometry.

These objects are commonly used when creating features, datums, coordinate systems, and other modeling operations.

## Working with NXOpen Objects

NXOpenKit geometry classes are lightweight wrappers around NXOpen objects. Every wrapper exposes a `to_nx` property that returns the underlying NXOpen object.

This allows NXOpenKit and NXOpen APIs to be used together in the same script.

### Example: Point

```python
from nxopenkit import Point

point = Point.create_from_coordinates(10, 20, 30)

nx_point = point.to_nx
```

### Example: Curve

```python
from nxopenkit import Part

curve = Part.get_curves()[0]

nx_curve = curve.to_nx
```

### Example: Face

```python
from nxopenkit import Part

face = Part.get_faces()[0]

nx_face = face.to_nx
```

### Example: NXOpen Interoperability

```python
from nxopenkit import Part

body = Part.get_bodies()[0]

nx_body = body.to_nx

print(nx_body.JournalIdentifier)
```

Use the `to_nx` property whenever an NXOpen API requires a native NXOpen object.

## Point3d

`Point3d` represents a three-dimensional coordinate.

### Create a Point3d

```python
from nxopenkit import Point3d

point = Point3d(
    x=100,
    y=50,
    z=25,
)
```

### Create a Point3d Using Positional Arguments

```python
from nxopenkit import Point3d

point = Point3d(100, 50, 25)
```

### Access Coordinates

```python
from nxopenkit import Point3d

point = Point3d(100, 50, 25)

print(point.x)
print(point.y)
print(point.z)
```

## Point

Points are visible NX objects that exist within a part.

### Create a Point

```python
from nxopenkit import Point

point = Point.create_from_coordinates(
    100,
    50,
    25,
)
```

### Create Multiple Points

```python
from nxopenkit import Point

point1 = Point.create_from_coordinates(0, 0, 0)

point2 = Point.create_from_coordinates(100, 0, 0)

point3 = Point.create_from_coordinates(100, 50, 0)
```

### Access the Underlying NXOpen Point

```python
from nxopenkit import Point

point = Point.create_from_coordinates(100, 50, 25)

nx_point = point.to_nx
```

## Vector3d

`Vector3d` represents a direction and magnitude in three-dimensional space.

### Create a Vector

```python
from nxopenkit import Vector3d

vector = Vector3d(
    x=1,
    y=0,
    z=0,
)
```

### Create a Z Direction Vector

```python
from nxopenkit import Vector3d

vector = Vector3d(
    x=0,
    y=0,
    z=1,
)
```

## Direction

Directions are frequently used when creating features, datums, and transforms.

### Create a Direction from a Vector

```python
from nxopenkit import Direction, Point3d, Vector3d

origin = Point3d(0, 0, 0)
vector = Vector3d(1, 0, 0)

direction = Direction.create_from_vector(origin, vector)
```

### Create a Direction from a Curve Parameter

```python
from nxopenkit import Direction, Part

curve = Part.get_curves()[0]

direction = Direction.create_from_curve_parameter(
    curve,
    parameter=0.5,
)
```

### Create a Direction from a Curve Point

```python
from nxopenkit import Direction, Part, Point

curve = Part.get_curves()[0]
point = Point.create_from_coordinates(0, 0, 0)

direction = Direction.create_from_curve_point(
    curve,
    point,
)
```

### Create a Direction from an Edge

```python
from nxopenkit import Direction, Part

edge = Part.get_edges()[0]

direction = Direction.create_from_edge(edge)
```

### Create a Direction from a Datum Axis

```python
from nxopenkit import Direction, Part

axis = Part.get_datum_axes()[0]

direction = Direction.create_from_axis(axis)
```

When an NXOpen object or a NXOpenKit wrapper is not supported directly by a convenience method, pass its wrapper or `.to_nx` value according to the method's type requirement.

## Coordinate Systems

Coordinate systems define orientation and positioning within the model.

### Create a Coordinate System from X and Y Vectors

```python
from nxopenkit import CoordinateSystem, Point3d, Vector3d

origin = Point3d(0, 0, 0)
x_vector = Vector3d(1, 0, 0)
y_vector = Vector3d(0, 1, 0)

coordinate_system = CoordinateSystem.create_by_xy_vector(
    origin,
    x_vector,
    y_vector,
)
```

### Create a Coordinate System from X and Z Directions

```python
from nxopenkit import CoordinateSystem, Direction, Point, Point3d, Vector3d

origin = Point.create_from_coordinates(0, 0, 0)
x_direction = Direction.create_from_vector(Point3d(0, 0, 0), Vector3d(1, 0, 0))
z_direction = Direction.create_from_vector(Point3d(0, 0, 0), Vector3d(0, 0, 1))

coordinate_system = CoordinateSystem.create_by_xz_dir(
    origin,
    x_direction,
    z_direction,
)
```

## Datum Geometry

Datum geometry is commonly used when building features.

### Get Datum Planes

```python
from nxopenkit import Part

planes = Part.get_datum_planes()

for plane in planes:
    print(plane.name)
```

### Get Datum Axes

```python
from nxopenkit import Part

axes = Part.get_datum_axes()

for axis in axes:
    print(axis.name)
```

### Get Coordinate Systems

```python
from nxopenkit import Part

csys = Part.get_datum_csys()

for system in csys:
    print(system.name)
```

## Geometry Query Example

```python
from nxopenkit import Part

points = Part.get_points()

curves = Part.get_curves()

edges = Part.get_edges()

print(f"Points : {len(points)}")
print(f"Curves : {len(curves)}")
print(f"Edges  : {len(edges)}")
```

## Geometry and NXOpen

NXOpenKit does not replace NXOpen. Instead, it simplifies common tasks while still allowing direct access to the NXOpen API through the `to_nx` property.

```python
from nxopenkit import Part

face = Part.get_faces()[0]

nx_face = face.to_nx

print(nx_face.JournalIdentifier)
```

This makes it easy to use NXOpenKit for day-to-day automation while still accessing advanced NXOpen functionality when needed.

## Next Steps

After learning the geometry classes, continue with:

- [Feature Builders Overview](feature_builders_overview.md)
- [BlockBuilder](block_builder.md)
- [CylinderBuilder](cylinder_builder.md)
- [SphereBuilder](sphere_builder.md)
- [ExtrudeBuilder](extrude_builder.md)
- [RevolveBuilder](revolve_builder.md)
- [BooleanBuilder](boolean_builder.md)
- [Datum Builders](datum_builder.md)
- [PointBuilder](point_builder.md)

## Beginner Terms

- **Part**: The NX part file containing the model.
- **Point**: A visible NX point object in a part.
- **Point3d**: A lightweight X/Y/Z coordinate value; it is not automatically a visible point.
- **Vector3d**: A three-dimensional direction and magnitude represented by X, Y, and Z components.
- **Body**: Solid or sheet geometry in a part.
- **Feature**: A modeling operation such as a block, cylinder, extrude, or revolve.
- **Builder**: A configurable NX operation that is committed to create a feature.
- **Wrapper**: An NXOpenKit object that provides Python-friendly access to a native NXOpen object.
- **`to_nx`**: Exposes the native NXOpen object behind an NXOpenKit wrapper.
- **`commit_feature()`**: Finishes a feature builder and creates the corresponding NX feature.