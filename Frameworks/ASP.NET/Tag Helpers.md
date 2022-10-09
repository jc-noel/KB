# Tag Helpers

Tag helpers are components that autmoate the generation of [[HTML]]. They target standard HTML tags, such as [[anchor]], [[input]], [[link]], [[form]], and [[image]] tags (including [[Partial Views|partials]]).

Each tag helper is designed to act on a specific tag and parse data from [[Server-Side|server side]] processing to generate HTML.

Anchor tag example:
```html
<a class="nav-link text-dark" asp-area="" asp-page="/Welcome">Welcome</a>
```

The `asp-area` and `asp-page` are custom attributes that provide information to the anchor tag about the _area_ and _page_ that should be used when generating the HTML/URL.

## Enabling tag helpers

Like with most things in [[DOTNET|.NET 6]], tag helpers are an **opt-in** feature. So we must enable the feature to use them. If starting from a template, tag helpers are enabled via the `_ViewImports.cshtml` file in the Pages folder.

```csharp
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

The `addTagHelper` directive takes two arguments:
1. The tag helpers to enable
2. The name of the [[Assembly|assembly]] containing the tag helpers.

In our example, we are using the wildcard ( * ) to grab all tag helpers from the assembly `Microsoft.AspNetCore.Mvc.TagHelpers`.

With `addTagHelper` comes its twin, `removeTagHelper`. With this, you can selectively remove tag helpers.

```csharp
@removeTagHelper "Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers"
```

You can also opt individual tags out of processing by prefixing the tag with the `!` character. If you want the specific element to be processed via the client instead of the server, you would do that using this method.

```html
<!a href="https://www.learnrazorpages.com">Learn Razor Pages</!a>
```

Notice the prefix is placed at both start and ending tags. Another method to modify tag helper processing is by opting specific tags _into_ the parse/processing. To achieve this, you register a custom prefix with the `@tagHelperPrefix` direct and apply the chosen prefix.

This can be done in the `ViewImports` file where the tag helper enabling occurs:

```csharp
@tagHelperPrefix x
```

```html
<xa asp-page="/Index">Home<xa>
```

To help with readability and clarity, you can use punctuation to separate the prefix from the tag name:

```csharp
@tagHelperPrefix x:
```

```html
<x:a asp-page="/Index">Home<x:a>
```
