---
layout: post
title: "Managed object internals in .NET"
categories: tech dotnet
---

While the JVM and the CLR often say that "the details of how objects are laid out in memory is not part of the public API and subject to change and don't depend on it", I have found that it is often necessary, or at least useful, to know that the layout of an object in memory is.
This sequence of blogs does a great job of investigating and explaining those internals and providing a way to introspect the layout of an object.

1. [https://devblogs.microsoft.com/premier-developer/managed-object-internals-part-1-layout/](https://devblogs.microsoft.com/premier-developer/managed-object-internals-part-1-layout/)
2. [https://devblogs.microsoft.com/premier-developer/managed-object-internals-part-2-object-header-layout-and-the-cost-of-locking/](https://devblogs.microsoft.com/premier-developer/managed-object-internals-part-2-object-header-layout-and-the-cost-of-locking/)
3. [https://devblogs.microsoft.com/premier-developer/managed-object-internals-part-3-the-layout-of-a-managed-array-3/](https://devblogs.microsoft.com/premier-developer/managed-object-internals-part-3-the-layout-of-a-managed-array-3/)
4. [https://devblogs.microsoft.com/premier-developer/managed-object-internals-part-4-fields-layout/](https://devblogs.microsoft.com/premier-developer/managed-object-internals-part-4-fields-layout/)

Additionally there is a library [ObjectLayoutInspector](https://www.nuget.org/packages/ObjectLayoutInspector) that provides a way to investigate the runtime layout of some .NET class or struct.
How to use this is explained, in great detail, in the project's [readme](https://github.com/SergeyTeplyakov/ObjectLayoutInspector), but the basic idea is as follows:

```csharp
ObjectLayoutInspector.TypeLayout.PrintLayout<SomeType<Decimal>(recursively: true);
var layout = ObjectLayoutInspector.TypeLayout.GetLayout<string>();
Console.WriteLine(layout.ToString());

class SomeType<T> {
    string s;
    (int, T, T, T) t;
}
```
