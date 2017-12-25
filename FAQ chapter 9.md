# Изучение Главы 9 книги "Professional ASP.NET MVC 5".

В процессе изучения главы упоминаются Регулярные выражения -  https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%B3%D1%83%D0%BB%D1%8F%D1%80%D0%BD%D1%8B%D0%B5_%D0%B2%D1%8B%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F

Практическое задание к Главе 9:
- создайте контроллер, настройте маршрутизацию при помощи аттрибутов, добавьте действие обращение к которому начинается со слова date, например http://localhost:50330/date и который URL которого имеет три параметра: year, month, day, причем year и day числа, а длина month от 3 до 10
- создайте еще один контроллер, настройте маршрутизацию традиционным способом, назначив имя маршруту, обращение к контроллеру должно начинаться со слова site, должен быть необязательный URL параметр id
- установите и настройте Route Debugger, протестируйте текущие маршруты
- исключите из маршрутизации путь, начинающийся со слова ignore и имеющий три параметра: year, month, day, протестируйте обращение по такому пути
- добавьте маршрут, который начинается со слова site, имеет два параметра: first, second, которые могут состоять из букв, цифр, и знака подчеркивания, длиной 3 символа, а также в конце пути иметь дополнительное неограниченное число URL параметров
- добавьте область, контроллер и представление в область и сконфигурируйте маршрутизацию при помощи аттрибутов
- создайте представление, которое будет выводить ссылку на один из именованных маршрутов, созданных ранее

Контрольные вопросы к Главе 9:
- **Каковы предписания к URL с точки зрения удобства использования**
> Usability expert Jakob Nielsen (www.useit.com) urges developers to pay attention to URLs and
provides the following guidelines for high-quality URLs. You should provide

> ➤ A domain name that is easy to remember and easy to spell

> ➤ Short URLs

> ➤ Easy-to-type URLs

> ➤ URLs that refl ect the site structure

> ➤ URLs that are hackable to allow users to move to higher levels of the information architecture
by hacking off the end of the URL

> ➤ Persistent URLs, which don’t change

- **Что такое маршрутизация и подходы к маршрутизации в ASP.NET MVC?**
> Routing within the ASP.NET MVC framework serves two main purposes:

> ➤ It matches incoming requests that would not otherwise match a file on the file system and it
maps the requests to a controller action.

> ➤ It constructs outgoing URLs that correspond to controller actions.

> The URL rewriting is focused on mapping one URL to another URL. For
example, URL rewriting is often used for mapping old sets of URLs to a new set of URLs. Contrast
that to Routing, which is focused on mapping a URL to a resource.
> You might say that Routing embodies a resource-centric view of URLs. In this case, the URL represents
a resource (not necessarily a page) on the Web. With ASP.NET Routing, this resource is a piece
of code that executes when the incoming request matches the route. The route determines how the
request is dispatched based on the characteristics of the URL—it doesn’t rewrite the URL.
> Another key difference is that Routing also helps generate URLs using the same mapping rules that
it uses to match incoming URLs. URL rewriting applies only to incoming requests and does not help
in generating the original URL.

> MVC has always supported a centralized, imperative, code-based style of defi ning routes that we’ll call
traditional routing. This is a good option that is still fully supported. However, MVC 5 adds a
second option using declarative attributes on your controller classes and action methods, which is
called attribute routing. This new option is simpler and keeps your route URLs together with your
controller code. Both options work well, and both are fl exible enough to handle complex routing
scenarios.

- **Маршрутизация посредством аттрибутов, параметры маршрутизации, аттрибут маршрутизации контроллера, префиксы и отступление от префикса в определенном действии, что значит [Route] (без параметра), route constraints, route defaults, необязательные параметры маршрутизации.**

> **Маршрутизация посредством аттрибутов**

