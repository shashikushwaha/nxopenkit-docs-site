# Point Builder

Before running these examples, open or create an NX part and make sure it is the active work part.

`PointBuilder` creates associative NX point features from coordinates or other geometry.

```python
from nxopenkit import PointBuilder
```

## Create from Coordinates

```python
point = PointBuilder.create_from_coordinates(
    x_cor=100,
    y_cor=50,
    z_cor=25,
)

feature = point.commit_feature()
```

## Create from a Point3d

```python
from nxopenkit import Point3d

coordinate = Point3d(100, 50, 25)

point = PointBuilder.create_from_point3d(coordinate)
feature = point.commit_feature()
```

## Create from an Array

```python
point = PointBuilder.create_from_array([100, 50, 25])
feature = point.commit_feature()
```

The sequence must contain exactly three numeric values.

## Create on a Curve

Use `create_on_curve()` to place a point along a curve or connected curves:

```python
from nxopenkit import Part

curve = Part.get_curves()[0]

point = PointBuilder.create_on_curve(
    curves_edges=curve,
    percentage=50,
)
feature = point.commit_feature()
```

The default location option is percentage arc length. A sequence of curves can be supplied when a composite route is intended. `help_point` guides NX when multiple routes are possible.

## Create on a Face

Use normalized surface parameters:

```python
face = Part.get_faces()[0]

point = PointBuilder.create_on_face(
    face=face,
    u_value=0.5,
    v_value=0.5,
)
feature = point.commit_feature()
```

## Create between Two Points

```python
point1 = Point.create_from_coordinates(0, 0, 0)
point2 = Point.create_from_coordinates(100, 50, 25)

point = PointBuilder.create_between_points(
    point1,
    point2,
    percentage_location=50,
)
feature = point.commit_feature()
```

The percentage defaults to `50`, placing the point halfway between the two points.

## Create at a Curve Center

```python
from nxopenkit import Part, PointBuilder

curve = Part.get_curves()[0]

point = PointBuilder.create_at_center(curve)
feature = point.commit_feature()
```

## Create at a Curve or Edge Intersection

```python
curve1 = Part.get_curves()[0]
curve2 = Part.get_curves()[1]

point = PointBuilder.create_at_curves_intersection(
    curve_edge1=curve1,
    curve_edge2=curve2,
)
feature = point.commit_feature()
```

Optional help points can guide NX when more than one intersection is possible:

```python
point = PointBuilder.create_at_curves_intersection(
    curve_edge1=curve1,
    curve_edge2=curve2,
    help_point1=Point3d(0, 0, 0),
    help_point2=Point3d(100, 100, 0),
)
```

## Create an Offset Point

```python
from nxopenkit import Vector3d

source = Point.create_from_coordinates(0, 0, 0)

point = PointBuilder.create_offset_point(
    point=source,
    vector=Vector3d(0, 0, 1),
    offset_distance=25,
)
feature = point.commit_feature()
```

## Create at a Face-Curve Intersection

```python
face = Part.get_faces()[0]
curve = Part.get_curves()[0]

point = PointBuilder.create_at_faces_curves_intersection(
    faces=face,
    curve_edge=curve,
)
feature = point.commit_feature()
```

`faces` and `curve_edge` may each be a single object or a sequence.

## Create at a Datum Plane-Curve Intersection

```python
plane = Part.get_datum_planes()[0]
curve = Part.get_curves()[0]

point = PointBuilder.create_at_plane_curves_intersection(
    plane=plane,
    curve_edge=curve,
)
feature = point.commit_feature()
```

## Accessing the NXOpen Builder

```python
nx_builder = point.nx_builder
print(type(nx_builder).__name__)
```

The returned object is `NXOpen.Features.PointFeatureBuilder`.

## Commit Result

All factory methods return a `PointBuilder`. Commit it to obtain an NXOpenKit `Feature`:

```python
feature = point.commit_feature()
print(feature.name)
```

## Next Steps

- [Creating Geometry](creating_geometry.md)
- [Datum Builder](datum_builder.md)
- [Feature Builders Overview](feature_builders_overview.md)
