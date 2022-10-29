# Sections
[[Razor]] includes **sections**, defined using the `@section` directive within the **CONTENT PAGE**.

```csharp
@section ThingsToDoWidget {
	<p>Visit Eiffel Tower</p>
}
```

## Rendering sections
When rendering this content in the [[Layout Pages|layout page]], place a call to the `RenderSectionAsync` method at the location you want the content to appear.

This method has two versions:

1. One that takes the section's name as a string.
2. The other takes a boolean indicating whether all content pages are required to define the section.

```csharp
@await RenderSectionAsync("ThingsToDoWidget", false)
```

An example of this is in the default `_Layout` page. A call to the `RenderSectionAsync` refers to the "scripts" section:

```html
	<script scr="~/lib/jquery/dist/jquery.min.js"></script>
	<script scr="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
	<script src="~/js/site.js" asp-append-version="true"></script>
	@await RenderSectionAsynnc("Scripts", required: false)
```

The above shows that the main `_Layout` page includes the global script files and **allows any page that inherits from this layout** to define it's own scripts, which **will be inserted where `RenderSectionAsync` is called**.

`IsSectionDefined` can be used to show default content if a section hasn't been defined in the content page.

```csharp
<div class="card alert-danger p-5 border-5 mt-2">
@if (IsSectionDefined("ThingsToDoWidget"))
{
	@await RenderSectionAsync("ThingsToDoWidget")
}
else
{
	<p>Things To Do Widget default content</p>
}
</div>
```

When a content page defines a section, it **MUST BE HANDLED BY THE LAYOUT PAGE** using one of the following:

* `RenderSectionAsync`
*  `IsSectionDefined`
* `IgnoreSection`

```csharp
@if(!IsAdmin)
{
	IgnoreSection("admin");
}
else
{
	@await RenderSectionAsync("admin")
}
```

Due to `IgnoreSection` returning void, we don't prefix with `@` and must terminate with a `;`.