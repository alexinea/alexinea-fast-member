<p>
    <a href="https://alexinea.com" target="_blank" title="FastMember">
        <img width="100" src="logo.png" />
    </a>
</p>

# Fast access to .net fields/properties

[![NuGet](https://img.shields.io/nuget/v/Alexinea.FastMember.svg)](https://nuget.org/packages/Alexinea.FastMember) 
[![Nuget](https://img.shields.io/nuget/dt/Alexinea.FastMember.svg)](https://nuget.org/packages/Alexinea.FastMember)

> This is a stopgap project, because the official upstream project has not been updated in the past period of time.
>
> At the same time, this project merged the pr submitted by other fork repo of the upstream project:
> - https://github.com/mgravell/fast-member/pull/85
> - https://github.com/mgravell/fast-member/pull/86
> 
> Please use upstream official projects first. https://github.com/mgravell/fast-member

In .NET reflection is slow... well, kinda slow. If you need access to the members of an arbitrary type, with the type and member-names known only at runtime - then it is frankly **hard** (especially for DLR types). This library makes such access easy and fast.

An introduction to the reasons behind fast-member can be found <a href="http://marcgravell.blogspot.com/2012/01/playing-with-your-member.html" target="_blank">on my blog</a>; example usage is simply:

```csharp
var accessor = TypeAccessor.Create(type); 
string propName = // something known only at runtime 
while( /* some loop of data */ )
{ 
  accessor[obj, propName] = rowValue; 
}
```
or
```csharp
// obj could be static or DLR 
var wrapped = ObjectAccessor.Create(obj);
string propName = // something known only at runtime 
Console.WriteLine(wrapped[propName]);
```
### Ever needed an `IDataReader`?

This is pretty common if you are doing object-mapping between an object model and ADO.NET concepts such as `DataTable` or `SqlBulkCopy`; loading a `DataTable` (yes, some people still use it) from a sequence of typed objects can now be as easy as:
```csharp
IEnumerable<SomeType> data = ... 
var table = new DataTable(); 
using(var reader = ObjectReader.Create(data)) 
{ 
  table.Load(reader); 
}
```
(the `Create` method offers parameters to control the specific members, if needed)

Or if you want to throw the data into a database as fast as humanly possible:
```csharp
using(var bcp = new SqlBulkCopy(connection)) 
using(var reader = ObjectReader.Create(data, "Id", "Name", "Description")) 
{ 
  bcp.DestinationTableName = "SomeTable"; 
  bcp.WriteToServer(reader); 
}
```

### Ahead of Time
Library emits IL code during runtime. Will not work in constrained Ahead of Time environments. Xamarin iOS and Unity IL2CPP are such.


### Extension Project

- [Alexinea.FastMember.Extensions](https://github.com/alexinea/alexinea-fast-member-extensions)

In addition, we also recommend that you use [Leo](https://github.com/night-moon-studio/leo) based on [NCC Natasha](https://github.com/dotnetcore/Natasha), a high-performance object reading and writing component.

## License

[Apache License 2.0](LICENSE)