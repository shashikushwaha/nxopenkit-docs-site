# Extrude Builder

Before running these examples, open or create an NX part and make sure it is the active work part. Profile and target names such as `PROFILE*` and `TARGET*` are examples that may need to be changed for your model.

`ExtrudeBuilder` creates a feature by extruding one or more profile curves along a direction.

```python
from nxopenkit import ExtrudeBuilder
```

## Basic Extrusion

Use `create_extrude()` with profile curves, a start distance, and an optional end distance:

```python
from nxopenkit import ExtrudeBuilder, Part

profiles = Part.get_curves("PROFILE*")

extrude = ExtrudeBuilder.create_extrude(
    curves=profiles,
    start_distance=0,
    end_distance=50,
)

feature = extrude.commit_feature()
```

At least one profile curve is required.

## Direction

The default direction is positive Z:

```python
Vector3d(0, 0, 1)
```

Supply a custom direction with `direction`:

```python
from nxopenkit import ExtrudeBuilder, Vector3d

extrude = ExtrudeBuilder.create_extrude(
    profiles,
    0,
    50,
    direction=Vector3d(1, 0, 0),
)
```

The supplied vector is normalized by the builder.

## Start and End Limits

Distances may be numbers or strings containing valid NX expressions:

```python
extrude = ExtrudeBuilder.create_extrude(
    profiles,
    start_distance=-10,
    end_distance="50",
)
```

If `end_distance` is omitted, the NX extrusion limit may remain unset until configured through `nx_builder` or an NXOpen workflow.

## Settings

Use `settings()` to configure the body style and tolerances:

```python
import NXOpen

extrude.settings(
    body_type=NXOpen.GeometricUtilities.FeatureOptions.BodyStyle.Solid,
    tolerance=0.01,
)
```

`tolerance` must be a finite positive number when supplied.

## Offset

Use `offset()` for symmetric or nonsymmetric offsets:

```python
import NXOpen

extrude.offset(
    NXOpen.GeometricUtilities.Type.SymmetricOffset,
    end_offset=5,
)
```

For a nonsymmetric offset, provide both offsets:

```python
extrude.offset(
    NXOpen.GeometricUtilities.Type.NonsymmetricOffset,
    end_offset=5,
    start_offset=2,
)
```

## Boolean Operation

Use `boolean()` to unite, subtract, or intersect the extrusion:

```python
import NXOpen

extrude.boolean(
    NXOpen.GeometricUtilities.BooleanOperation.BooleanType.Unite,
    [target_body],
)
```

Target bodies are required except for Create operations.

## Draft

Use `draft()` to apply a simple draft angle:

```python
extrude.draft(
    draft_type=NXOpen.GeometricUtilities.SimpleDraft.SimpleDraftType.SimpleFromProfile,
    draft_angle=5,
)
```

An optional `draft_option` can be supplied for NXOpen `MultiDraft` angle configuration.

## Complete Example

```python
import NXOpen

from nxopenkit import ExtrudeBuilder, Part, Vector3d

profiles = Part.get_curves("PROFILE*")
target = Part.get_bodies("TARGET*")[0]

feature = (
    ExtrudeBuilder.create_extrude(
        profiles,
        start_distance=0,
        end_distance=50,
        direction=Vector3d(0, 0, 1),
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
nx_builder = extrude.nx_builder
print(type(nx_builder).__name__)
```

The returned object is `NXOpen.Features.ExtrudeBuilder`.

## Next Steps

- [Revolve Builder](revolve_builder.md)
- [Boolean Builder](boolean_builder.md)
- [Feature Builders Overview](feature_builders_overview.md)