> After you create a new ASP.NET MVC Web Application project, take a quick look at the code
in Global.asax.cs. You’ll notice that the Application_Start method contains a call to the
RegisterRoutes method. This method is the central control point for your routes and is located in
the ~/App_Start/RouteConfig.cs fi le. Because you’re starting with attribute routes, you’ll clear
out everything in the RegisterRoutes method for now and just have it enable attribute routing by
calling the MapMvcAttributeRoutes registration method. When you’re done, your RegisterRoutes
method should look like this:
```c#
public static void RegisterRoutes(RouteCollection routes)
{
routes.MapMvcAttributeRoutes();
}
```
> Now you’re ready to write your fi rst route. At its core, a route’s job is to map a request to an action.
The easiest way to do this is using an attribute directly on an action method:
```c#
public class HomeController : Controller
{
[Route("about")]
public ActionResult About()
{
return View();
}
}
```
> This route attribute will run your About method any time a request comes in with /about as the
URL. You tell MVC which URL you’re using, and MVC runs your code. It doesn’t get much simpler
than this.
> If you have more than one URL for your action, you use multiple route attributes. For example, you
might want your home page to be accessible through the URLs /, /home, and /home/index. Those
routes would look like this:
```c#
[Route("")]
[Route("home")]
[Route("home/index")]
public ActionResult Index()
{
return View();
}
```
> The string you pass in to the route attribute is called the route template, which is a pattern-matching
rule that determines whether this route applies to an incoming request. If the route matches,
MVC will run its action method. In the preceding routes, you’ve used static values like about or
home/index as your route templates, and your route will match only when the URL path has that
exact string. They might look quite simple, but static routes like these can actually handle quite a bit
of your application.

> **параметры маршрутизации**
> Forexample, if your action shows the details for a person record, you might want to include the record
ID in your URL. That’s solved by adding a route parameter:
```c#
[Route("person/{id}")]
public ActionResult Details(int id)
{
// Do some work
return View();
}
```
> Putting curly braces around id creates a placeholder for some text that you want to reference by
name later. To be precise, you’re capturing a path segment, which is one of part of a URL path separated
by slashes (but not including the slashes). To see how that works, let’s defi ne a route like this:
```c#
[Route("{year}/{month}/{day}")]
public ActionResult Index(string year, string month, string day)
{
// Do some work
return View();
}
```
> You can name these parameters almost anything you want (alphanumeric characters are allowed
as well as a few other characters). When a request comes in, Routing parses the request URL and
places the route parameter values into a dictionary (specifi cally a RouteValueDictionary accessible
via the RequestContext), using the route parameter names as the keys and the corresponding subsections
of the URL (based on position) as the values.
> When an attribute route matches and an action method runs, the route parameters from the route
are used by model binding to populate values for any method parameters with the same name.
Later, you’ll learn about how route parameters are different from method parameters.

> **аттрибут маршрутизации контроллера**

> Wouldn’t it be nice to fi nd
some way to avoid repeating yourself and just say that each action method maps to a URL under
home? Fortunately, you can:
```c#
[Route("home/{action}")]
public class HomeController : Controller
{
public ActionResult Index()
{
return View();
}
public ActionResult About()
{
return View();
}
public ActionResult Contact()
{
return View();
}
}
```
> When you define a route on the controller class, you can use a special route
parameter named action, and it serves as a placeholder for any action name. It has the same effect
as your putting separate routes on each action and typing in the action name statically; it’s just a
more convenient syntax. You can have multiple route attributes on your controller class just like you
do on your action methods.

> Often, some actions on a controller will have slightly different routes from all the others. In that
case, you can put the most common routes on the controller and then override these defaults on the
actions with different route patterns. For example, maybe you think /home/index is too verbose
and you want to support /home as well. You could do that as follows:
```c#
[Route("home/{action}")]
public class HomeController : Controller
{
[Route("home")]
[Route("home/index")]
public ActionResult Index()
{
return View();
}
public ActionResult About()
{
return View();
}
public ActionResult Contact()
{
return View();
}
}
```

> **префиксы и отступление от префикса в определенном действии**

