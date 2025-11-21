## Short and Accurate Difference between interface and type in TypeScript:

|Feature|`interface`|`type`|
|---|---|---|
|**Use case**|Describes object shapes|Can alias any type (objects, unions, primitives, etc.)|
|**Extending**|Can be extended with `extends`|Can use intersections (`&`) to combine|
|**Merging**|Supports declaration merging|Does **not** support merging|
|**Flexibility**|Best for defining object contracts|More flexible for complex types|

✅ **Use** `**interface**` when defining object/class shapes.

✅ **Use** `**type**` when working with unions, primitives, or need advanced type composition.

[https://www.typescript-training.com/course/fundamentals-v4/11-classes/](https://www.typescript-training.com/course/fundamentals-v4/11-classes/)