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

- **Сочетание маршрутизации посредством аттрибутов и традиционной маршрутизации, выбор между двумя подходамик маршрутизации**
- **Именованные маршруты их назначение, добавление именнованных маршрутов посредством аттрибутов, именнованные маршруты и хелперы**
- **MVC Areas. Areas and attribute routing.**
- **Catch-all parameters.**
- **Multiple Route Parameters in a Segment.**
- **Игнорирование запроса по определенному маршруту.**
- **Debugging Routes.**
- **URL Generation: Overflow parameters.**
- **Custom Route Constraints.**