> Every route begins with home/ (the class is named
HomeController, after all). You can say that just once, using RoutePrefix:
```c#
[RoutePrefix("home")]
[Route("{action}")]
public class HomeController : Controller
{
[Route("")]
[Route("index")]
public ActionResult Index()
{
return View();
}
public ActionResult About()
{
return View();
}
public ActionResult Contact()
{
return View();
}
}
```
> Now, all your route attributes can omit home/ because the prefi x provides that automatically. The
prefi x is just a default, and you can escape from it if necessary. For example, you might want your
home controller to support the URL / in addition to /home and /home/index. To do that, just
begin the route template with ~/, and the route prefi x will be ignored. Here’s how it looks when
HomeController supports all three URLs for the Index method (/, /home, and /home/index):
```c#
[RoutePrefix("home")]
[Route("{action}")]
public class HomeController : Controller
{
[Route("~/")]
[Route("")] // You can shorten this to [Route] if you prefer.
[Route("index")]
public ActionResult Index()
{
return View();
}
public ActionResult About()
{
return View();
}
public ActionResult Contact()
{
return View();
}
}
```

> **что значит [Route] (без параметра)**

> 

> **Route constraints.**

> A route constraint is a condition that must be satisfied for the route to match. In this case, you just
need a simple int constraint:
```c#
[Route("person/{id:int}")]
public ActionResult Details(int id)
{
// Do some work
return View();
}
[Route("person/{name}")]
public ActionResult Details(string name)
{
// Do some work
return View();
}
```
> Note the key difference here: instead of defi ning the route parameter as just {id}, you’ve now
defi ned it as {id:int}. Putting a constraint in the route template like this is called an inline
constraint, and a number of them are available.

NAME      | EXAMPLE          | USAGE DESCRIPTION
----------|------------------|-------------------
bool      | {n:bool}         | A Boolean value
datetime  | {n:datetime}     | A DateTime value
decimal   | {n:decimal}      | A Decimal value
double    | {n:double}       | A Double value
float     | {n:float}        | A Single value
guid      | {n:guid}         | A Guid value
int       | {n:int}          | An Int32 value
long      | {n:long}         | An Int64 value
minlength | {n:minlength(2)} | A String value with at least two characters
maxlength | {n:maxlength(2)} | A String value with no more than two characters
length    | {n:length(2)}    | A String value with exactly two characters
length    | {n:length(2,4)}  | A String value with two, three, or four characters
min       | {n:min(1)}       | An Int64 value that is greater than or equal to 1
max       | {n:max(3)}       | An Int64 value that is less than or equal to 3
range     | {n:range(1,3)}   | The Int64 value 1, 2, or 3
alpha     | {n:alpha}        | A String value containing only the A–Z and a–z characters
regex     | {n:regex (^a+$)} | A String value containing only one or more 'a' characters (a Regex match for the ^a+$ pattern)

> Inline route constraints give you fi ne-grained control over when a route matches. If you have URLs
that look similar but behave differently, route constraints give you the power to express the difference
between these URLs and map them to the correct action.


> **Route defaults.**

> The Routing API allows you
to supply default values for parameters. For example, you can defi ne the route like this:
```c#
[Route("home/{action=Index}")]
```
> The `{action=Index}` snippet defines a default value for the `{action}` parameter. This default
allows this route to match requests when the action parameter is missing. In other words, this route
now matches any URL with one or two segments instead of matching only two-segment URLs. This
now allows you to call the Index action method, using the URL /home, which satisfies your goal.

> **Необязательные параметры маршрутизации.**

> You can use one route and make id optional:
```c#
[RoutePrefix("contacts")]
[Route("{action}/{id?}")]
public class ContactsController : Controller
{
    public ActionResult Index()
    {
        // Show a list of contacts
        return View();
    }
    public ActionResult Details(int id)
    {
        // Show the details for the contact with this id
        return View();
    }
    public ActionResult Update(int id)
    {
        // Display a form to update the contact with this id
        return View();
    }
    public ActionResult Delete(int id)
    {
        // Delete the contact with this id
        return View();
    }
}
```
> An optional route parameter is a special case of a default value. From a Routing standpoint, whether
you mark a parameter as optional or list a default value doesn’t make a lot of difference; in both
cases, the route actually has a default value. An optional parameter just has the special default value
UrlParameter.Optional.

> Suppose you had the following two
routes defi ned, the fi rst one containing a default value for the {action} parameter:
```c#
[Route("contacts/{action=Index}/{id}")]
[Route("contacts/{action}/{id?}")]
```
> In this example, an ambiguity exists about which route the request should match. To avoid these
types of ambiguities, the Routing engine only uses a particular default value when every subsequent
value UrlParameter.Optional). In this example, if you have a default value for {action} you
should also provide a default value for {id} (or make it optional).

