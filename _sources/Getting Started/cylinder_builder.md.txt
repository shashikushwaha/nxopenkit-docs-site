# Cylinder Builder

Before running these examples, open or create an NX part and make sure it is the active work part. Name patterns such as `CYLINDER_ARC*` and `TARGET*` are illustrative. Replace them with names that exist in your NX part.

`CylinderBuilder` creates cylindrical solid features in the active NX work part. It supports two construction methods:

- Axis, origin, diameter, and height
- Existing arc and height

```python
from nxopenkit import CylinderBuilder
```

## Create by Axis, Diameter, and Height

Use `create_by_axis_diameter_height()` when the cylinder axis is supplied by a point and a direction-like object.

```python
from nxopenkit import CylinderBuilder, Point, Vector3d

origin = Point.create_from_coordinates(0, 0, 0)
axis = Vector3d(0, 0, 1)

cylinder = CylinderBuilder.create_by_axis_diameter_height(
    origin_point=origin,
    vector=axis,
    diameter=50,
    height=100,
)

feature = cylinder.commit_feature()
```

The vector does not need to be unit length. NX uses its direction. Valid axis objects include `Vector3d`, `Direction`, and `DatumAxis`.

## Create from an Arc

Use `create_by_arc_height()` to construct a cylinder from an existing arc or circular curve:

```python
from nxopenkit import CylinderBuilder, Part

arc = Part.get_curves("CYLINDER_ARC*")[0]

cylinder = CylinderBuilder.create_by_arc_height(
    arc=arc,
    height=100,
)

feature = cylinder.commit_feature()
```

Height and diameter values must be positive. The builder validates height and diameter when the axis/diameter/height form is used.

## Parent Associativity

Use `settings()` to control whether the cylinder origin remains associated with its parent geometry:

```python
cylinder.settings(associate_origin=True)
feature = cylinder.commit_feature()
```

The default is `True`.

## Boolean Operations

Use `boolean()` to combine the cylinder with existing bodies:

```python
import NXOpen

from nxopenkit import CylinderBuilder, Part

target_body = Part.get_bodies("TARGET*")[0]

cylinder = CylinderBuilder.create_by_axis_diameter_height(
    origin,
    axis,
    50,
    100,
)

cylinder.boolean(
    NXOpen.Features.Feature.BooleanType.Unite,
    [target_body],
)

feature = cylinder.commit_feature()
```

The target argument is a sequence of `Body` wrappers:

- `NXOpen.Features.Feature.BooleanType.Create` — create a separate solid
- `NXOpen.Features.Feature.BooleanType.Unite` — unite with the target bodies
- `NXOpen.Features.Feature.BooleanType.Subtract` — subtract the cylinder from the targets
- `NXOpen.Features.Feature.BooleanType.Intersect` — retain intersecting material

Unite, Subtract, and Intersect require at least one target body.

## Fluent Usage

Builder configuration methods return the same builder:

```python
import NXOpen

feature = (
    CylinderBuilder.create_by_axis_diameter_height(
        origin,
        axis,
        50,
        100,
    )
    .settings(associate_origin=True)
    .boolean(NXOpen.Features.Feature.BooleanType.Create, [])
    .commit_feature()
)
```

## Accessing the NXOpen Builder

Use `nx_builder` for NXOpen functionality that is not yet wrapped:

```python
nx_builder = cylinder.nx_builder
print(type(nx_builder).__name__)
```

The returned object is `NXOpen.Features.CylinderBuilder`.

## Next Steps

- [Block Builder](block_builder.md)
- [Sphere Builder](sphere_builder.md)
- [Feature Builders Overview](feature_builders_overview.md)
