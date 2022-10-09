# PageModel as a View Model

First, a quick run down of a **view model**. It is a class that encapsulates the data required for a particular view or page. A view model, contains a subset of data from one or more objects.

We will use an example to bring this to life, an order summary page on a website. This page would contain a subset of details from various database tables to properly display the needed information to order a certain product.

![Order Summary Fields](https://drek4537l1klr.cloudfront.net/brind/v-10/Figures/03image009.png)

The selected fields will form the basis of our view model, which we will call `OrderSummaryViewModel`:

```csharp
public class OrderSummaryViewModel
{
	public int CustomerId { get; set; }
	public string CustomerName { get; set; }
	public string BillingAddress { get; set; }
	public bool UseBillingForShipping { get; set; }
	public int ProductId { get; set; }
	public string Name { get; set; }
	public decimal Price { get; set; }
}
```

This class our view model, a container for the data required for a view. View models are used extensively in the [[MVC]] framework. The view we want to use this in would include the `@model` directive and reference the view model class we designed for this specific view:

```csharp
@model OrderSummaryViewModel
```

**ANY PUBLIC PROPERTIES** added to the PageModel will be accessible to the Razor page that references the PageModel via the `Model` property within the page.

Here is an example using the Welcome page:

```csharp
public class WelcomeModel : PageModel
{
	public DateTime SaleEnds { get; set; } = new DateTime(DateTime.Now.Year, 6, 30);
	public void OnGet()
	{
	}
}
```

Next is the Welcome Razor page that includes the `@model` directive which references the `WelcomeModel` type, and the `SaleEnds` value being accessed via the page's `Model` property:

```csharp
@page
@model WebApplication1.Pages.WelcomeModel
@{
}
<p>Sale ends at @Model.SaleEnds.ToString("h tt, MMMM dd")</p>
```