> It turns out that with Routing, any path segment (the portion of the URL between two slashes) with
literal values must have a match for each of the route parameter values when matching the request
URL.

- **Традиционная маршрутизация в ASP.NET MVC (не посредством аттрибутов), Route Defaults, Route Constraints, ordering route definitions,**
> 

> *Route Defaults*
> With an attribute route, you would make the {id} parameter optional by changing it to {id?}
inline in the route template. Traditional routing takes a different approach. Instead of putting
this information inline as part of the route template, traditional routing puts it in a separate
argument after the route template. To make {id} optional in traditional routing, you can defi ne
the route like this:
```c#
routes.MapRoute("simple", "{controller}/{action}/{id}",
new {id = UrlParameter.Optional});
```
> The third parameter to MapRoute is for default values. The {id = UrlParameter.Optional} snippet
defi nes a default value for the {id} parameter. Unlike attribute routing, the relationship between
optional and default values is obvious here. An optional parameter is simply a parameter with the
special default value of UrlParameter.Optional, and that’s exactly how it’s specifi ed in a traditional
route defi nition.
> This now allows you to call the List action method, using the URL /albums/list, which satisfi es
your goal. As in attribute routing, you can also provide multiple default values. The following snippet
demonstrates adding a default value for the {action} parameter:
```c#
routes.MapRoute("simple",
"{controller}/{action}/{id}",
new { id = UrlParameter.Optional, action = "index" });
```
> Attribute routing would have placed this default inline, using the syntax {action=Index}. Once
again, traditional routing uses a different style. You specify default and optional values in a separate
argument used just for this purpose.
> The earlier example supplies a default value for the {action} parameter within the URL via the
Defaults dictionary property of the Route class. Typically the URL pattern of {controller}/
{action} would require a two-segment URL in order to be a match. But by supplying a default
value for the second parameter, this route no longer requires that the URL contain two segments to
be a match. The URL may now simply contain the {controller} parameter and omit the {action}
parameter to match this route. In that case, the {action} value is supplied via the default value
rather than the incoming URL. Though the syntax is different, the functionality provided by default
values works exactly as it did with attribute routing.
> Let’s revisit the Table 9-3 on route URL patterns and what they match, and now throw defaults into
the mix as shown in the following examples:
```c#
routes.MapRoute("defaults1",
"{controller}/{action}/{id}",
new {id = UrlParameter.Optional});
routes.MapRoute("defaults2",
"{controller}/{action}/{id}",
new {controller = "home",
action = "index",
id = UrlParameter.Optional});
```
> The defaults1 route matches the following URLs:
```
/albums/display/123
/albums/display
```
> The defaults2 route matches the following URLs:
```
/albums/display/123
/albums/display
/albums
/
```
> Default values even allow you to map URLs that don’t include controller or action parameters at
all in the route template. For example, the following route has no parameters at all; instead, the controller
and action parameters are provided to MVC by using defaults:
```c#
routes.MapRoute("static",
"welcome",
new { controller = "Home", action = "index" });
```
> Just like with attribute routing, remember that the position of a default value relative to other route
parameters is important. For example, given the URL pattern {controller}/{action}/{id}, providing
a default value for {action} without specifying a default for {id} is effectively the same as
not having a default value for {action}. Unless both parameters have a default value, a potential
ambiguity exists, so Routing will ignore the default value on the {action} parameter. When you
specify a default value for one parameter, make sure you also specify default values for any parameters
following it, or your default value will largely be ignored. In this case, the default value only
comes into play when generating URLs, which is covered later in the section “Inside Routing: How
Routes Generate URLs.”

> *Route Constraints*
> 

