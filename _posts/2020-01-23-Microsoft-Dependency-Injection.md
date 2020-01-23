---
layout: post
title: Is .NET Core Built-in container enough for your DI?
---
### Is .NET Core Built-in container enough for your DI?
One of the first choices you have to make when starting a .NET core project is whether to use the built-in DI container from Microsoft, or to use third-party DI containers for injecting and resolving dependencies in your project.
One of the advantage that the built-in Microsoft dependency injection will gives you, is that the .NET core framework libraries themselves register their dependencies using it. Actually the goal of the built-in container is to provide DI for the framework itself and to keep it simple as possible to integrate other third-party DI containers with it. For small projects o projects that doesn't have much depended services to run, the built-in DI will do the work just fine, but if you need more advanced, convention based registration and injections, then you will need to look for third-party containers like [AutoFac](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html) or [Unity](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection).

But after all, here in this post we gonna talk about Microsoft.Extensions.DependencyInjection library that ships with default ASP.NET core projects, and in .NET core console apps, you need to install it via Nuget. Setting up the container in web projects is pretty straightforward. In Startup.cs file you'll see a method ConfigureServices with a parameter named services (IServiceCollection). You can add your dependencies here, by adding interfaces and implementations classes to the services parameter. 

```cs
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    // Add application services.
    services.AddTransient<IEmailSender, AuthMessageSender>();
    services.AddScoped<IEmailSender, AuthMessageSender>();
    services.AddSingleton<IDbService, DbService>();
}
```
The difference between Add methods are the lifetime. Depending on how the lifetime of an operation service is configured, the container provides the same or different instance of the service when requested by a class.
* ```AddTransient()```: a new instance is provided by the container, every time a controller or service requested it.
* ```AddScoped()```: a new instance created per client request, the operationId of the service is the same in and individual request.
* ```AddSingleton()```: a singleton instance created only once and used across all client requests and services.


