# Изучение Главы 11 книги "Professional ASP.NET MVC 5".

Практическое задание к Главе 11:
- создайте приложение по шаблону ASP.NET MVC без авторизации/аутентификации с поддержкой Web API (соответствующая опция при создании проекта), настройте маршрутизацию для Web API, если она не настроена по умолчанию, создайте модель городов со свойствами Id, Name
- создайте контроллер для обработки Web API запросов на просмотр, добавление, обновление, удаление городов.
- протестируйте GET запросы через браузер, замените XML на JSON формат (результат метода действия JsonResult<T> и метод, генерирующий JSON из объекта Json<T>). POST, PUT, DELETE запросы должны возвращать HTTP код результата, в случае ошибки и текст ошибки (возвращаемый тип HttpResponseMessage и метод, генерирующий результат, например: return Request.CreateErrorResponse(HttpStatusCode.BadRequest, "Data not inserted");). Замените параметр методов POST и PUT на обект класса города, возвращаемый результат замените на объект JSON, например: return Json<string>("Success"); или return Json<T>(new T {});
- протестируйте работу API POST, PUT, DELETE при помощи Google Chrome расширения Postman https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop (выберите тип API, вставьте URL, например DELETE и http://localhost:52459/api/values/5 , во вкладке Headers установите Content-Type в application/json, во вкладке Body установите raw и введите JSON спецификацию, например:
{
    "Name": "Sample"
}
нажмите Send и посмотрите возвращаемый результат, при необходимости получения дополнительной информации обратитесь к справке по Postman)

Контрольные вопросы к Главе 11:
- **В чем отличия ASP.NET Web API от ASP.NET MVC?**
> Web API ships with MVC, and both utilize controllers. However, Web API does not share the
Model-View-Controller design of MVC. They both share the notion of mapping HTTP requests
to controller actions, but rather than MVC’s pattern of using an output template and view engine
to render a result, Web API directly renders the resulting model object as the response. Many of the
design differences between Web API and MVC controllers come from this core difference between
the two frameworks. This section illustrates the basics of writing a Web API controller and actions.