- **Сочетание маршрутизации посредством аттрибутов и традиционной маршрутизации, выбор между двумя подходамик маршрутизации**
> *Combining Attribute Routing with Traditional Routing*
> Now you’ve seen both attribute routes and traditional routes. Both support route templates, constraints,
optional values, and defaults. The syntax is a little different, but the functionality they offer
is largely equivalent, because under the hood both use the same Routing system.
> You can use either attribute routing, traditional routing, or both. To use attribute routing, you need
to have the following line in your RegisterRoutes method (where traditional routes live):
`routes.MapMvcAttributeRoutes();`
> Think of this line as adding an über-route that contains all the route attributes inside it. Just like any
other route, the position of this über-route compared to other routes makes a difference. Routing checks
each route in order and chooses the fi rst route that matches. If there’s any overlap between a traditional
route and an attribute route, the fi rst route registered wins. In practice, we recommend putting the
MapMvcAttributeRoutes call fi rst. Attribute routes are usually more specifi c, and having attribute
routes come fi rst allows them to take precedence over traditional routes, which are usually more generic.
> Suppose you have an existing application that uses traditional routing, and you want to add a new
controller to it that uses attribute routing. That’s pretty easy to do:
```c#
routes.MapMvcAttributeRoutes();
routes.MapRoute("simple",
"{controller}/{action}/{id}",
new { action = "index", id = UrlParameter.Optional});
// Existing class
public class HomeController : Controller
{
public ActionResult Index()
{
return View();
}
public ActionResult About()
{
return View();
}
public ActionResult Contact()
{
return View();
}
}
[RoutePrefix("contacts")]
[Route("{action=Index}/{id?}")]
public class NewContactsController : Controller
{
public ActionResult Index()
{
// Do some work
return View();
}
public ActionResult Details(int id)
{
// Do some work
return View();
}
public ActionResult Update(int id)
{
// Do some work
return View();
}
public ActionResult Delete(int id)
{
// Delete the contact with this id
return View();
}
}
```

> *Choosing Attribute Routes or Traditional Routes*
> Should you use attribute routes or traditional routes? Either option is reasonable, but here are some
suggestions on when to use each one.
> Consider choosing traditional routes when:

> ➤ You want centralized confi guration of all your routes.

> ➤ You use custom constraint objects.

> ➤ You have an existing working application you don’t want to change.

> Consider choosing attribute routes when:

> ➤ You want to keep your routes together with your action’s code.

> ➤ You are creating a new application or making signifi cant changes to an existing one.

> The centralized confi guration of traditional routes means there’s one place to go to understand how
a request maps to an action. Traditional routes also have some more fl exibility than attribute routes.
For example, adding a custom constraint object to a traditional route is easy. Attributes in C# only
support certain kinds of arguments, and for attribute routing, that means everything is specifi ed in
the route template string.
> On the other hand, attribute routing nicely keeps everything about your controllers together, including
both the URLs they use and the actions that run. I tend to prefer attribute routing for that
reason. Fortunately, you can use both and moving a route from one style to the other if you change
your mind is not difficult.

