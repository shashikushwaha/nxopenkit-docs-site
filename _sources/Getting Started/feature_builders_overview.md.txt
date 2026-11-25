# Feature Builders Overview

NXOpenKit feature builders provide Python-friendly interfaces for creating and configuring NX modeling features. Each builder wraps an NXOpen feature builder while still exposing the underlying NXOpen object when needed.

Feature builders follow this workflow:

1. Create a builder with `create_*` or `__init__`.
2. Configure optional groups, such as settings, limits, Boolean operations, or offsets.
3. Commit the feature with `commit_feature()`.
4. Optionally access the underlying NXOpen builder through `nx_builder`.

## Builder Workflow

Most feature builders return the builder itself from their configuration methods. This supports both direct and chained calls:

```python
builder = SomeBuilder.create_something(...)
builder.settings()
feature = builder.commit_feature()
```

Builder methods are not a uniform `commit()` API for every feature. Feature builders use `commit_feature()`, which returns an NXOpenKit `Feature` wrapper.

## Revolve Example

```python
from nxopenkit import Part, Point, RevolveBuilder

curves = Part.get_curves("PROFILE*")
axis = Part.get_datum_axes("REVOLVE_AXIS")[0]
axis_point = Point.create_from_coordinates(0, 0, 0)

revolve = RevolveBuilder.create_revolve(
    curves,
    axis,
    axis_point,
    start_angle=0,
    end_angle=360,
)

feature = revolve.commit_feature()
```

---

## Builder Classes

Each builder class represents a specific NX feature type:

- [`BlockBuilder`](block_builder.md)
- [`CylinderBuilder`](cylinder_builder.md)
- [`SphereBuilder`](sphere_builder.md)
- [`ExtrudeBuilder`](extrude_builder.md)
- [`RevolveBuilder`](revolve_builder.md)
- [`BooleanBuilder`](boolean_builder.md)
- [`DatumPlaneBuilder`](datum_builder.md)
- [`DatumAxisBuilder`](datum_builder.md)
- [`DatumCsysBuilder`](datum_builder.md)
- [`PointBuilder`](point_builder.md)

Use the builder's documented `create_*` class method when available. Constructor parameters and available configuration methods vary by builder.

## Revolve Configuration

The `RevolveBuilder` accepts profile curves, a direction or datum axis, an axis point, and optional start and end angles.

```python
revolve = RevolveBuilder.create_revolve(
    curves,
    axis,
    axis_point,
    start_angle=0,
    end_angle=360,
)
```

### Settings

Use `settings()` to configure the body style and modeling tolerance:

```python
import NXOpen

revolve.settings(
    body_type=NXOpen.GeometricUtilities.FeatureOptions.BodyStyle.Solid,
    tolerance=0.01,
)
```

### Boolean Operation

Use `boolean()` with an NX Boolean type and a sequence of target bodies:

```python
import NXOpen

revolve.boolean(
    NXOpen.GeometricUtilities.BooleanOperation.BooleanType.Unite,
    [target_body],
)
```

Target bodies are required for Unite, Subtract, and Intersect. Use an empty target-body sequence for Create.

### Offset

Use `offset()` for symmetric or nonsymmetric offsets:

```python
import NXOpen

revolve.offset(
    NXOpen.GeometricUtilities.Type.SymmetricOffset,
    end_offset=5.0,
)
```

For a nonsymmetric offset, provide `start_offset` as well:

```python
import NXOpen

revolve.offset(
    NXOpen.GeometricUtilities.Type.NonsymmetricOffset,
    end_offset=5.0,
    start_offset=2.0,
)
```

## Fluent Configuration

Because configuration methods return the builder, calls can be chained:

```python
import NXOpen

feature = (
    RevolveBuilder.create_revolve(
        curves,
        axis,
        axis_point,
        start_angle=0,
        end_angle=360,
    )
    .settings(body_type=NXOpen.GeometricUtilities.FeatureOptions.BodyStyle.Solid)
    .boolean(
        NXOpen.GeometricUtilities.BooleanOperation.BooleanType.Unite,
        [target_body],
    )
    .commit_feature()
)
```

## Accessing the NXOpen Builder

Every NXOpenKit feature builder exposes an `nx_builder` property that returns the underlying NXOpen builder:

```python
revolve = RevolveBuilder.create_revolve(
    curves,
    axis,
    axis_point,
)

nx_builder = revolve.nx_builder

print(type(nx_builder).__name__)
```

The returned object is an `NXOpen.Features.RevolveBuilder`.

Use `nx_builder` when an NXOpen option is not yet exposed by NXOpenKit:

```python
import NXOpen

nx_builder = revolve.nx_builder
nx_builder.FeatureOptions.BodyType = (
    NXOpen.GeometricUtilities.FeatureOptions.BodyStyle.Solid
)

feature = revolve.commit_feature()
```

## NXOpenKit and NXOpen

NXOpenKit does not replace NXOpen. It provides wrappers for common workflows while preserving direct access through properties such as `nx_builder` and `to_nx`.

```python
face = Part.get_faces()[0]
nx_face = face.to_nx

print(nx_face.JournalIdentifier)
```

## Wrapper Coverage

The wrapper methods are organized to correspond to common NX dialog groups:

| NX dialog area | RevolveBuilder API |
|---|---|
| Section | `curves` constructor argument |
| Axis | `direction` and `axis_point` constructor arguments |
| Limits | `start_angle` and `end_angle` constructor arguments |
| Settings | `settings()` |
| Boolean | `boolean()` |
| Offset | `offset()` |

Use the NXOpen builder for options that are not yet represented by a wrapper method.

## Next Steps

Continue with the geometry and feature documentation:

- [Creating Geometry](creating_geometry.md)
- [Block Builder](block_builder.md)
- [Cylinder Builder](cylinder_builder.md)
- [Sphere Builder](sphere_builder.md)
- [Extrude Builder](extrude_builder.md)
- [Revolve Builder](revolve_builder.md)
- [Boolean Builder](boolean_builder.md)
- [Datum Builders](datum_builder.md)
- [Point Builder](point_builder.md)