- **Какой базовый класс и из какого пространства имен используется при создании контроллеров API? Какие методы и какие аттрибуты?**
> Contains the ValuesController that you get when you create a new project using the
Web API project template. The fi rst difference you’ll notice is that a new base class is used for all
API controllers: ApiController.
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Web.Http;
namespace WebApiSample.Controllers
{
public class ValuesController : ApiController {
 // GET api/values
 public IEnumerable<string> Get() {
 return new string[] { "value1", "value2" };
 }
 // GET api/values/5
 public string Get(int id) {
 return "value";
 }
 // POST api/values
 public void Post([FromBody] string value) {
 }
 // PUT api/values/5
 public void Put(int id, [FromBody] string value) {
 }
 // DELETE api/values/5
 public void Delete(int id) {
 }
 }
}
```
> The second thing you’ll notice is that the methods in the controller return raw objects rather than
views (or other action results). Instead of returning views composed of HTML, the objects that API
controllers return are transformed into the best matched format that the request asked for. (We’ll
talk a little later on about how that process takes place, as well as the new action results that were
added to Web API 2.)
> The third difference owes to conventional dispatching differences between MVC and Web API.
Whereas MVC controllers always dispatch to actions by name, Web API controllers by default dispatch
to actions by HTTP verb. Although you can use verb override attributes such as `[HttpGet]` or
`[HttpPost]`, most of your verb-based actions will probably follow the pattern of starting the action
name with the verb name. The action methods in the sample controller are named directly after the
verb, but they could also have just started with the verb name (meaning Get and GetValues are
both reachable with the GET verb).
> It’s also worth noting that ApiController is defi ned in the namespace System.Web.Http and not
in System.Web.Mvc where Controller is defi ned. When we discuss self-hosting later, the reason for
this will be clearer.

- **Параметры методов действий Web API контроллера.**
> To accept incoming values from the request, you can put parameters on your action, and just like
MVC, the Web API framework will automatically provide values for those action parameters.
Unlike MVC, there is a strong line drawn between values from the HTTP body and values taken
from other places (like from the URI).
> By default, Web API will assume that parameters that are simple types (that is, the intrinsic types,
strings, dates, times, and anything with a type converter from strings) are taken from non-body values,
and complex types (everything else) are taken from the body. An additional restriction exists as well:
Only a single value can come from the body, and that value must represent the entirety of the body.
> Incoming parameters that are not part of the body are handled by a model binding system that is
similar to the one included in MVC. Incoming and outgoing bodies, on the other hand, are handled
by a brand-new concept called formatters. Both model binding and formatters are covered in more
detail later in this chapter.

- **Результаты методов действий Web API контроллера.**
> Web API 2 introduced a better solution to this problem: the new action result classes. To return
action results, Web API controller actions use a return value type of IHttpActionResult, much like you would with MVC controllers and ActionResult. The ApiController class includes many sets of
methods that directly return action results; their resulting behavior is described in the following list:
➤ BadRequest: Returns an HTTP 400 (“Bad Request”). Optionally includes either
a message or an automatically formatted error class based on validation errors in a
ModelStateDictionary.
➤ Conflict: Returns an HTTP 409 (“Confl ict”).
➤ Content: Returns content (similar to the behavior of an action method that returns a raw
object). Content format is automatically negotiated, or optionally the developer can specify
the media type formatter and/or the content type of the response. The developer chooses
which HTTP status code the response uses.
➤ Created: Returns an HTTP 201 (“Created”). The Location header is set to the provided
URL location.
➤ CreatedAtRoute: Returns an HTTP 201 (“Created”). The Location header is set to the URL
that is constructed based on the provided route name and route values.
➤ InternalServerError: Returns an HTTP 500 (“Internal Server Error”). Optionally includes
content derived from the provided exception.
➤ Json: Returns an HTTP 200 (“OK”), with the provided content formatted as JSON.
Optionally formats the content with the provided serializer settings and/or character
encoding.
➤ NotFound: Returns an HTTP 404 (“Not Found”).
➤ Ok: Returns an HTTP 200 (“OK”). Optionally includes content whose format is automatically
negotiated (to specify the exact format, use the Content method instead).
➤ Redirect: Returns an HTTP 302 (“Found”). The Location header is set to the provided
URL location.
➤ RedirectToRoute: Returns an HTTP 302 (“Found”). The Location header is set to the URL
that is constructed based on the provided route name and route values.
➤ ResponseMessage: Returns the provided HttpResponseMessage.
➤ StatusCode: Returns a response with the provided HTTP status code (and an empty
response body).
➤ Unauthorized: Returns an HTTP 401 (“Unauthorized”). The authentication header is set to
the provided authentication header values.

- **Конфигурация Web API.**
> Web API was designed not to have any such static global values, and instead put its confi guration
into the HttpConfiguration class. This has two impacts on application design:

> ➤ You can run multiple Web API servers in the same application (because each server has its
own non-global confi guration)

> ➤ You can run both unit tests and end-to-end tests more easily in Web API because you contain
that confi guration in a single non-global object, as statics make parallelized testing much
more challenging.
> The configuration class includes access to the following items:

> ➤ Routes

> ➤ Filters to run for all requests

> ➤ Parameter binding rules

> ➤ The default formatters used for reading and writing body content

> ➤ The default services used by Web API

> ➤ A user-provided dependency resolver for DI on services and controllers

> ➤ HTTP message handlers

> ➤ A fl ag for whether to include error details such as stack traces

> ➤ A Properties bag that can hold user-defi ned values

> How you create or get access to this confi guration depends on how you are hosting your application:
inside ASP.NET, WCF self-host, or the new OWIN self-host.

- **Configuration in Web-Hosted Web API.**
> The default MVC project templates are all web-hosted projects because MVC only supports webhosting.
Inside the App_Startup folder are the startup confi guration fi les for your MVC application.
The Web API confi guration code is in WebApiConfig.cs (or .vb), and looks something like this:
```c#
public static class WebApiConfig {
 public static void Register(HttpConfiguration config) {
 // Web API configuration and services
 // Web API routes
 config.Routes.MapHttpAttributeRoutes();
 config.Routes.MapHttpRoute(
 name: "DefaultApi",
 routeTemplate: "api/{controller}/{id}",
 defaults: new { id = RouteParameter.Optional }
 );
 }
}
```
> Developers will make modifi cations to this fi le to refl ect any confi guration changes they want to
make for their application. The default contains a single route as an example to get you started.

- **Configuration in Self-Hosted Web API (общее ознакомление). Configurating WCF Self-Host (общее ознакомление). Configuration for OWIN Self-Host (общее ознакомление).**
> *Configuration in Self-Hosted Web API*
> The two other hosts that ship with Web API are a WCF-based self-host (contained in the assembly
System.Web.Http.SelfHost.dll) and an OWIN-based self-host (contained in the assembly
System.Web.Http.Owin.dll). Both of these self-host options are useful for when you want to
host your APIs outside of a web project (which typically means inside of a console application or a
Windows Service).
> There are no built-in project templates for self-hosting because no limitation exists to the project
type that you might want to use when self-hosting. The simplest way to get Web API running
in your application is to use NuGet to install the appropriate self-host Web API package (either
Microsoft.AspNet.WebApi.SelfHost or Microsoft.AspNet.WebApi.OwinSelfHost). Both packages
include all the System.Net.Http and System.Web.Http dependencies automatically.
> When self-hosting, you are responsible for creating the confi guration and starting and stopping the
Web API server as appropriate. Each self-host system uses a slightly different confi guration system,
as described in the following sections.

> *Confi gurating WCF Self-Host*
> The confi guration class you need to instantiate is HttpSelfHostConfiguration, which extends the
base HttpConfiguration class by requiring a base URL to listen to. After setting up the confi guration,
you create an instance of HttpSelfHostServer, and then tell it to start listening.
Here is a sample snippet of startup code for WCF self-host:
```c#
var config = new HttpSelfHostConfiguration("http://localhost:8080/");
config.Routes.MapHttpRoute(
 name: "DefaultApi",
 routeTemplate: "api/{controller}/{id}",
 defaults: new { id = RouteParameter.Optional }
);
var server = new HttpSelfHostServer(config);
server.OpenAsync().Wait();
```
> You should also shut down the server when you’re done:
```c#
server.CloseAsync().Wait();
```
> If you are self-hosting in a console app, you would probably run this code in your Main function.
For self-hosting in other application types, just fi nd the appropriate place to run application startup
and shutdown code and run these things there. In both cases, the .Wait() call could (and should)
be replaced with an async code (using async and await) if your application development framework
allows you to write an asynchronous startup and shutdown code. 

> *Configuration for OWIN Self-Host*
> OWIN (Open Web Interface for .NET) is a fairly new way to defi ne web applications that helps
isolate the application itself from the hosting and web server that will run the app. In this way, an
application can be written such that it could be hosted inside of IIS, inside of a custom web server,
or even inside of ASP.NET itself.
> Because OWIN is about abstracting the web server from the web application, you also need to
choose a way to connect your app to your web server of choice. The NuGet package Microsoft
.AspNet.WebApi.OwinSelfHost brings in parts of the Katana project to make it easy to self-host
your Web APIs using HttpListener, which has no dependency on IIS.
> Here is an example snippet of code that a console-based OWIN self-host application might use:
```c#
using (WebApp.Start<Startup>("http://localhost:8080/")) {
 Console.WriteLine("Server is running. Press ENTER to quit.");
 Console.ReadLine();
}
```
> Note that this code contains no Web API references; instead, it “starts” another class called
Startup. The defi nition of the Startup class that supports Web API might look something like this:
```c#
using System;
using System.Linq;
using System.Web.Http;
using Owin;
class Startup {
 public void Configuration(IAppBuilder app) {
 var config = new HttpConfiguration();
 config.Routes.MapHttpRoute(
 name: "DefaultApi",
 routeTemplate: "api/{controller}/{id}",
 defaults: new { id = RouteParameter.Optional }
 );
 app.UseWebApi(config);
 }
}
```
> The Startup class in an OWIN application conceptually replaces the WebApiConfig class that’s
used in web-hosted applications. The IAppBuilder type from OWIN allows you to confi gure the
application that will run; here you use the UseWebApi extension method that the Web API OWIN
Self Host package provides to confi gure OWIN.

- **Добавление маршрутов. Маршрутизация посредством аттрибутов.**
>  Web API’s primary route registration is the MapHttpRoute
extension method. As is the case for all Web API confi guration tasks, the routes for your application
are confi gured off the HttpConfiguration object.
> If you peek into the confi guration object, you’ll discover that the Routes property points to an
instance of the HttpRouteCollection class rather than ASP.NET’s RouteCollection class. Web
API offers several versions of MapHttpRoute that work against the ASP.NET RouteCollection
class directly, but such routes are only usable when web-hosted, so we recommend (and the project
templates encourage) that you use the versions of MapHttpRoute on HttpRouteCollection.

> The attribute-based routing feature that was introduced in MVC 5 is also
available to your Web API 2 applications. To enable attribute routing for your
Web API controllers, add the following line of code to your Web API startup
code, before any of your hand-confi gured routes:
```c#
config.MapHttpAttributeRoutes();
```

> The routing system in Web API uses the same routing logic that MVC uses to help determine which
URIs should route to the application’s API controllers, so the concepts you know from MVC apply
to Web API, including the route matching patterns, defaults, and constraints. To keep Web API from
having any hard dependencies on ASP.NET, the team took a copy of the routing code from
ASP.NET and ported it to Web API. The way this code behaves changes slightly depending on
your hosting environment.
> When running in the self-hosted environment, Web API uses its own private copy of the routing
code, ported from ASP.NET into Web API. Routes in Web API will look much the same as those in
MVC, but with slightly different class names (HttpRoute versus Route, for example).
When your application is web-hosted, Web API uses ASP.NET’s built-in routing engine because it’s
already hooked into the ASP.NET request pipeline. When registering routes in a web-hosted environment,
the system will not only register your HttpRoute objects, but it will also automatically create
wrapper Route objects and register them in the ASP.NET routing engine. The major difference between
self-hosting and web-hosting is when routing is run; for web-hostin, routing is run fairly early (by
ASP.NET), whereas in the self-host scenario, routing is fairly late (by Web API). If you are writing a
message handler, it’s important to note that you might not have access to routing information, because
routing might not yet have been run.
> The most signifi cant difference between the default MVC route and the default Web API route is
the lack of the {action} token in the latter. As discussed earlier, Web API actions are dispatched to
by default based on the HTTP verb that the request used. However, you can override this mapping 
by using the {action} matching token in the route (or by adding an action value to the default
values for the route). When the route contains an action value, Web API uses that action name to
fi nd the appropriate action method.
> Even when using action name -based routing, the default verb mappings do still apply; that is,
if the action name starts with one of the well-known verb names (Get, Post, Put, Delete,
Head, Patch, and Options), then it’s matched to that verb. For all the actions whose names don’t
match one of the well-known verbs, the default supported verb is POST. You should decorate your
actions using the `[Http...]` family of attributes (`[HttpDelete]`, `[HttpGet]`, `[HttpHead]`,
`[HttpOptions]`, `[HttpPatch]`, `[HttpPost]` and `[HttpPut]`) or the `[AcceptVerb]` attribute to
indicate what verb(s) should be allowed when the default conventions aren’t correct.

- **Привязка параметров, аттрибуты.**
> Web API uses parameter binders to determine how to provide values for individual
parameters. You can use attributes to infl uence that decision (such as `[ModelBinder]`, an
attribute we’ve seen before with MVC), but the default logic uses the simple type versus complex
type logic when there are no overrides applied to infl uence the binding decision.
> The Parameter Binding system looks to the action’s parameters to fi nd any attributes that derive
from ParameterBindingAttribute. The following list shows a few such attributes that are built
into Web API. In addition, you can register custom parameter binders that do not use model
binding or formatters, either by registering them in the confi guration or by writing your own
ParameterBindingAttribute-based attributes.

> ➤ ModelBinderAttribute: This tells the parameter binding system to use model binding
(meaning, create the value through the use of any registered model binders and value providers).
This is what is implied by the default binding logic for any parameter of a simple type.

> ➤ FromUriAttribute: This is a specialization of ModelBindingAttribute that tells the system
only to use value providers from factories, which implement IUriValueProviderFactory
to limit the values bound to ensure that they come only from URI. Out of the box, the route
data and query string value providers in Web API implement this interface.

> ➤ FromBodyAttribute: This tells the parameter binding system to use formatters (meaning,
create the value by fi nding an implementation of MediaTypeFormatter, which can decode
the body and create the given type from the decoded body data). This is what is implied by
the default binding logic for any complex type.

> The parameter binding system is quite different from the way MVC works. In MVC, all parameters
are created through model binding. Model binding in Web API works mostly the same way as MVC
(model binders and providers, and value providers and factories), although it’s been re-factored quite
a bit, based on the alternate model binding system from MVC Futures. You will fi nd built-in model
binders for arrays, collections, dictionaries, simple types, and yes, even complex types (though you
would need to use `[ModelBinder]` to get them to run, obviously). Although the interfaces have
changed slightly, if you know how to write a model binder or value provider in MVC, you’ll be right
at home doing the same thing for Web API.
> Formatters are a new concept for Web API. Formatters are responsible for both consuming and producing
body content. You can think of them in much the same way you might think of serializers in
.NET: classes that are responsible for encoding and decoding custom complex types into and out of
the stream of bytes, which is the body content. You can encode exactly one object into the body, and
decode exactly one object back out of the body (although that object can contain nested objects, as you
would expect of any complex type in .NET).
> Built into Web API you will fi nd three formatters, one which:

> ➤ Encodes and decodes JSON (using Json.NET)

> ➤ Encodes and decodes XML (using either DataContractSerializer or XmlSerializer)

> ➤ Decodes form URL encoded from data in the body from a browser form post.

> Each of these formatters is quite powerful, and will make its best effort to transcode its supported
format into the class of your choosing.

- **Фильтрование запросов.**
> The ability to fi lter requests with attributes has been in ASP.NET MVC since version 1.0, and
the ability to add global fi lters was added in MVC 3. ASP.NET Web API includes both features,
although as discussed previously, the fi lter is global at the confi guration level, not at the application
level (as no such application-wide global features exist in Web API).
> One of the improvements in Web API over MVC is that fi lters are now part of the asynchronous
pipeline, and are by defi nition always async. If a fi lter could benefi t from being asynchronous—for
example, logging exception failures to an asynchronous data source such as a database or the fi le
system—then it can do so. However, the Web API team also realized that sometimes being forced to
write asynchronous code is unnecessary overhead, so they also created synchronous attribute-based
base class implementations of the three fi lter interfaces. When porting MVC fi lters, using these base
classes is probably the simplest way to get started. If a fi lter needs to implement more than one stage
of the fi lter pipeline (such as action fi ltering and exception fi ltering), there are no helper base classes
and the interfaces need to be implemented explicitly.
> Developers can apply fi lters at the action level (for a single action), at the controller level (for all
actions in the controller), and at the confi guration level (for all actions on all controllers in the confi
guration). Web API includes one fi lter in the box for developers to use, AuthorizeAttribute. Much
like its MVC counterpart, this attribute is used to decorate actions that require authorization, and
includes the AllowAnonymousAttribute, which can selectively “undo” the AuthorizeAttribute.
The Web API team also released an out-of-band NuGet package to support several OData-related
features, including QueryableAttribute, which can automatically support OData query syntax
(such as the $top and $filter query string values).

> ➤ IAuthenticationFilter: Authentication fi lters identify the user making the request. In previous
versions of ASP.NET MVC and Web API, authentication was not easily pluggable; you
either depended on built-in behavior from your web server, or you co-opted another fi lter
stage like authorization. Authentication fi lters run before authorization fi lters.

> ➤ IAuthorizationFilter / AuthorizationFilterAttribute: Authorization fi lters run
before any parameter binding has happened. They are intended to fi lter out requests that do
not have the proper authorization for the action in question. Authorization fi lters run before
action fi lters.

> ➤ IActionFilter / ActionFilterAttribute: Action fi lters run after parameter binding
has happened and wrap the call to the API action method, allowing interception before the
action has been dispatched to and after it is done executing. They are intended to allow
developers to either augment and/or replace the incoming values and/or outgoing results of
the action.

> ➤ IExceptionFilter / ExceptionFilterAttribute: Exception fi lters are called when calling
the action resulted in an exception being thrown. Exception fi lters can inspect the exception
and take some action (for example, logging); they can also opt to handle the exception by providing
a new response object.

> You have no equivalent to the MVC HandleError attribute in Web API. When an MVC application
encounters an error, its default behavior is to return the ASP.NET “yellow screen of death.”
This is appropriate (if not entirely user friendly) when your application is generating HTML. The
HandleError attribute allows MVC developers to replace that behavior with a custom view. Web
API, on the other hand, should always attempt to return structured data, including when error conditions
occur, so it has built-in support for serializing errors back to the end user. Developers who
want to override this behavior can write their own error handler fi lter and register it at the confi guration
level.

- **Enabling Dependency Injection (общее ознакомление).**
> ASP.NET MVC 3 introduced limited support for dependency injection containers to provide both
built-in MVC services and the ability to be the factory for non-service classes such as controllers and
views. Web API has followed suit with similar functionality, with two critical differences.
> First, MVC used several static classes as the container for the default services consumed by MVC.
Web API’s confi guration object replaces the need for these static classes, so the developer can inspect
and modify this default service listed by accessing HttpConfiguration.Services.
> Second, Web API’s dependency resolver has introduced the notion of “scopes.” A scope can be thought
of as a way for a dependency injection container to keep track of the objects that it has allocated
in some particular context so that they can be easily cleaned up all at once. Web API’s dependency
resolver uses two scopes:

> ➤ A per-confi guration scope—For services global to the confi guration, cleaned up when the
confi guration is disposed

> ➤ A request-local scope—For services created in the context of a given request, such as those
consumed by a controller, and cleaned up when the request is completed

- **Exploring API's programmatically (общее ознакомление).**
> An MVC application’s controllers and actions are usually a fairly ad-hoc affair, designed solely to
suit the display of HTML in the application. Web APIs, on the other hand, tend to be more ordered
and planned. Offering the ability to discover APIs at run time enables developers to provide key
functionality along with their Web API applications, including things like automatically generated
help pages and testing client UI.
> Developers can acquire the IApiExplorer service from HttpConfiguration.Services and use
it to programmatically explore the APIs exposed by the service. For example, an MVC controller
could return the IApiExplorer instance from Web API to this snippet of Razor code to list all the
available API endpoints.

>> GET api/Values

>> GET api/Values/{id}
>>  Parameters
>>      id (FromUri)

>> POST api/Values
>>  Parameters    
>>      value (FromBody)

>> PUT api/Values/{id}
>>  Parameters
>>      id (FromUri)
>>      value (FromBody)

>> DELETE api/Values/{id}
>> Parameters
>>      id (FromUri)

```c#
@model System.Web.Http.Description.IApiExplorer
@foreach (var api in Model.ApiDescriptions) {
 <h1>@api.HttpMethod @api.RelativePath</h1>
 if (api.ParameterDescriptions.Any()) {
 <h2>Parameters</h2>
 <ul>
 @foreach (var param in api.ParameterDescriptions) {
 <li>@param.Name (@param.Source)</li>
 }
 </ul>
 }
}
```
> In addition to the automatically discoverable information, developers can implement the
IDocumentationProvider interface to supplement the API descriptions with documentation text,
which could be used to offer richer documentation and test client functionality. Because the documentation
is pluggable, developers can choose to store the documentation in whatever form is convenient,
including attributes, stand-alone fi les, database tables, resources, or whatever best suits the application
build process.
> For a more complete example of what’s possible with these APIs, you can install the Microsoft
.AspNet.WebApi.HelpPage Nuget package into a project with both MVC and Web API. This package
is a good starting point for developers who want to ship automated documentation along with
their web APIs.

- **Tracing the Application (общее ознакомление).**
> One of the most challenging things with remotely deployed code is debugging when something has
gone wrong. Web API enables a very rich automatic tracing ecosystem that is turned off by default
but can be enabled by the developer as needed. The built-in tracing functionality wraps many of the
built-in components and can correlate data from individual requests as it moves throughout the layers
of the system.
> The central part of tracing is the ITraceWriter service. Web API does not ship with any implementations
of this service because it is anticipated that developers will likely already have their own favorite
tracing system (such as ETW, log4net, ELMAH, or many others). Instead, on startup Web API checks
whether an implementation of ITraceWriter is available in the service list, and if so, automatically
begins tracing all requests. The developer must choose how best to store and browse this trace information—typically,
by using the confi guration options provided by their chosen logging system.
> Application and component developers can also add tracing support to their systems by retrieving the
ITraceWriter service and, if it’s not null, writing tracing information to it. The core ITraceWriter
interface only contains a single Trace method, but several extension methods are designed to make
tracing different levels of messages (debug, info, warning, error, and fatal messages) easy. You also
have helpers to trace entry and exit to both synchronous and asynchronous methods.
