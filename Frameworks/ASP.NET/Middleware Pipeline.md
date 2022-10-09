# Middleware Pipeline
[[Middleware]] is added to the [[ASP.NET]] pipeline via the `WebApplication`.  Typically, middleware is created as separate classes that are then registered via [[Extension Methods|extension methods]]. However, it is also possible to add a `RequestDelegate` directly to the pipeline.

See: [[Delegates]]

These `RequestDelegate`'s are methods that match the request method signature, which is as follows:
* A method that takes an `HttpContext` as a parameter and returns a `Task`.

```csharp
async Task TerminalMiddleware(HttpContext contenxt)
{
	await context.Response.WriteAsync("That's all, folks!");
}
```

The above middleware is considered **terminal** or **terminating** middleware, as in, it terminates/short circuits the request and sends a response.

**Terminating middleware is registered with the `APP.RUN()` method.**

To have the middleware pass the context on to the next middleware in the pipeline, a second parameter is required, a `Func` that returns a `Task`. `Func<Task> next` is what the parameter would look like, and it represents the next middleware in the pipeline.

**FUNC: Generic delegate, form is `Func<returnType, inputType, inputType, ... >`**

```csharp
async Task PassThroughMiddleware(HttpContext context, Func<Task> next)
{
	If (context.Request.query.ContainsKey("stop"))
	{
		await context.Response.WriteAsync("Stop the world");
	}
	else
	{
		await next();
	}
}
```

The above middleware is a **PASSTHROUGH** middleware, as in, depending on the route taken, will pass the task on to the next middleware in the pipeline.

**MIDDLEWARE THAT PASSES CONTROL TO THE NEXT COMPONENT IS REGISTERED WITH THE `APP.USE()` METHOD**

Code placed **AFTER** the `await next()` portion of the passthrough middleware will still execute after the task has been handed off to the next middleware, as long as another middleware doesn't short circuit the request.

The reason being, if no other middleware has short circuited the request, then the **RESPONSE** will flow back through this middleware and we can perform additional logic on it, such as logging.