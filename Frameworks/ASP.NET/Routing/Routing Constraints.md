
## Route Constraints

As far as routing is concerned, all URL data type is a string type. This is problematic when trying to use data from the URL to use in calculations or to construct various other types.

Route constraints provide a solution to this problem. They enable you to specify the data type and range of values that a route data item must comply with.

```csharp
"{cityName}/{arrivalYear:int}-{arrivalMonth:int}-{arrivalDay:int}"
```

The range constraint accepts minimum and maximum acceptable values:

```csharp
"{cityName}/{arrivalYear:int}-{arrivalMonth:range(1-12)}-{arrivalDay:int}"
```

The above examples are obviously trying to make a date with the passed in parameters, we could change the template altogether and constrain the value to a `datetime` type.

```csharp
"{cityName}/{arrivalDate:datetime}"
```

## Constraints Table

Constraint|Description|Example
:--:|--|:--:
`alpha`| Matches uppercase or lowercase Latin alphabet characters. |`{title:alpha}`
`bool`| Matches a Boolean value. | `{isActive:bool}`
`int`| Matches a 32-bit integer value. | `{id:int}`
`datetime`| Matches a DateTime value. | `{startdate:datetime}`
`decimal`| Matches a decimal value. | `{cost:decimal}`
`double`| Matches a 64-bit floating-point value. | `{latitude:double}`
`float`| Matches a 32-bit floating-point value. | `{x:float}`
`long`| Matches a 64-bit integer value. | `{x:long}`
`guid`| Matches a GUID value. | `{id:guid}`
`length`| Matches a string with a specified length or within a specified range of lengths| `{key:length(8, 10)}`
`min`| Matces an integer with a minimum value. | `{age:min(18)}`
`max`| Matches an integer with a maximum value. | `{height:max(10)}`
`minlength`| Matches a string with a minimum length. | `{title:minlength(2)}`
`maxlength`| Matches a string with a maximum length. | `{postcode:maxlength(8)}`
`range`| Matches an integer within a range of values. | `{month:range(1,12)}`
`regex`| Matches a regular expression. | `{postcode:regex(^[A-Z]{2}\d\s?\d[A-Z]{2}$)}`


It is possible to apply multiple constraints to a parameter. The below example shows a value that must be only letters and of max length 9:

```csharp
"{cityName:alpha:maxlength(9)}"
```

## Custom Route Constraints

The constraints table above only shows the most commonly used ones, as there are others on the [[ASP.NET]] documentation site. It is also possible to create your own custom route constraints and register them with the routing service.

The following example shows a route constraint on the value for the`cityName` parameter. Route constraints are classes that implement the `IRouteConstraint` interface.

The first thing would be to create a C# class named `CityRouteConstraint`, place in folder named `RouteConstraints` for organization, with the following example code:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Routing;
using System;
using Sytem.Linq;

namespace CityBreaks.RouteConstraints
{
	public class CityRouteConstraint : IRouteConstraint
	{
		public bool Match(HttpContext httpContext, IRouter route, string routeKey, RouteValueDictionary values, RouteDirection routeDirection)
		{
			var cities = new[] { "amsterdam", "barcelona", "berlin", 
                                 "copenhagen", "dubrovnik", "edinburgh", 
                                 "london", "madrid", "paris", "rome", "venice" };
			return 
                cities.Contains(values[routeKey]?.ToString().ToLowerInvariant());
		}
	}
}
```

The `IRouteConstraint` [[Interface|interface]] has one member - a method named `Match` that returns a [[Type System|boolean]]. This method takes a number of parameters, not all required, just what is needed to perform the required action.

In the above example, we only need the `RouteValueDictionary` and the `routeKey`. The `routeKey` is the name of the parameter being checked, if there is a match between the incoming parameter and an item in the cities array, `Match` will return `true`.

**CUSTOM ROUTES MUST BE REGISTERED** in order to recognized and used by ASP.NET. This is done in the `Program` class and is added to the `ConstraintMap` collection:

```csharp
builder.Services.AddRazorPages();
builder.Services.Configure<RouteOptions>(options => {
	options.ConstraintMap.Add("city", typeof(CityRouteConstraint));
})
```

Once registered, use the constraint like any other:

```csharp
@page "{cityName:city}"
```

