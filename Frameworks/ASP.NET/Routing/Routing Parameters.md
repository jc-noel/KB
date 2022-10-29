# Routing Parameters

In [[Razor Pages]], data that is passed in the [[URL]] is known as _**route data**_ and is represented in [[Routing Templates|routing templates]] as _**route parameters**_.

Parameters are added to the route templates by specifying their name in curly braces { }. These can be named almost anything, besides the following reserved words:

* action
* area
* controller
* handler
* page

The following line of code shows a parameter named **cityName** being added to the route template for the City page:

```csharp
@page "{cityName}"
```

The route template that gets created for this would be `City/{cityName}`. So any value that is passed into the named parameter gets added to a `RouteValueDictionary` object (dictionary of route values) and added to a `RouteData` object.

To retrieve these values, use `RouteData.Values`:

```csharp
@page "{cityName}"
@model CityBreaks.Pages.CityModel

<h2>@RouteData.Values["cityName"] Details</h2>
```

With this setup, if we do not provide a value when navigating to the `City` page, we will get a [[HTTP Response Status Codes|404]] response. This is because, by default, parameters require a value and cannot be ommitted.

Parameters can be made options with a question mark - `{cityName?}`. These optional parameters must be the last parameter in a route template to work properly. The parameters before it must either be required or have defaults assigned to them:

```csharp
"{cityName=Paris}"
```


## Binding route data to handler parameters

Query string values in URLs are automatically bound to parameters in [[PageModel]] handler methods if their **names match** and the same is true for route data. When an incoming value is assigned to a handler method parameter, you then assign that value to a PageModel property.

```csharp
[City.cshtml.cs]
public class CityModel : PageModel
{
	public string CityName { get; set; }
	public void OnGet(string cityName)
	{
		CityName = cityName;
	}
}

[City.cshtml]
@page "{cityName=Paris}"
@model CityBreaks.Pages.CityModel
@{
}
<h3>@Model.CityName Details</h3>
```

The above example showed how to handle one parameter, below we demonstrate multiple parameters, (note the segments are separated by a `/`):

```csharp
[City.cshtml.cs]
public class CityModel : PageModel
{
	public string CityName { get; set; }
	public int? Rating { get; set; }
	public void OnGet(string cityName, int? rating)
	{
		CityName = cityName;
		Rating = rating;
	}
}

[City.cshtml]
@page "{cityName}/{rating?}"
@model CityBreaks.Pages.CityModel
@{
}
<h3>@Model.CityName Details</h3>
<p>Minimum Rating: @Model.Rating.GetValueOrDefault()</p>
```

When taking note that the parameter segments are separated by a `/`, multiple parameters are also allowed in the same segment. To achieve this, you must separate the parameters with literal text that isn't featured in the parameters value.

```csharp
"{cityName}/{arrivalYear}-{arrivalMonth}-{arrivalDay}"
```

The above example uses `-` as the literal text to separate the parameter values and will match on the following `/City/London/2022-4-18`.

## Catch-all parameters

There will be occasions when you don't know how many segments the URL will consist of. An example would be when mapping URLs to database entries from a CMS. A [[Routing Templates|route template]] will need to match anything so that when the given endpoint is selected, it can look up the URL in the database and grab what it needs.

To declare a catch all parameter, prefix the name with one or two asterisks:

* `{* cityName}`
* `{** cityName}`

Catch alls will match everything from the parameter position to the end of the URL, so make it the last parameter. The difference between one or two asterisks is as follows:

* One asterisk with URL-encode path separators as `%2F`. Example: `/City/London/2022/4/18` becomes `/City%2fLondon%2F2022%2F4%2F18`
* Two asterisks the encoding is decoded, _round tripped_, to include literal path separators: `/City/London/2022/4/18`