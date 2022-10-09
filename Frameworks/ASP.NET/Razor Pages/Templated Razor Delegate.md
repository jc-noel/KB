# Templated Razor Delegate

The **TEMPLATED RAZOR DELEGATE** feature enables you to use a [[Delegates|delegate]] to create [[Razor]] templates and assign them to a variable for reuse.

Razor template delegates are express as a `Func<dynamic, object>` (a generic delegate).

* The body of the method contains a snippet of Razor, with the opening [[HTML]] tag prefixed with an `@` symbol.
* The **input parameter** represents data and is a `dynamic` type, so it can **represent anything**.
* The data is accessible within the template through a parameter named `item`.

```csharp
@{
	Func<dynamic, object> myUnorderedListTemplate = @<ul>
		@foreach (var city in item)
		{
			<li>@city.Name<li>
		}
	</ul>;
}
@myUnorderedListTemplate(cities)
```

* The above example relies on a `dynamic` input parameter, leading to **potential errors** that only surface at runtime.
* Use strong typing to confine the types your template accepts.

The `dynamic` parameter has **been replaced** with a `List<City>`:
```csharp
@{
	Func<List<City>, object> myUnorderedListTemplate =
	@<ul>
		@foreach (var city in item)
		{
			<li>@city.Name<li>
		}
	</ul>
}
```

A **DRAWBACK** is the templated Razor delegate accepts only one argument representing the data item. However, there is an alternative that enables a template to take any number of parameters.

Create a method in a code or a `functions` block that returns `void` (or `Task` if async processing is required) and just include HTML markup in the body of the method:

```csharp
@{
	void MyUndorderedListTemplate(List<City> cities, string style)
	{
		<ul>
		@foreach (var city in cities)
		{
			<li class="@(city.Name) == "London" ? style : null)">@city.Name</li>
		}
		</ul>
	}
}
@{ MyUnorderedListTemplate(cities, "active"); }
```

These helpers are only intended for **REUSE WITHIN THE SAME RAZOR PAGE**