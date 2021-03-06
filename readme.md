# Effective logging

## What is it?

- Good, consistent information
- Keeps application code as clean as possible
- Easily consumable
- Allows for easy code fixes
- Enables deeper understanding
- Adds data for prioritization

## Logging - high level overview

### Logging Levels

- `Trace`: the lowest, most verbose level. You will not use this one.
- `Debug`: provides ample data to troubleshoot an issue. Use only for purpose of troubleshooting.
- `Warning`: indicates a more localized error happened, but is not vital and the app can continue working. E.g. an unexpected value or an exception we handled.
- `Error`: unhandled exception. Typical log level for an error.
- `Fatal`: A very specific type of error happened. Indicates complete app failure. E.g. the database is not available.

### Categories

- Usually a fully qualified class name, along with the namespace, but we can define our own using `LoggerFactory`.

### Scopes

- Allow grouping your log entries. All entries within a scope have a specific string associated with them.

### Filters

- A logging level is assigned to each category. Usually in `appsettings.json`.
- Serilog itself provides its own filtering, so keep that in mind.

## ILogger Method Parameters

- `EventId` - struct of `Id` and `Name`.
- `Exception` - will log `.ToString()`, so override on your own exceptions if necessary.
- `Message` - formatted string.
- `Args` - values for formatted string placeholder items.

## Logical Grouping

- `Category`: by using `ILogger<T>`, the `T` is logged as `SourceContext`. Custom `SourceContext` can be provided by using `LoggerFactory`.
- `EventId`: optional. Logged as `{"id": "1", "name": "Foo"}`.
- Scopes: span classes and assemblies.

## High-performance Logging

- Use `LoggerMessage`. Examples were given for tracking perfomance using `Stopwatch` class.

## Automatic logging

### Global Exception Handler

- Do not use `DeveloperExceptionPage` middleware. It is a development-only tool that makes your app different in development from production.
- Logger should log the exception.
- Assign RequestId and make the API return it, for user's reference.
- Implement a custom middleware: reference `StackUnderflow.API.Middlewares.ApiExceptionHandlerMiddleware`.

### Calling API via HttpClient

- A separate error handler isneeded. Find `StandardHttpMessageHandler` in "Effective logging for ASP.NET Core" course source code.

### Filters

- Globally applied, in `Startup.cs`.
- Create a custom class implementing either `IActionFilter` or `IAsyncActionFilter`.

### Attributes and Base Classes

- Locally applied, per class, controller or action.
- Your options are:
  - Attributes: `ActionFilterAttribute` and `ResultFilterAttribute`, both per controller or action.
  - Custom base classes.
