# View Components

Similar to [[Partial Views||partial views]], view components are a more advanced way to generate resusable [[HTML]]. They can be used to break up and simplify complex [[Layout Pages|layouts]], or represent UI elements.

View components are recommended instead of partials when any type of [[Server-Side|server-side]] logic is required to obtain or process data used in the resulting HTML. Examples could be calls to an external resource such as a file, database or web service.