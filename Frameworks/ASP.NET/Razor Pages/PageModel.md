# PageModel

The main purpose of the [[Razor Pages]] PageModel class is to provide a clear separation between the UI (the `.cshtml` _view_ file) and the processing logic for the page.

The PageModel class is automatically generated when you add a new Razor Page to your app. It will be named after the page with the word "Model", example: Welcome page will have a `WelcomeModel`.

PageModel classes are derived from the `PageModel` [[Type System|type]]. This type has a lot of properties and methods to work with [[HTTP]] requests. The public PageModel properties are exposed via the `@model` directive which references the PageModel type.

See Also:
* [[Passing Data to Pages]]
* [[PageModel as a View Model]]
* [[PageModel as a Controller]]



