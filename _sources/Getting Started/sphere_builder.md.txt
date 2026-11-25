# Sphere Builder

Before running these examples, open or create an NX part and make sure it is the active work part. Replace example names such as `SPHERE_ARC*` or `TARGET*` with objects in your model.

`SphereBuilder` creates spherical solid features in the active NX work part. It supports two construction methods:

- Center point and diameter
- Existing arc

```python
from nxopenkit import SphereBuilder
```

## Create by Center Point and Diameter

Use `create_by_center_point_diameter()` to create a sphere from its center and diameter:

```python
from nxopenkit import Point, SphereBuilder

center = Point.create_from_coordinates(0, 0, 0)

sphere = SphereBuilder.create_by_center_point_diameter(
    center_point=center,
    diameter=50,
)

feature = sphere.commit_feature()
```

For a radius-based workflow, convert the desired radius to diameter:

```python
radius = 25
sphere = SphereBuilder.create_by_center_point_diameter(
    center,
    radius * 2,
)
```

## Create from an Arc

Use `create_by_arc()` when the sphere should be created from an existing arc:

```python
from nxopenkit import Part, SphereBuilder

arc = Part.get_curves("SPHERE_ARC*")[0]

sphere = SphereBuilder.create_by_arc(arc)
feature = sphere.commit_feature()
```

## Parent Associativity

Use `settings()` to control associativity:

```python
sphere.settings(associate_origin=True)
feature = sphere.commit_feature()
```

The default is `True`.

## Boolean Operations

Use `boolean()` to unite, subtract, or intersect the sphere with another body:

```python
import NXOpen

from nxopenkit import Part, SphereBuilder

target_body = Part.get_bodies("TARGET*")[0]

sphere = SphereBuilder.create_by_center_point_diameter(
    center,
    50,
)

sphere.boolean(
    NXOpen.Features.Feature.BooleanType.Unite,
    [target_body],
)

feature = sphere.commit_feature()
```

Target bodies are required for Unite, Subtract, and Intersect. An empty target list can be used with Create.

## Accessing the NXOpen Builder

```python
nx_builder = sphere.nx_builder
print(type(nx_builder).__name__)
```

The returned object is `NXOpen.Features.SphereBuilder`.

## Next Steps

- [Cylinder Builder](cylinder_builder.md)
- [Boolean Builder](boolean_builder.md)
- [Feature Builders Overview](feature_builders_overview.md)

Name patterns such as `SPHERE_ARC*` and `TARGET*` are illustrative. Replace them with names that exist in your NX part.
