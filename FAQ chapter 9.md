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
> 
> **параметры маршрутизации**
> 
> **аттрибут маршрутизации контроллера**
> 
> **префиксы и отступление от префикса в определенном действии**
> 
> **что значит [Route] (без параметра)**
> 
> **route constraints**
> A route constraint is a condition that must be satisfi ed for the route to match. In this case, you just
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


> **route defaults**
> 
> **необязательные параметры маршрутизации**
> 

- **Традиционная маршрутизация в ASP.NET MVC (не посредством аттрибутов), Route Defaults, Route Constraints, ordering route definitions,**
- **Сочетание маршрутизации посредством аттрибутов и традиционной маршрутизации, выбор между двумя подходамик маршрутизации**
- **Именованные маршруты их назначение, добавление именнованных маршрутов посредством аттрибутов, именнованные маршруты и хелперы**
- **MVC Areas. Areas and attribute routing.**
- **Catch-all parameters.**
- **Multiple Route Parameters in a Segment.**
- **Игнорирование запроса по определенному маршруту.**
- **Debugging Routes.**
- **URL Generation: Overflow parameters.**
- **Custom Route Constraints.**
