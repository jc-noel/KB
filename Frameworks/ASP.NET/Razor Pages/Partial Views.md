# Partial Views

**PARTIAL VIEWS** are Razor `.cshtml` files that contain a chunk of [[HTML]] and optionally some [[Razor|razor]] syntax. The difference between partials and a standard Razor page is that it does **NOT** include a `@page` directive.

This is because partials are not meant to be browsed to directly. Partials can be used to:

* Break up comples UI
* [[DRY|Avoid repetitive code]]
* Generate HTML for async partial page updates

By convention, partial views are named with a leading underscore `_myPartial.cshtml`. These can be placed anywhere in the `Pages` folder and the discovery process is the same for [[Layout Pages|layouts]].

When you have a partial you would like to include in an layout, the recommended mechanism is to use the partial [[Tag Helpers|tag helper]].

```html
<partial name="_NavigationPartial" />
```

Place this tag helper where you want the output from the partial to be rendered.

```html
<body>
	<div class="container">
		<header class="card alert-success border-5 p-3 mt-3">Header</header>
		<partial name="_NavigationPartial" />
		<div class="row mt-2">
			<div class="col-3">
```

Partial views help avoid repetition of code, an example is the `_ValidationScriptsPartial.cshtml`, which contains script elements for validating form values. 

This example and the navigation one show static content being used, partials can also **USE DYNAMIC CONTENT**. In the partial file, we specify the structure or nature of the data that will be passed to it by using the `@model` directive.

An example could be that the data for our navigation menu is generated by the parent page and uses a `Dictionary<string, string>`. With this, the partial file would declare the model as the first line:

```csharp
@model Dictionary<string, string>
```

Then, in the layout file, we pass the data to the partial:

```html
<partial name="_NavigationPartial" model="Model.Nav" />

<!-- Or implicitly with for -->
<partial name="_NavigationPartial" for="Nav" />
```
