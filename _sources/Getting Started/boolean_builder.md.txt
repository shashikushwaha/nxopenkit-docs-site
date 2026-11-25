# Boolean Builder

Before running these examples, open or create an NX part and make sure it is the active work part. The target and tool name patterns in this page are examples; replace them with bodies in your model.

`BooleanBuilder` combines a target body with one or more tool bodies using Unite, Subtract, or Intersect.

```python
from nxopenkit import BooleanBuilder
```

## Operations

Use the dedicated factory methods:

| Method | Operation |
|---|---|
| `create_unit()` | Unite |
| `create_subtract()` | Subtract |
| `create_intersect()` | Intersect |

## Unite Bodies

Unite adds the tool bodies to the target body:

```python
from nxopenkit import BooleanBuilder, Part

target = Part.get_bodies("TARGET*")[0]
tools = Part.get_bodies("TOOL*")

boolean = BooleanBuilder.create_unit(
    target_body=target,
    tool_bodies=tools,
)

feature = boolean.commit_feature()
```

At least one tool body is required.

## Subtract Bodies

Subtract removes the tool bodies from the target body:

```python
boolean = BooleanBuilder.create_subtract(
    target_body=target,
    tool_bodies=tools,
)

feature = boolean.commit_feature()
```

## Intersect Bodies

Intersect retains the regions shared by the target and tool bodies:

```python
boolean = BooleanBuilder.create_intersect(
    target_body=target,
    tool_bodies=tools,
)

feature = boolean.commit_feature()
```

## General Constructor

The constructor can be used directly with an NX Boolean operation enum:

```python
import NXOpen

boolean = BooleanBuilder(
    target_body=target,
    tool_bodies=tools,
    boolean_type=NXOpen.Features.Feature.BooleanType.Unite,
)

feature = boolean.commit_feature()
```

## Settings

Use `settings()` to configure the modeling tolerance and copy options:

```python
boolean.settings(
    tolerance=0.01,
    keep_target=True,
    keep_tool=True,
    convert_to_sew=False,
)
```

- `keep_target=True` preserves an additional copy of the target body.
- `keep_tool=True` preserves an additional copy of the tool bodies.
- `convert_to_sew=True` enables conversion to a sew operation in the NXOpen builder.
- `tolerance` must be a finite positive number.

## Commit Result

`commit_feature()` creates an NXOpenKit `Feature` wrapper:

```python
feature = boolean.commit_feature()
print(feature.name)
```

## Accessing the NXOpen Builder

```python
nx_builder = boolean.nx_builder
print(type(nx_builder).__name__)
```

The returned object is `NXOpen.Features.BooleanBuilder`.

## Boolean in a Feature Builder

Block, cylinder, sphere, extrude, and revolve builders also provide a `boolean()` method for setting a Boolean operation as part of feature creation:

```python
import NXOpen

from nxopenkit import ExtrudeBuilder

feature = (
    ExtrudeBuilder.create_extrude(profiles, 0, 50)
    .boolean(
        NXOpen.GeometricUtilities.BooleanOperation.BooleanType.Subtract,
        [target_body],
    )
    .commit_feature()
)
```

Use `BooleanBuilder` when the Boolean operation is the main operation rather than an option on another feature builder.

## Next Steps

- [Cylinder Builder](cylinder_builder.md)
- [Sphere Builder](sphere_builder.md)
- [Extrude Builder](extrude_builder.md)
- [Revolve Builder](revolve_builder.md)
