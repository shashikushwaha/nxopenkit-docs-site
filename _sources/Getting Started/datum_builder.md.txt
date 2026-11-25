# Datum Builders

Before running these examples, open or create an NX part and make sure it is the active work part. Reference objects such as faces, curves, and points must exist in that part.

Datum builders create reference geometry used to place features and establish a model coordinate system.

NXOpenKit provides three datum builder types:

- `DatumPlaneBuilder`
- `DatumAxisBuilder`
- `DatumCsysBuilder`

```python
from nxopenkit import (
    DatumPlaneBuilder,
    DatumAxisBuilder,
    DatumCsysBuilder,
)
```

## Datum Plane Builder

`DatumPlaneBuilder` creates a datum plane using one of several NX plane construction methods.

### Fixed Principal Planes

Create datum planes aligned with the principal coordinate planes:

```python
xy_plane = DatumPlaneBuilder.create_along_xy_plane()
yz_plane = DatumPlaneBuilder.create_along_yz_plane()
zx_plane = DatumPlaneBuilder.create_along_zx_plane()

xy_feature = xy_plane.commit_feature()
```

### Plane through an Object

Create a plane coincident with a face or another planar object:

```python
from nxopenkit import Part

face = Part.get_faces()[0]
plane = DatumPlaneBuilder.create_by_through_object(face)
feature = plane.commit_feature()
```

### Plane at a Distance

Create a plane offset from geometry:

```python
plane = DatumPlaneBuilder.create_at_distance(
    geometry=face,
    number_of_plane=25,
)
feature = plane.commit_feature()
```

### Plane at an Angle

Create a plane at an angle from a sequence of reference objects:

```python
plane = DatumPlaneBuilder.create_at_angle(
    geometry=[face, Part.get_edges()[0]],
    angle=45,
)
feature = plane.commit_feature()
```

### Plane from Points

Create a plane from one, two, or three points:

```python
point1 = Point.create_from_coordinates(0, 0, 0)
point2 = Point.create_from_coordinates(100, 0, 0)
point3 = Point.create_from_coordinates(0, 100, 0)

plane = DatumPlaneBuilder.create_by_three_points(
    point1,
    point2,
    point3,
)
feature = plane.commit_feature()
```

### Plane from a Point and Direction

```python
from nxopenkit import Direction, Part, Point, Point3d, Vector3d

point = Point.create_from_coordinates(0, 0, 0)
direction = Direction.create_from_vector(
    Point3d(0, 0, 0),
    Vector3d(0, 0, 1),
)

plane = DatumPlaneBuilder.create_by_point_direction(
    point,
    direction,
)
feature = plane.commit_feature()
```

### Additional Plane Methods

The class also provides helpers for:

- Bisecting geometry
- Curves and points
- Tangent faces or lines
- Tangent angle relative to a plane
- Curves using Frenet orientation
- Two lines
- A fixed plane along a custom origin and normal vector

For example:

```python
plane = DatumPlaneBuilder.create_along_vector(
    origin=Point3d(10, 10, 10),
    vector=Vector3d(0, 0, 1),
)
feature = plane.commit_feature()
```

### Plane Configuration

Use `plane_orientation()` to reverse or alternate the plane:

```python
plane.plane_orientation(
    reverse_direction=True,
)
```

Use `offset()` to set an additional offset:

```python
plane.offset(
    distance=10,
    reverse_offset=False,
)
```

Use `set_plane_corner()` when configuring the four visible corner points of a plane.

## Datum Axis Builder

`DatumAxisBuilder` creates reference axes from points, directions, curves, faces, or intersections.

### Axis Along a Principal Direction

```python
axis_x = DatumAxisBuilder.create_along_x_direction()
axis_y = DatumAxisBuilder.create_along_y_direction()
axis_z = DatumAxisBuilder.create_along_z_direction()

axis_feature = axis_z.commit_feature()
```

### Axis from a Point and Direction

```python
point = Point.create_from_coordinates(0, 0, 0)
direction = Direction.create_from_vector(
    Point3d(0, 0, 0),
    Vector3d(0, 0, 1),
)

axis = DatumAxisBuilder.create_by_point_direction(
    point,
    direction,
)
feature = axis.commit_feature()
```

### Axis from a Curve or Face

```python
curve = Part.get_curves()[0]
axis = DatumAxisBuilder.create_on_curve_or_face_axis(curve)
feature = axis.commit_feature()
```

### Axis from Two Intersecting Objects

```python
plane1 = Part.get_datum_planes()[0]
plane2 = Part.get_datum_planes()[1]

axis = DatumAxisBuilder.create_by_intersection(plane1, plane2)
feature = axis.commit_feature()
```

`create_on_curves_vector()` creates an axis based on position along a curve or connected curves:

```python
from nxopenkit import DatumAxisBuilder, Part

edge = Part.get_edges()[0]
axis = DatumAxisBuilder.create_on_curves_vector(
    curves=edge,
    arc_length=50,
    percent=True,
)
feature = axis.commit_feature()
```

When `percent=True`, `arc_length` is a percentage from the curve start. Set it to `False` to use an absolute arc length.

### Axis Configuration

Use `axis_orientation()` to reverse the axis direction:

```python
axis.axis_orientation(reverse_direction=True)
```

Use `settings()` to control associativity and `resize()` to adjust the displayed end distance.

## Datum Coordinate System Builder

`DatumCsysBuilder` creates a datum coordinate system using a transform.

### From X and Y Vectors

```python
from nxopenkit import DatumCsysBuilder, Point3d, Vector3d

csys = DatumCsysBuilder.create_by_xy_vector(
    origin=Point3d(0, 0, 0),
    x_vector=Vector3d(1, 0, 0),
    y_vector=Vector3d(0, 1, 0),
)

feature = csys.commit_feature()
```

### From X and Z Directions

```python
from nxopenkit import DatumCsysBuilder, Direction, Point, Point3d, Vector3d

origin = Point.create_from_coordinates(0, 0, 0)
x_direction = Direction.create_from_vector(Point3d(0, 0, 0), Vector3d(1, 0, 0))
z_direction = Direction.create_from_vector(Point3d(0, 0, 0), Vector3d(0, 0, 1))

csys = DatumCsysBuilder.create_by_xz_dir(
    origin,
    x_direction,
    z_direction,
)
feature = csys.commit_feature()
```

Other factory methods include:

- `create_by_yz_dir()`
- `create_by_plane_y_dir()`
- `create_by_plane_x_dir()`
- `create_by_three_planes()`

### Coordinate-System Settings

```python
csys.settings(
    is_associative=True,
    scale_factor=1.25,
)
feature = csys.commit_feature()
```

## Accessing an NXOpen Builder

Every datum builder exposes `nx_builder`:

```python
nx_plane_builder = plane.nx_builder
nx_axis_builder = axis.nx_builder
nx_csys_builder = csys.nx_builder
```

## Next Steps

- [Point Builder](point_builder.md)
- [Creating Geometry](creating_geometry.md)
- [Feature Builders Overview](feature_builders_overview.md)