- **Именованные маршруты их назначение, добавление именнованных маршрутов посредством аттрибутов, именнованные маршруты и хелперы**
> Routing in ASP.NET doesn’t require that you name your routes, and in many cases it seems to work
just fi ne without using names. To generate a URL, simply grab a set of route values you have lying
around, hand it to the Routing engine, and let the Routing engine sort it all out. However, as you’ll
see in this section, in some cases this can break down due to ambiguities about which route should
be chosen to generate a URL. Named routes solve this problem by giving precise control over route
selection when generating URLs.
For example, suppose an application has the following two traditional routes defined:
```c#
public static void RegisterRoutes(RouteCollection routes)
{
routes.MapRoute(
name: "Test",
url: "code/p/{action}/{id}",
defaults: new { controller = "Section", action = "Index", id = "" }
);
routes.MapRoute(
name: "Default",
url: "{controller}/{action}/{id}",
defaults: new { controller = "Home", action = "Index", id = "" }
);
}
```
> To generate a hyperlink to each route from within a view, you write the following code:
```c#
@Html.RouteLink("to Test", new {controller="section", action="Index", id=123})
@Html.RouteLink("to Default", new {controller="Home", action="Index", id=123})
```
> Notice that these two method calls don’t specify which route to use to generate the links. They
simply supply some route values and let the ASP.NET Routing engine fi gure it all out. In this example,
the fi rst method generates a link to the URL /code/p/Index/123 and the second to /Home/
Index/123, which should match your expectations. This is fi ne for these simple cases, but in some
situations this can bite you.
> Suppose you add the following page route at the beginning of your list of routes so that the URL /
static/url is handled by the page /aspx/SomePage.aspx:
```c#
routes.MapPageRoute("new", "static/url", "~/aspx/SomePage.aspx");
```
> Note that you can’t put this route at the end of the list of routes within the RegisterRoutes method
because it would never match incoming requests. A request for /static/url would be matched by
the default route and never make it through the list of routes to get to the new route. Therefore, you
need to add this route to the beginning of the list of routes before the default route.
> Moving this route to the beginning of the defi ned list of routes seems like an innocent enough
change, right? For incoming requests, this route will match only requests that exactly match /
static/url but will not match any other requests. This is exactly what you want. However, what
about generated URLs? If you go back and look at the result of the two calls to Url.RouteLink,
you’ll fi nd that both URLs are broken:
`/static/url?controller=section&action=Index&id=123`
and
`/static/url?controller=Home&action=Index&id=123`
> This goes into a subtle behavior of Routing, which is admittedly somewhat of an edge case, but is
something that people run into from time to time.
> Typically, when you generate a URL using Routing, the route values you supply are used to “fi ll in”
the route parameters, as discussed earlier in this chapter.
> When you have a route with the URL `{controller}/{action}/{id}`, you’re expected to supply
values for `controller`, `action`, and `id` when generating a URL. In this case, because the new route
doesn’t have any route parameters, it matches every URL generation attempt because technically,
“a route value is supplied for each route parameter.” It just so happens that there aren’t any route
parameters. That’s why all the existing URLs are broken, because every attempt to generate a URL
now matches this new route.
> This might seem like a big problem, but the fixis simple: Always use named routes when generating
URLs. Most of the time, letting Routing sort out which route you want to use to generate a URL
is really leaving it to chance, which is not something that sits well with the obsessive-compulsive,
control-freak developer. When generating a URL, you generally know exactly which route you want
to link to, so you might as well give it a name and use it. If you have a need to use non-named routes
and are leaving the URL generation entirely up to Routing, we recommend writing unit tests that
verify the expected behavior of the routes and URL generation within your application.
> Specifying the name of the route not only avoids ambiguities, but it might even eke out a bit of a
performance improvement because the Routing engine can go directly to the named route and
attempt to use it for URL generation.
> For the previous example, where you generated two links, the following change fi xes the issue. We
changed the code to use named parameters to make it clear what the route was.
```c#
@Html.RouteLink(
linkText: "route: Test",
routeName: "test",
routeValues: new {controller="section", action="Index", id=123}
)
@Html.RouteLink(
linkText: "route: Default",
routeName: "default",
routeValues: new {controller="Home", action="Index", id=123}
)
```
> For attribute routes, the name is specified as an optional argument on the attribute:
`[Route("home/{action}", Name = "home")]`
> Generating links to attribute routes works the same way as it does for traditional routes.
For attribute routes, unlike traditional routes, the route name is optional. We recommend leaving it
out unless you need to generate a link to the route. Under the hood, MVC does a small bit of extra
work to support link generation for named attribute routes, and it skips that work if the attribute
route is unnamed.

- **MVC Areas. Areas and attribute routing.**
> Areas, introduced in ASP.NET MVC 2, allow you to divide your models, views, and controllers into
separate functional sections. This means you can separate larger or more complex sites into sections,
which can make them a lot easier to manage.

> *Area Route Registration*
> You confi gure area routes by creating classes for each area that derive from the AreaRegistration
class, overriding AreaName and RegisterArea members. In the default project templates for ASP.
NET MVC, there’s a call to the method AreaRegistration.RegisterAllAreas within the
Application_Start method in Global.asax.

> *Area Route Conflicts*
> You might have good reasons to use the same controller name (for example, you don’t want to
affect your generated route URLs). In that case, you can specify a set of namespaces to use for
locating controller classes for a particular route. The following code shows how to do that using a
traditional route:
```c#
routes.MapRoute(
"Default",
"{controller}/{action}/{id}",
new { controller = "Home", action = "Index", id = "" },
new [] { "AreasDemoWeb.Controllers" }
);
```
> The preceding code uses a fourth parameter that is an array of namespace names. The controllers
for the example project live in a namespace called AreasDemoWeb.Controllers.

