# PageModel as a Controller (Handler Methods)

When talking about controllers, the thing to remember is that their primary role is to process requests. Our PageModel handles this process by using _**handler methods**_, which are analogous to controller actions in other MVC frameworks.

ASP.NET usings conventions to determine which handler method to execute when a request comes in. This convention matches the name of the HTTP method to the name of the handler method using this pattern:

`On[Method]` with `Async` being an optional addition for methods that run [[Asynchrony|asynchronously]]. So if we had a `OnGet` or `OnGetAsync` method, it would match to [[HTTP Verbs|GET]] requests.

```csharp
public class WelcomeModel : PageModel
{
	public string Message { get; set; }

	public void OnGet()
	{
		Message = "OnGet Executed";
	}

	public void OnPost()
	{
		Message = "OnPost Executed";
	}
}
```

The following shows a Welcome Razor page that includes an anchor tag helper and a form that has a method attribute set to `post`. When you click the link, a GET request is sent, thus triggering our `OnGet` method. Upon submitting the form, our `OnPost`, handler method will trigger due to it being a [[HTTP Verbs|POST]] command.

```html
@page
@model WebApplication1.Pages.WelcomeModel
@{
}

<p>@Model.Message</p>
<a asp-page="/Welcome">Get</a>
<form method="post"><button>Post</button></form>
```

## Handler Method Parameters

When a request is made to our page, data can be passed in the [[URL]] and be bound to handler method parameters. This happens when the parameter's name and the name of the URL data item match.

```csharp
public void OnGet(int id)
{
	Message = $"OnGet executed with id = {id}";
}
```

The `id` is set by using an ASP.NET tag attribute: `asp-route-*`. The `*` is replaced by the name of the data you want to pass to our PageModel.

```html
<a asp-page="/Welcome" asp-route-id="5">Get</a>
```

By **DEFAULT** this data is passed in the query string of the URL request:

```html
localhost:5001/Welcome?id=5
```

## Named Handler Methods

With [[CSharp|C#]], you can create overloads of methods by varying the number of parameters and their [[Type System|type]]. However, the [[Razor Pages]] framework does not allow overloading `OnGet`, `OnPost`, etc.. this applies to the `async` versions as well. When more than one handler matches a request, a `AmbiguousActionException` is thrown at runtime.

This raises the question, how do we implement two `OnGet` or `OnPost` handler methods? As it is common to have two page features that both [[HTTP Verbs|POST]], such as a search form and a registration form. The solution is _**NAMED HANLDER**_ methods.

```csharp
public class WelcomeModel : PageModel
{
	public string Message { get; set; }
	
	public void OnPostSearch(string searchTerm)
	{
		Message = $"You searched for {searchTerm}";
	}

	public void OnPostRegister(string email)
	{
		Message = $"You registered {email} for newsletters";
	}
}
```

```html
@page
@model WelcomeModel
@{
}

<div class="col">
	<form method="post" asp-page-handler="Search">
		<p>Search</p>
		<input name="searchTerm" />
		<button>Search</button>
	</form>
	<form method="post" asp-page-handler="Register">
		<p>Register</p>
		<input name="email" />
		<button>Register</button>
	</form>
	<p>@Model.Message</p>
</div>
```

The query string that gets appended to the URL request looks like `?handler=Search`. This will create a match between the handler query string and the handler method itself. So the Razor Pages framework will select the `OnPostSearch` handler and output the generated message.

## Handler Method Return Types

So far the examples shown have a return type of `void`. There are other supported return types such as `Task`, and any type that implements the `IActionResult` interface. These `IActionResult` types are known as _**ActionResults**_.

The role of **ActionResults** is to generate the response. We can return many different types of responses, such as rendering a Razor page, returning a file, or redirecting a user to a different page. **WHEN WE RETURN `VOID` OR `TASK`**, the Razor Pages framework will implicitly return a `PageResult`, which is an **ActionResult** type that renders the associated Razor page.

An example where we update our `OnPostSearch` handler method to explicitly return a `PageResult`:

```csharp
public PageResult OnPostSearch(string searchTerm)
{
	Message = $"You searched for {searchTerm}";
	return new PageResult();
}
```

**NOTICE** we use the `new` keyword to return a newly instantiated `PageResult` object. The PageModel class also includes helper methods that provide shorthand ways of creating **ActionResults**:

```csharp
public PageResult OnPostSearch(string searchTerm)
{
	Message = $"You searched for {searchTerm}";
	return Page();
}
```

`Page()` is a helper method which wraps around the expression `new PageResult()`. Below is a table of the most common **ActionResult** and their helper methods:

ActionResult Class Name|Helper Method|Description
:--|:--|:--
PageResult|Page|Renders the current Razor page
FileContentResult|File|Returns a file from a byte array, stream or virtual path
NotFoundResult|NotFound|Returns an HTTP 404 status code indicating that the resource was not found
PartialResult|Partial|Renders a partial view or page
RedirectToPageResult|RedirectToPage, RedirectToPagePermanent|Redirects the user to the specified page. The RedirectToPage method returns an HTTP 302 status code, indicating that the redirect is temporary.
StatusCodeResult|StatusCode|Returns an HTTP response with the specified status code

**BE AS SPECIFIC AS POSSIBLE** when choosing the return type of the handler method. 

There are occasions where you will need to return one, two, or more **ActionResult** types. An example would be using a parameter value to look up something in a database, if it doesn't exist, send back a `NotFoundResult`. If it does, return it with a `PageResult`. Since these are two different responses, the return type can be what they both implement, `IActionResult`.

```csharp
public IActionResult OnGet(int id)
{
	var data = database.Find(id);
	if (data == null)
	{
		// wrapper for "new NotFoundResult()"
		return NotFound();
	}
	else
	{
		// wrapper for "new PageResult()"
		return Page();
	}
}
```

