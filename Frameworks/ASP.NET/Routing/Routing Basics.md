# Routing Basics
The routing framework comes as separate components and is plugged in as [[Middleware Pipeline|middleware]]. **TWO KEY COMPONENTS** that control routing are the `EndpointRouting` middleware and the `EndPoint` middleware. Both added to the pipeline via `UseRouting` and `MapRazorPages`.

Here they are listed in the `Program` class:
```csharp
app.UseStaticFiles();
app.UseRouting();
app.UseAuthorization();
app.MapRazorPages();
```

The `EndpointRouting` ( `UseRouting()` ) middleware is used to match incoming URLs to endpoints (Razor pages). Once a match is made, information about the matched endpoint is added to the [[HttpContext Class|HttpContext]], which is then passed along the pipeline. To access this property, the `GetEndpoint` method is provided via the HttpContext.

MIDDLEWARE THAT **DOES NOT RELY** ON ROUTING, like the static file middleware, should be placed before `UseRouting()`.

MIDDLEWARE THAT **DOES RELY** ON ROUTING should be placed between `UseRouting()` and `UseEndpoints` ( `MapRazorPages()` ). An example is the `UseAuthorization()` middleware, it needs to know what endpoint was selected to determine if the current user can access it.