> To utilize areas with attribute routing, use the RouteArea attribute. In attribute routing, you don’t
need to specify the namespace, because MVC can fi gure that out for you (the attribute is on the controller,
which knows its own namespace). Instead, you specify the name of the AreaRegistration
in a RouteArea attribute.
```c#
[RouteArea("admin")]
[Route("users/{action}")]
public class UsersController : Controller
{
// Some action methods
}
```
> By default, all attribute routes for this class use the area name as the route prefi x. So the preceding
route is for URLs like /admin/users/index. If you would rather have a different route prefi x, you
can use the optional AreaPrefix property:
```c#
[RouteArea("admin", AreaPrefix = "manage")]
[Route("users/{action}")]
```
> This code would use URLs like /manage/users/index instead. Just like with prefi xes defi ned by
RoutePrefix, you can leave the RouteArea prefi x out by starting the route template with the ~/
characters.

- **Catch-all parameters.**
> A catch-all parameter allows for a route to match part of a URL with an arbitrary number of
segments. The value put in the parameter is the rest of the URL path (that is, it excludes the query
string, if any). A catch-all parameter is permitted only as the last segment of the route template.
For example, the following traditional route below would handle requests like those shown
in Table 9-4:
```c#
public static void RegisterRoutes(RouteCollection routes)
{
routes.MapRoute("catchallroute", "query/{query-name}/{*extrastuff}");
}
```
> Attribute routing uses the same syntax. Just add an asterisk (*) in front of the parameter’s name to
make it a catch-all parameter.

URL                  | PARAMETER VALUE
---------------------|----------------------
/query/select/a/b/c  | extrastuff = "a/b/c"
/query/select/a/b/c/ | extrastuff = "a/b/c/"
/query/select/       | extrastuff = null (Route still matches. The catch-all just catches the null string in this case.)

- **Multiple Route Parameters in a Segment.**
> As mentioned earlier, a route URL may have multiple parameters per segment. For example, all the
following are valid route URLs:

> ➤ {title}-{artist}

> ➤ Album{title}and{artist}

> ➤ {filename}.{ext}

> To avoid ambiguity, parameters cannot be adjacent. For example, the following are invalid:

> ➤ {title}{artist}

> ➤ Download{filename}{ext}

> When a request comes in, Routing matches literals in the route URL exactly. Route parameters are
matched greedily, which has the same connotations as it does with regular expressions. In other
words, the route tries to match as much text as possible with each route parameter.
> For example, how would the route {filename}.{ext} match a request for /asp.net.mvc.xml? If
{filename} were not greedy, it would match only "asp" and the {ext} parameter would match
"net.mvc.xml". But because route parameters are greedy, the {filename} parameter matches
everything it can: "asp.net.mvc". It cannot match any more because it must leave room for the
.{ext} portion to match the rest of the URL, "xml."

- **Игнорирование запроса по определенному маршруту.**
> One way to ensure that Routing ignores such requests is to use the StopRoutingHandler.
The following example shows adding a route the manual way, by creating a route with a new
StopRoutingHandler and adding the route to the RouteCollection:
```c#
public static void RegisterRoutes(RouteCollection routes)
{
routes.Add(new Route
(
"{resource}.axd/{*pathInfo}",
new StopRoutingHandler()
));
routes.Add(new Route
(
"reports/{year}/{month}"
, new SomeRouteHandler()
));
}
```
> If a request for /WebResource.axd comes in, it will match that fi rst route. Because the fi rst route returns
a StopRoutingHandler, the Routing system will pass the request on to normal ASP.NET processing,
which in this case falls back to the normal HTTP handler mapped to handle the .axd extension.
There’s an even easier way to tell Routing to ignore a route, and it’s aptly named IgnoreRoute. It’s
an extension method that’s added to the RouteCollection type just like MapRoute, which you’ve
seen before. It is convenient, and using this new method along with MapRoute changes the example
to look like this:
```c#
public static void RegisterRoutes(RouteCollection routes)
{
routes.IgnoreRoute("{resource}.axd/{*pathInfo}");
routes.MapRoute("report-route", "reports/{year}/{month}");
}
```
> Isn’t that cleaner and easier to look at? You can fi nd a number of places in ASP.NET MVC where
extension methods such as MapRoute and IgnoreRoute can make things a bit tidier.

