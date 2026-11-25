# Revolve Builder

Before running these examples, open or create an NX part and make sure it is the active work part. Profile and axis names such as `PROFILE*` and `REVOLVE_AXIS*` are examples; replace them with objects in your model.

`RevolveBuilder` creates a feature by revolving one or more profile curves around an axis.

```python
from nxopenkit import RevolveBuilder
```

## Basic Revolve

Use `create_revolve()` with profile curves, a direction or axis, and an axis point:

```python
from nxopenkit import Part, Point, RevolveBuilder

profiles = Part.get_curves("PROFILE*")
axis = Part.get_datum_axes("REVOLVE_AXIS*")[0]
axis_point = Point.create_from_coordinates(0, 0, 0)

revolve = RevolveBuilder.create_revolve(
    curves=profiles,
    direction=axis,
    axis_point=axis_point,
    start_angle=0,
    end_angle=360,
)

feature = revolve.commit_feature()
```

At least one profile curve is required.

## Axis Options

The `direction` parameter accepts:

- `DatumAxis`
- `Direction`
- `Vector3d`

For an axis defined by a vector and point:

```python
from nxopenkit import Point, Vector3d

axis_point = Point.create_from_coordinates(0, 0, 0)
axis_direction = Vector3d(0, 0, 1)

revolve = RevolveBuilder.create_revolve(
    profiles,
    axis_direction,
    axis_point,
)
```

## Start and End Angles

Angles default to `0` and `360` degrees. Values may be numbers or strings containing NX expressions:

```python
revolve = RevolveBuilder.create_revolve(
    profiles,
    axis,
    axis_point,
    start_angle=0,
    end_angle=180,
)
```

## Settings

Use `settings()` to configure the body type and modeling tolerance:

```python
import NXOpen

revolve.settings(
    body_type=NXOpen.GeometricUtilities.FeatureOptions.BodyStyle.Solid,
    tolerance=0.01,
)
```

## Boolean Operation

Use `boolean()` to combine the revolve feature with one or more target bodies:

```python
revolve.boolean(
    NXOpen.GeometricUtilities.BooleanOperation.BooleanType.Unite,
    [target_body],
)
```

Target bodies are required for Unite, Subtract, and Intersect.

## Offset

Use `offset()` to add a symmetric or nonsymmetric offset:

```python
revolve.offset(
    NXOpen.GeometricUtilities.Type.SymmetricOffset,
    end_offset=5,
)
```

For a nonsymmetric offset:

```python
revolve.offset(
    NXOpen.GeometricUtilities.Type.NonsymmetricOffset,
    end_offset=5,
    start_offset=2,
)
```

## Complete Example

```python
import NXOpen

from nxopenkit import (
    Part,
    Point,
    RevolveBuilder,
)

profiles = Part.get_curves("PROFILE*")
axis = Part.get_datum_axes("REVOLVE_AXIS*")[0]
axis_point = Point.create_from_coordinates(0, 0, 0)
target = Part.get_bodies("TARGET*")[0]

feature = (
    RevolveBuilder.create_revolve(
        profiles,
        axis,
        axis_point,
        start_angle=0,
        end_angle=360,
    )
    .settings(
        body_type=NXOpen.GeometricUtilities.FeatureOptions.BodyStyle.Solid,
        tolerance=0.01,
    )
    .boolean(
        NXOpen.GeometricUtilities.BooleanOperation.BooleanType.Unite,
        [target],
    )
    .commit_feature()
)
```

## Accessing the NXOpen Builder

```python
nx_builder = revolve.nx_builder
print(type(nx_builder).__name__)
```

The returned object is `NXOpen.Features.RevolveBuilder`.

## Next Steps

- [Extrude Builder](extrude_builder.md)
- [Datum Builder](datum_builder.md)
- [Feature Builders Overview](feature_builders_overview.md)
