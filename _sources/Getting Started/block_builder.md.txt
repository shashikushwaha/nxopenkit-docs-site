# Block Builder

Before running these examples, open or create an NX part and make sure it is the active work part. Use `Part.work_part()` in interactive NX scripts or `Part.open_part(...)` in batch scripts.

`BlockBuilder` creates rectangular solid features in the active NX work part. It supports three construction methods:

- Origin and edge lengths
- Two points and height
- Two diagonal points

## Import

```python
from nxopenkit import BlockBuilder, Point
```

## Create by Origin and Edge Lengths

Use `create_by_origin_lengths()` to create a block from an origin point and dimensions along X, Y, and Z.

```python
origin = Point.create_from_coordinates(0, 0, 0)

block = BlockBuilder.create_by_origin_lengths(
    origin_point=origin,
    xc_length=100,
    yc_length=60,
    zc_length=40,
)

feature = block.commit_feature()
```

The default dimensions are `100` for all three edge lengths.

```python
block = BlockBuilder.create_by_origin_lengths(origin)
feature = block.commit_feature()
```

## Create by Two Points and Height

Use `create_by_two_points_height()` to define a base edge from two points and add a height:

```python
base_origin = Point.create_from_coordinates(0, 0, 0)
second_point = Point.create_from_coordinates(100, 0, 0)

block = BlockBuilder.create_by_two_points_height(
    origin_point=base_origin,
    second_point=second_point,
    height=40,
)

feature = block.commit_feature()
```

## Create by Two Diagonal Points

Use `create_by_two_diagonal_points()` to create a block between two opposite corner points:

```python
corner1 = Point.create_from_coordinates(0, 0, 0)
corner2 = Point.create_from_coordinates(100, 60, 40)

block = BlockBuilder.create_by_two_diagonal_points(
    origin_point=corner1,
    second_point=corner2,
)

feature = block.commit_feature()
```

## Parent Associativity

Use `settings()` to control whether the block origin is associated with its parent geometry:

```python
block = BlockBuilder.create_by_origin_lengths(
    origin,
    100,
    60,
    40,
)

block.settings(associate_origin=True)
feature = block.commit_feature()
```

The default is `True`.

## Boolean Operations

Use `boolean()` to combine the block with an existing body:

```python
import NXOpen

from nxopenkit import BlockBuilder, Part, Point

origin = Point.create_from_coordinates(0, 0, 0)
target_body = Part.get_bodies()[0]

block = BlockBuilder.create_by_origin_lengths(
    origin,
    100,
    60,
    40,
)

block.boolean(
    NXOpen.Features.Feature.BooleanType.Unite,
    target_body,
)

feature = block.commit_feature()
```

Supported NX Boolean types include:

- `NXOpen.Features.Feature.BooleanType.Create`
- `NXOpen.Features.Feature.BooleanType.Unite`
- `NXOpen.Features.Feature.BooleanType.Subtract`
- `NXOpen.Features.Feature.BooleanType.Intersect`

A target body is required for Unite, Subtract, and Intersect operations. Blocks created directly use the Create Boolean operation by default, so no `boolean()` call is needed for a separate solid.

## Accessing the NXOpen Builder

The `nx_builder` property exposes the underlying NXOpen block feature builder:

```python
block = BlockBuilder.create_by_origin_lengths(
    origin,
    100,
    60,
    40,
)

nx_builder = block.nx_builder

print(type(nx_builder).__name__)
```

Use this object for NXOpen options that are not yet exposed by `BlockBuilder`.

## Committing a Block

`commit_feature()` commits the builder and returns an NXOpenKit `Feature` wrapper:

```python
feature = block.commit_feature()

print(feature.name)
print(feature.to_nx.JournalIdentifier)
```

The `Feature` wrapper provides access to the committed NX feature and the underlying NXOpen feature through `to_nx`.

## Fluent Usage

Because configuration methods return the builder, the workflow can be chained:

```python
import NXOpen

feature = (
    BlockBuilder.create_by_origin_lengths(
        origin,
        100,
        60,
        40,
    )
    .settings(associate_origin=True)
    .boolean(
        NXOpen.Features.Feature.BooleanType.Unite,
        target_body,
    )
    .commit_feature()
)
```

For Unite, Subtract, or Intersect, pass a `Body` wrapper as the target. Use the default `Create` operation for a separate block.

## Next Steps

- [Feature Builders Overview](feature_builders_overview.md)
- [Creating Geometry](creating_geometry.md)
