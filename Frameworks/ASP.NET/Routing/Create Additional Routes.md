When customizing the [[Routing Templates|route template]] through the `@page` directive, the routing system's default template is changed. This change makes it possible to specify additional templates that a page can match.

In the `Program` class, when configuring the `RazorPagesOptions`, we can use the `AddPageRoute` method to register additional routes for a specific page:

```csharp
builder.Services
	.AddRazorPages()
	.AddRazorPagesOptions(options => 
	{
		options.Conventions.AddPageRoute("/Index", "FindMe");
	});
```

The above example shows that the `Index` page has an additional route applied to it so that it can be found at the URL `/FindMe`. There is no limit to the number of additional routes defined for a page and can include [[Routing Parameters|parameters]] and [[Routing Constraints|constraints]]. 

## PageRouteModel Conventions

The `AddPageRoute` method is a convenient way to add a new routing convention to one page at a time. However, this does not scale well when applying new routing conventions to multiple pages.

When using the `AddPageRoute` method, [[ASP.NET]] creates a new `PageRouteModelConvention` and adds that to the `PageConventionCollection`, which is represented by the `RazorPagesOptions Conventions` property. The convention is represented by the `IPageRouteModelConvention` [[Interface|interface]] which can be used to apply new conventions to multiple pages.

**AS WE HAVE ESTABLISHED**, you pass data in [[URL|URLs]] as parameter values, and route templates that incorporate parameters need to be declared explicitly. So, if we want to support adding a route template to multiple pages, create a `PageRouteModelConvention` that then can be applied to any number of pages:

```csharp
public interface IPageRouteModelConvention : IPageConvention
{
	void Apply(PageRouteModel model);
}
```

**TAKE NOTICE** that the `IPageRouteModelConvention` interface, in turn, implements the `IPageConvention` interface. This is a [[Marker Interface|marker interface]], it has no methods and is used as part of the route discovery process to denote the implementing class as one that contains route model conventions that should be applied to the application.

The `PageRouteModel` parameter provides a gateway for applying new conventions, it has a `Selectors` property, which represents a collection of `SelectorModel` objects. 

Each one has an `AttributeRouteModel` property, which in turn, has a `Template` property representing a route template that enables mapping a URL to a particular page:

```csharp
PageRouteModel
	RelativePath: "/Pages/Index.cshtml"
	Selectors: [Count = 3]
		SelectorModel[0]:
			AttributeRouteModel:
				Template: "Index"
		SelectorModel[1]:
			AttributeRouteModel:
				Template: ""
		SelectorModel[2]:
			AttributeRouteModel:
				Template: "FindMe"
```

The `PageRouteModel` class and the classes that make up its properties are a lot more complex than this. There are 3 `SelectorModel` objects in the representation of the Index page with the final containing the _FindMe_ route template.

**WITHIN** the `Apply` method, you can access the existing `SelectorModel` objects and amend the value of the `Template` property to chagne the existing templates, or you can add `SelectorModel` objects to the `Selectors` collection to add additional route templates.

The following example shows a `PageRouteModelConvention` that copies the existing route templates, inserts an optional route parameter called `culture` as the first segment, then adds the copy to a new `SelectorModel` to every page:

```csharp
using Microsoft.AspNetCore.Mvc.ApplicationModels;

namespace CityBreaks.PageRouteModelConventions
{
	public class CultureTemplatePageRouteConvention : IPageRouteModelConvention
	{
		public void Apply(PageRouteModel model)
		{
			var selectorCount = model.Selectors.Count;

			model.Selectors.Add(new SelectorModel
			{
				AttributeRouteModel = new AttributeRouteModel
				{
					Order = 100,
					Template = AttributeRouteModel.CombineTemplates("{culture?}", 
                        selector.AttributeRouteModel.Template)
				}
			});
		}
	}
}
```

**NEXT** we register the `CultureTemplatePageRouteConvention` by adding it to the `RazorPagesOptions Conventions` collection:

```csharp
builder.Services.AddRazorPages().AddRazorPagesOptions(options => {
	options.Conventions.AddPageRoute("/Index", "FindMe");
	options.Conventions.Add(new CultureTemplatePageRouteConvention());
});
```

Once this convention is registered, it is invoked and the `Apply` method is executed for every [[Razor Pages|razor page]] in the application at startup. The result is that the total number of route templates has doubled:

```
"Index"
""
"FindMe"
"{culture?}/Index"
"{culture?}"
"{culture?}/FindMe"
```

**NOTICE** that the `culture` parameter can be null, so we can reach the home page with a culture in the URL and without one. **IF** we wanted to make this mandatory, update the existing template (remove the ?):

```csharp
foreach (var selector in model.Selectors)
{
	selector.AttributeRouteModel.Template = 
                    AttributeRouteModel.CombineTemplates("{culture}", 
	                    selector.AttributeRouteModel.Template);
}
```

