# Passing Data (ViewData)

The recommended approach with sharing data to the page is in a **strongly typed** manner as a view model. However, there is another option which is weakly typed, a dictionary-based feature called `ViewData`.

### ViewData

With `ViewData`, items are stored within the `ViewDataDictionary` as key-value pairs, and accessed by referencing the case-insensitive string key:

```csharp
ViewData["Title"] = "Welcome!";
```

Then in the layout page:

```html
<title>@ViewData["Title"] - WebApplication1</title>
```

The values in the `ViewDataDictionary` are `object` types, meaning you can store anything you like in the `ViewData`. If the value is a non-string type, it must be casted to it's correct type. 

A `DateTime` type is shown:

```csharp
ViewData["SaleEnds"] = new DateTime(DateTime.Now.Year, 6, 30, 20, 0, 0);
```

Displaying the value in the layout:

```html
<p>Sale ends at @ViewData["SaleEnds"]</p>
```

Then to access DateTime specific methods and properties:

```csharp
Sale Ends at: @(((DateTime)ViewDate["SaleEnds"]).ToString("h tt, MMMM dd"))
```

Giving the output: `Sale Ends at: 8PM, June 30`.

**YOU ARE ABLE** to set the values of `ViewData` items in the PageModel class itself. The following example show's this in the handler method called `OnGet`:

```csharp
public class WelcomeModel : PageModel
{
	public void OnGet()
	{
		ViewData["Title"] = "Welcome!";
	}
}
```

To access this data, you must ensure that the Razor page includes the `@model` directive references the `WelcomeModel` type:

```csharp
@model WelcomeModel
```

**BE AWARE**, assignments made in the Razor page will overwrite any made within the PageModel.

`ViewData` should be used sparingly, it is useful for passing small pieces of simple data to layouts, like a page's title. As previously mentioned at the beginning, using strongly typed PageModel properties is the recommended way.

See: [[PageModel as a View Model]]