- **Debugging Routes.**
> When the Route Debugger is enabled it replaces all of your routes’ route handlers with a
DebugRouteHandler. This route handler traps all incoming requests and queries every route in
the route table to display diagnostic data on the routes and their route parameters at the bottom
of the page.
> To use the Route Debugger, simply use NuGet to install it via the following command in the
Package Manager Console window in Visual Studio: Install-Package RouteDebugger. This
package adds the Route Debugger assembly and then adds a setting to the appSettings section of
web.config used to turn Route Debugger on or off:
<add key="RouteDebugger:Enabled" value="true" />
> As long as the Route Debugger is enabled, it will display the route data pulled from the current
request URL in the address bar (see Figure 9-1). This enables you to type in various URLs in the
address bar to see which route matches. At the bottom, it shows a list of all defi ned routes in your
application. This allows you to see which of your routes would match the current URL.

- **URL Generation: Overflow parameters.**
> Overflow parameters are route values used in URL generation that are not specifi ed in the route’s
defi nition. To be precise, we mean values in the route’s URL, its defaults dictionary, and its constraints
dictionary. Note that ambient values are never used as overfl ow parameters.
Overflow parameters used in route generation are appended to the generated URL as query string
parameters. Again, an example is most instructive in this case. Assume that the following default
route is defined:
```c#
public static void RegisterRoutes(RouteCollection routes)
{
routes.MapRoute(
"Default",
"{controller}/{action}/{id}",
new { controller = "Home", action = "Index", id = UrlParameter.Optional }
);
}
```
> Suppose you’re generating a URL using this route and you pass in an extra route value, page = 2.
Notice that the route defi nition doesn’t contain a parameter named “page.” In this example, instead
of generating a link, you’ll just render out the URL using the `Url.RouteUrl` method.
`@Url.RouteUrl(new {controller="Report", action="List", page="123"})`
> The URL generated will be /Report/List?page=123. As you can see, the parameters we specifi ed
are enough to match the default route. In fact, we’ve specifi ed more parameters than needed. In
those cases, those extra parameters are appended as query string parameters. The important thing
to note is that Routing is not looking for an exact match when determining which route is a match.
It’s looking for a suffi cient match. In other words, as long as the specifi ed parameters meet the
route’s expectations, it doesn’t matter if extra parameters are specified.

- **Custom Route Constraints.**
> Routing provides an IRouteConstraint interface with a single Match method. Here’s a look at the
interface defi nition:
```c#
public interface IRouteConstraint
{
bool Match(HttpContextBase httpContext, Route route, string parameterName,
RouteValueDictionary values, RouteDirection routeDirection);
}
```
> When Routing evaluates route constraints, and a constraint value implements IRouteConstraint,
it causes the route engine to call the IRouteConstraint.Match method on that route constraint to
determine whether the constraint is satisfi ed for a given request.
> Route constraints are run for both incoming URLs and while generating URLs. A custom route
constraint will often need to inspect the routeDirection parameter of the Match method to apply
different logic depending on when it is being called.
> Routing itself provides one implementation of this interface in the form of the
HttpMethodConstraint class. This constraint allows you to specify that a route should match only
requests that use a specific set of HTTP methods (verbs).
> For example, if you want a route to respond only to GET requests, but not POST, PUT, or DELETE
requests, you could defi ne the following route:
```c#
routes.MapRoute("name", "{controller}", null,
new { httpMethod = new HttpMethodConstraint("GET")} );
```
> MVC also provides a number of custom constraints in the System.Web.Mvc.Routing.Constraints
namespace. These are where the inline constraints used by attribute routing live, and you can use
them in traditional routing as well. For example, to use attribute routing’s {id:int} inline constraint
in a traditional route, you can do the following:
```c#
routes.MapRoute("sample", "{controller}/{action}/{id}", null,
new { id = new IntRouteConstraint() });
```
