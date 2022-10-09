# Layout Pages

**DEFINING A LAYOUT PAGE** only takes a call to `RenderBody` in a regular [[Razor]] file with a `.cshtml` extension. This method will render the page-specific content.

Once defined, assigning the layout can be done per page by setting the `Layout` property. This string can be the name of the layout file without its extension or the path to the file.

```csharp
@{
	Layout = "_Layout";
	Layout = "/Pages/Shared/_Layout.cshtml";
}
```

When setting the layout by name only, the framework will look in predefined locations.

First the folder containing the calling page, then moves up. If still nothing, will look in shared folders. Using `/Pages/Admin/DestinationOrders` as an example, the search would look like the following:

* `/Pages/Admin/DestinationOrders/_Layout.cshtml`
* `/Pages/Admin/_Layout.cshtml`
* `/Pages/_Layout.cshtml`
* `/Pages/Shared/_Layout.cshtml`
* `/Views/Shared/_Layout.cshtml`

## ViewStart
Instead of assigning the `Layout` property on each page, we can use a file called `_ViewStart`. This is a special Razor file that contains Razor code and is **executed BEFORE** any page that it affects. Which would be any Razor page in the same folder and all subfolders.

This file usually consists of a code block only, although it can contain markup as well. Due to this file being executed before any pages, it is the ideal way to set layouts for pages. **HOWEVER**, any layout assignment that takes place in an individual page will override the `_ViewStart` settings.

Also, placing additional `_ViewStart` files lower in the pages folder hierarchy, will override any assignments as well.

**Nested** layouts can be achieved by explicitly assigning a layout in a child layout.

![Nested Layouts](https://drek4537l1klr.cloudfront.net/brind/v-10/Figures/03image005.png)