# Generating URLs

[[Razor Pages]] includes tools that generate [[URL|URLs]] based on the application's routing configuration. This means, if you change the routing, the links they generate are automatically updated.

The main tools are the [[Tag Helpers|anchor tag helper]] and the `LinkGenerator` service.

The anchor tag helpr is responsible for generating an anchor element that links to pages within the application. The anchor tag generates the href attribute's value based on the routing configuration for the application. So changes to the routing system configuration is picked up from the tag helper and adjusts accordingly.

```html
<a class="nav-link text-dark" asp-area="" asp-page="/Welcome">Welcome</a>
```

The above snippet shows an anchor with two custom attributes, both prefixed with `asp-`, and no `href` attribute. If there was one, a `InvalidOperationException` will be thrown at runtime.

The custom attributes supported by the anchor tag of most interest are:

* page
* page-handler
* route-*
* all-route-data
* host
* protocol
* area

The `page`, `page-handler`, and `route-*` attributes are the most common to work with.

### `page`

The `page` attribute takes the name of the page (the path without the extension, root being Pages folder) that you want to generate to, `/Welcome`. If the page is in subfolders, such as _/Pages/Admin/Index.cshtml_, pass the `page` attribute `/Admin/Index`.

**IF** the routing system can't find the page, the href with contain an empty string. Look out for generated links taking you to the home page, check the value of the offending `page` attribute.

When pages have multiple route templates, the last template in the `PageRouteModel` will be used for generation:

![](https://drek4537l1klr.cloudfront.net/brind/v-10/Figures/04_07.png)

### `page-handler`

The `page-handler` attribute is used to specify the name of the [[PageModel as a Controller#Named Handler Methods|named handler method]] that should be executed. By default, the value passed to the page-handler attribute is applied to the query string with a key named "handler": `?handler=Search`.

This can be changed to become part of the URL by altering the page's route template to include a parameter named "handler". Add this as an optional parameter so the regular `OnGet` and `OnPost` handlers can also be reached: `@page "{handler?}"`

### `route-*`

The `route-*` attribute caters for route parameter values where the `*` represents teh name of the parameter. This tag helper generates a link to the city page for Rome, using `cityName` as the parameter:

```html
<a asp-page="/City" asp-route-cityName="Rome">Rome</a>
```

The `route` attribute in the above example would produce `/City?cityName=Rome`.

### `all-route-data`

The `all-route-data` parameter takes a `Dictionary<sring, string>` as a value, that is then used to wrap multiple route parameter values. This keeps us from having to add multiple `route-*` attributes:

```csharp
var d = new Dictionary<string, string> {{"cityName","Madrid"},{"rating","5"}};
<a asp-page="/City" asp-all-route-data="d">Click</a>
```

### `protocol` & `host`

**THE DEFAULT BEHAVIOR** of the anchor tag helper is to generate a relative URL based on the location of the target page. 

You can specify the protocol ([[HTTP|HTTPS]]) using the `protocol` attribute, the [[Hostname|hostname]] using the `host` attribute, and the anchor tag helper will generate an absolute URL using those values.

### `area`

That last attribute in the list, `area`, is used to specify the _Area_ that the target page is in. The name of the area is included as the first segment of the generated URL.