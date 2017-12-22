# Изучение Главы 8 книги "Professional ASP.NET MVC 5".

Данная глава затрагивает некоторые аспекты front-end разработки (HTML/CSS/JavaScript/jQuery, front-end фреймворк Bootstrap), при желании детально изучить данные технологии обращайтесь к вашему начальнику отдела или тутору, для общего ознакомления используйте следующие ресурсы:
HTML: https://html5book.ru/osnovy-html/
CSS: https://html5book.ru/osnovy-css/
JS: https://html5book.ru/osnovy-javascript/
jQuery: книга "jQuery Succinctly" (обращайтесь к тутору) или https://html5book.ru/vvedenie-v-jquery/
Bootstrap: https://www.tutorialspoint.com/bootstrap/

 Практическое задание к Главе 8:
- создайте приложение для работы с городами (отображение, добавление и т.п. - используйте Scaffolding Template)
- создайте представление со стилизованной Bootstrap кнопкой (классы btn, btn-primary), при нажатии на которую происходит асинхронный вывод (т.е. без перезагрузки всей страницы) в этом же представлении частичного представления со списком городов
- добавьте в созданное представление форму с полем ввода текста для поиска городов и кнопкой, при нажатии на которую происходит асинхронный вывод в этом же представлении частичного представления с результатом поиска
- создайте для свойства названия города аттрибут кастомной валидации на стороне клиента, который устанвливает максимальное количество символов названия города и выводит ошибку в представлении создания нового города, если введенное значение названия города превышает установленное в аттрибуте валидации (например:
[MaxCharacters(10)]
public string Name { get; set; }
)

Контрольные вопросы к Главе 8:
- **jQuery, функция jQuery и в какой момент она запускается.**
> jQuery is one of the most popular JavaScript libraries in existence, and remains an open source
project. Microsoft supports jQuery, and the project template for ASP.NET MVC will place all the files you need in order to
use jQuery into a Scripts folder when you create a new MVC project.

> The jQuery function object is the object you’ll use to gain access to jQuery features. The function (named jQuery) is aliased to the $ sign (because $ requires less typing and is a legal function name in JavaScript). Even more confusing is how you can pass nearly any type of argument into the $ function, and the function will deduce what you intend to achieve. The following
code demonstrates some typical uses of the jQuery function:
```js
$(function () {
$("#album-list img").mouseover(function () {
$(this).animate({ height: '+=25', width: '+=25' })
.animate({ height: '-=25', width: '-=25' });
});
});
```
> The first line of code invokes the jQuery function ($) and passes an anonymous JavaScript function
as the first parameter.

> When you pass a function as the first parameter, jQuery assumes you are providing a function to
execute as soon as the browser is finished building a document object model (DOM) from HTML
supplied by the server—that is, the code will run after the HTML page is done loading from the
server. This is the point in time when you can safely begin executing script against the DOM, and
we commonly call this the “DOM ready” event.

- **jQuery селекторы, события и method chaining.**

> Selectors are the strings you pass to the jQuery function to select elements in the DOM. The
jQuery selector syntax derives from CSS 3.0 selectors, with some additions. The list of
the selectors you’ll see in everyday jQuery code.
``` 
$("#header") - Find the element with an id of "header"
$(".editor-label") - Find all elements with a class name of ".editor-label"
$("div") - Find all <div> elements
$("#header div") - Find all <div> elements that are descendants of the element with an id
of "header"
$("#header > div") - Find all <div> elements that are children of the element with an id of
"header"
$("a:even") - Find evenly numbered anchor tags
```
> Another one of jQuery’s strengths is the API it provides for subscribing to events in the DOM.
Although you can use a generic on method to capture any event using an event name specified as
a string, jQuery also provides dedicated methods for common events, such as click, blur, and
submit.

> After you have some DOM elements selected, or are inside an event handler, jQuery makes
manipulating elements on a page easy. You can read the values of their attributes, set the values of
their attributes, add CSS classes to or remove them from the element, and more. The following code
adds the highlight class to or removes it from anchor tags on a page as the user’s mouse moves
through the element. The anchor tags should appear differently when users move their mouse over
the tag (assuming you have a highlight style set up appropriately).
```js
$("a").mouseover(function () {
$(this).addClass("highlight");
}).mouseout(function () {
$(this).removeClass("highlight");
});
```
> A couple of interesting notes about the preceding code:

> ➤ All the jQuery methods you use against a wrapped set, like the mouseover method, return
the same jQuery wrapped set. This means you can continue invoking jQuery methods on
elements you’ve selected without reselecting those elements. We call this method chaining.

> ➤ Shortcuts are available in jQuery for many common operations. Setting up effects for
mouseover and mouseout is a common operation, and so is toggling the presence of a style
class. You could rewrite the last snippet using some jQuery shortcuts and the code would
morph into the following:
```
$("a").hover(function () {
$(this).toggleClass("highlight");
});
```
- **Unobtrusive JavaScript.**
> Unobtrusive JavaScript is the practice of keeping JavaScript code separate from markup. You package
all the script code you need into .js files. If you look at the source code for a view, you don’t see
any JavaScript intruding into the markup. Even when you look at the HTML rendered by a view,
you still don’t see any JavaScript inside. The only sign of script you’ll see is one or more <script>
tags referencing the JavaScript files.

> You might find unobtrusive JavaScript appealing because it follows the same separation of concerns
that the MVC design pattern promotes. Keep the markup that is responsible for the display
separate from the JavaScript that is responsible for behavior. Unobtrusive JavaScript has additional
advantages, too. Keeping all of your script in separately downloadable files can give your site a
performance boost because the browser can cache the script file locally.

> Unobtrusive JavaScript also allows you to use a strategy known as progressive enhancement for your
site. Progressive enhancement is a focus on delivering content. Only if the device or browser viewing
the content supports features like scripts and style sheets will your page start doing more advanced
things, such as animating images.

> ASP.NET MVC 5 takes an unobtrusive approach to JavaScript. Instead of emitting JavaScript code
into a view to enable features such as client-side validation, the framework sprinkles metadata into
HTML attributes. Using jQuery, the framework can fi nd and interpret the metadata, and then
attach behaviors to elements, all using external script fi les. Thanks to unobtrusive JavaScript, the
Ajax features of ASP.NET MVC support progressive enhancement. If the user’s browser doesn’t
support scripting, your site will still work (they just won’t have the “nice to have” features such as
client validation).

- **Способы включения jQuery в представление.**
>Adding a script reference is as easy as including the following code:
```html
<script src="~/Scripts/jquery-1.10.2.js"></script>
```
> Although a simple script reference (as shown earlier) works, it’s version dependent: If you
want to update to a newer version of jQuery, you must search through your code and replace
the script references with the updated version number. A better way of including a jQuery reference
in your views is to use the built-in, version-independent jQuery script bundle. You can
see this approach in the script references in /Views/Shared/_Layout.cshtml as shown in the
following code:
`@Scripts.Render("~/bundles/jquery")`
> In addition to simplifying script updates in the future, this bundle reference also provides a number
of other benefits, such as automatically using minimized scripts in release mode and centralizing
script references so you can make updates in one place. 

> Чтобы подключить файл javascript используется метод Render класса System.Web.Optimization.Scripts:
`@Scripts.Render("~/scripts/jquery.validate.min.js")`
> Этот метод принимает в качестве параметра строку - полный путь к скрипту.
> Также для подключения скриптов мы можем использовать хелпер Url.Content:
```html
<script src="@Url.Content("~/scripts/jquery.validate.min.js")" type="text/javascript"></script>
```
- **Опишите шаги включения jQuery в проект и представление, при которых обновление jQuery до следующей версии потребует минимальных усилий и времени. Зависимости библиотек. Хорошая практика добавления кастомных скриптов в проект.**
> 220

>  The Scripts
directory in a new project already includes more than a dozen script fi les that you didn’t write (often
called vendor scripts), creating a separate application-specifi c subdirectory for your custom scripts is
a good practice. This makes it obvious to both you and other developers who work with your code
which scripts are libraries and which are custom application specifi c. A common convention is to
place your custom scripts in a /Scripts/App subdirectory.

- **Порядок загрузки скриптов в представление.**
> The script tag must appear later in the rendered document than the script reference for jQuery,
because MusicScripts.js requires jQuery and the browser loads scripts in the order in which they
appear in the document.

- **С точки зрения производительности в каком месте HTML документа лучше добавлять JS скрипты?**
> You might wonder why the standard script references aren’t just included
at the top of the _Layout view, so jQuery would be available for scripts in any
of your views. This is done for performance reasons. The general recommendation
is to put JavaScript references at the end of your HTML documents, right
before the closing body tag, so that script references don’t block parallel downloads
for other page resources (images and CSS). 

- **Если возникает необходимость поместить дополнительный скрипт, зависящий от jQuery в определенном представлении, код которого подключается посредством @RenderBody, но бандл jQuery подключается позже (расположен ниже в коде, например представление _Layout), что приведет к ошибке, - то как обойти данную проблему?**
> The solution to this problem is to render your custom scripts in the predefi ned scripts section,
discussed next.

> Rather than just writing out script tags inline in individual views, you can inject scripts into the
output using defi ned Razor sections where scripts should appear. You can add your own custom
sections, but the default _Layout view in a new ASP.NET MVC 5 application includes a section
specifi cally for you to include scripts that depend on jQuery. The name of the section is Scripts,
and it will appear after jQuery is loaded so that your custom scripts can take a dependency
on jQuery.
Inside of any content view, you can now add a scripts section to inject view-specifi c scripts. This
example shows how to place it at the bottom of the /Views/Home/Index.cshtml view:
```html
<ul class="row list-unstyled" id="album-list">
 @foreach (var album in Model)
{
 <li class="col-lg-2 col-md-2 col-sm-2 col-xs-4 container">
 <a href="@Url.Action("Details", "Store", new { id = album.AlbumId })">
 <img alt="@album.Title" src="@Url.Content( @album.AlbumArtUrl)" />
 <h4>@album.Title</h4>
 </a>
 </li>
}
</ul>
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
@section Scripts {
    <script src="~/Scripts/App/MusicScripts.js"> </script>
}
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
</div>
```
> The section approach allows you to have precise placement of script tags and ensure required scripts
are included in the proper order. By default, the _Layout view in a new MVC 5 application renders
the script toward the bottom of the page, just before the closing body tag.

- **Опишите назначение каждого файла, включенного в каталог Scripts приложения ASP.NET MVC по умолчанию.**
> _references.js is just a list of JavaScript libraries in your project, written out using triple-slash
(///) comments. Visual Studio uses it to determine which libraries to include in global JavaScript
IntelliSense throughout your project (in addition to other in-page script references, which are
also included at the individual view level).
> There are also several .min.js files. Each contains a minimized version of another script file.
JavaScript minimization is the process of shrinking a JavaScript file by removing comments, thus
shortening variable names, and other processes that reduce the file size. Minimized JavaScript files
are great for performance because they cut down on bandwidth and client-side parsing, but they’re
not easy to read. For this reason, both minimized and unminimized versions are included in the
project templates. This allows you to read and debug using the easy-to-read, commented versions,
but gain the performance benefits of using minimized files in production. This is all handled for you
by the ASP.NET bundling and minification system—in debug mode it serves the unminimized versions;
in release mode it automatically finds and serves the .min.js versions.
> jQuery also includes a .min.map.js version. This is a source map file. Source maps are an emerging
standard, which allows browsers to map minified, compiled code back to the original code that was
authored. If you’re debugging JavaScript in a browser that supports source maps and one is available
for the script you’re debugging, it shows you the original source.

> Bootstrap.js contains a set of jQuery-based plugins that complement Bootstrap by adding some
additional interactive behavior. For example, the Modals plugin shows simple modal displays using
Bootstrap styles, using jQuery for display and events. библиотека, позволяющая создавать адаптивные веб-приложения с использованием css-фреймворка bootstrap

> Respond.js is a tiny JavaScript library, included because it’s required by Bootstrap. It’s what’s
known as a polyfill: a JavaScript library that adds support for newer browser standards to older
browsers. In the case of Respond.js, that missing standard is min-width and max-width CSS3
media query support for Internet Explorer 6–8. This allows Bootstrap’s responsive CSS to work
great on Internet Explorer 6–8, and it’s ignored in newer browsers that have native support for
CSS3 media queries. позволяет использовать правила media queries CSS3 в старых браузерах, которые напрямую не поддерживают данную возможность

> Modernizr.js is a JavaScript library that helps you build modern applications by modernizing
older browsers. For example, one important job of Modernizr is to enable the new HTML 5 elements
(such as header, nav, and menu) on browsers that don’t natively support HTML 5 elements
(like Internet Explorer 6). Modernizr also allows you to detect whether advanced features such as
geolocation and the drawing canvas are available in a particular browser. библиотека, позволяющая определить, поддерживает ли браузер те или иные возможности HTML5 и CSS3

> The files with “unobtrusive” in the name are those written by Microsoft. The unobtrusive scripts
integrate with jQuery and the MVC framework to provide the unobtrusive JavaScript features
mentioned earlier. You’ll need to use these files if you want to use Ajax features of the ASP.NET
MVC framework. предоставляет поддержку ненавязчивой валидации модели

> jquery.validate.js - представляет функционал для валидации на стороне клиента

> jquery-1.10.2.intellisense.js и jquery.validate-vsdoc.js - используются для поддержки документации и IntelliSense по соответствующим библиотекам в Visual Studio

> jquery-1.10.2.js - базовая библиотека jQuery, на которую опираются большинство других скриптов. В данном случае используется версия 1.10.2.

- **Ajax helpers и их включение в проект.**
> You’ve seen the HTML helpers in ASP.NET MVC. You can use the HTML helpers to create forms
and links that point to controller actions. You also have a set of Ajax helpers in ASP.NET MVC. Ajax
helpers also create forms and links that point to controller actions, but they behave asynchronously.
When using these helpers, you don’t need to write any script code to make the asynchrony work.
Behind the scenes, these Ajax helpers depend on the unobtrusive MVC extensions for jQuery. To
use the helpers, you need to install the jquery.unobtrusive-ajax.js script in your project and add
script references to your views. This is a change from previous versions of MVC, which included the
script in the project template as well as a script reference in the _Layout view. 

> The Ajax functionality of the Ajax helpers will not work without a reference
to the jquery.unobtrusive-ajax.js script. If you’re having trouble
with the Ajax helpers, this is the fi rst thing you should check.

> Fortunately, adding the unobtrusive Ajax script to your project is really easy using NuGet. Rightclick
your project, open the Manage NuGet Packages dialog, and search for Microsoft jQuery
Unobtrusive Ajax. Alternatively, you can install it via the Package Manager
Console using the following command: Install-Package Microsoft.jQuery.Unobtrusive.Ajax.

> You can either add a script reference to the application’s _Layout view or just in views that will be
using the Ajax helpers. Unless you’re making a lot of Ajax requests throughout your site, I recommend
just adding script references to individual views.
This example shows how to add an Ajax request to the Scripts section of the Home Index view
(Views/Home/Index.cshtml). You can manually type in the script reference, or you can drag and
drop jQuery file from Solution Explorer into the view and Visual Studio will automatically add the
script reference.
> The updated view should now include the following script references (assuming you followed the
earlier example, which added the MusicScripts.js reference):
```html
@section Scripts {
 <script src="~/Scripts/App/MusicScripts.js"></script>
 <script src="~/Scripts/jquery.unobtrusive-ajax.min.js"> </script>
}
```

- **Как работает Ajax хелпер действия?**
> The ActionLink method of the Ajax property creates an anchor tag with asynchronous
behavior. Imagine you want to add a “daily deal” link at the bottom of the opening page
for the MVC Music Store. When users click the link, you don’t want them to navigate to a
new page, but you want the existing page to automatically display the details of a heavily
discounted album.
To implement this behavior, you can add the following code into the Views/Home/Index.cshtml
view, just below the existing album list:
```html
<div id="dailydeal">
 @Ajax.ActionLink("Click here to see today's special!",
 "DailyDeal",
 null,
 new AjaxOptions
 {
 UpdateTargetId = "dailydeal",
 InsertionMode = InsertionMode.Replace,
 HttpMethod = "GET"
 },
 new {@class = "btn btn-primary"})
</div>
```
The fi rst parameter to the ActionLink method specifi es the link text, and the second parameter
is the name of the action you want to invoke asynchronously. Like the HTML helper of the same
name, the Ajax ActionLink has various overloads you can use to pass a controller name, route values,
and HTML attributes.
One signifi cantly different type of parameter is the AjaxOptions parameter. The options parameter
specifi es how to send the request, and what will happen with the result the server returns. Options
also exist for handling errors, displaying a loading element, displaying a confi rmation dialog, and
more. In the above code listing, you are using options to specify that you want to replace the element
with an id of "dailydeal" using whatever response comes from the server.
The fi nal parameter, htmlAttributes, specifi es the HTML class you’ll use for the link to apply a
basic Bootstrap button style.
To have a response available, you’ll need a DailyDeal action on the HomeController:
```c#
public ActionResult DailyDeal()
 {
 var album = GetDailyDeal();
 return PartialView("_DailyDeal", album);
 }
 // Select an album and discount it by 50%
 private Album GetDailyDeal()
 {
 var album = storeDB.Albums
 .OrderBy(a => System.Guid.NewGuid())
 .First();
 album.Price *= 0.5m;
 return album;
 }
 ```
- **Ajax хелпер формы и как он работает.**
> Let’s imagine another scenario for the front page of the music store. You want to give the user the
ability to search for an artist. Because you need user input, you must place a form tag on the page,
but not just any form—an asynchronous form:
```html
<div class="panel panel-default">
 <div class="panel-heading">Artist search</div>
 <div class="panel-body">
 @using (Ajax.BeginForm("ArtistSearch", "Home",
 new AjaxOptions
 {
 InsertionMode = InsertionMode.Replace,
 HttpMethod = "GET",
 OnFailure = "searchFailed",
 LoadingElementId = "ajax-loader",
 UpdateTargetId = "searchresults",
 }))
 {
 <input type="text" name="q" />
 <input type="submit" value="search" />
 <img id="ajax-loader"
 src="@Url.Content("~/Images/ajax-loader.gif")"
 style="display:none" />
 }
 <div id="searchresults"></div>
 </div>
</div>
```
- **Подключение валидации на стороне клиента для представления, с использованием хелпером, для всего приложения.**
> 
- **Хелперы и валидация.**
- **Кастомная валидация на стороне клиента. Какие преимущества(о) можете отметить по сравнению с серверной валидацией?**
> The IClientValidatable interface defi nes a single method: GetClientValidationRules.
When the MVC framework fi nds a validation object with this interface present, it
invokes GetClientValidationRules to retrieve—you guessed it—a sequence of
ModelClientValidationRule objects. These objects carry the metadata, or the rules, the
framework sends to the client.
> You can implement the interface for the custom validator with the following code:
```c#
public class MaxWordsAttribute : ValidationAttribute,
 IClientValidatable
{
 public MaxWordsAttribute(int wordCount)
 : base("Too many words in {0}")
 {
 WordCount = wordCount;
 }
 public int WordCount { get; set; }
 protected override ValidationResult IsValid(
 object value,
 ValidationContext validationContext)
 {
 if (value != null)
 {
 var wordCount = value.ToString().Split(' ').Length;
 if (wordCount > WordCount)
 {
 return new ValidationResult(
 FormatErrorMessage(validationContext.DisplayName)
 );
 }
 }
 return ValidationResult.Success;
 }
 public IEnumerable<ModelClientValidationRule>
 GetClientValidationRules(
 ModelMetadata metadata, ControllerContext context)
 {
 var rule = new ModelClientValidationRule();
 rule.ErrorMessage =
 FormatErrorMessage(metadata.GetDisplayName());
 rule.ValidationParameters.Add("wordcount", WordCount);
 rule.ValidationType = "maxwords";
 yield return rule;
 }
}
 ```
- **jQuery UI.**
> jQuery UI is a jQuery plugin that includes both effects and widgets. Like all plugins it integrates
tightly with jQuery and extends the jQuery API. As an example, let’s return to the fi rst bit of code
in this chapter—the code to animate album items on the front page of the store:
```js
$(function () {
 $("#album-list img").mouseover(function () {
 $(this).animate({ height: '+=25', width: '+=25' })
 .animate({ height: '-=25', width: '-=25' });
 });
});
```
> Instead of the verbose animation, let’s take a look at how you would use jQuery UI to make
the album bounce. The fi rst step is to install the jQuery UI Combined Library NuGet package
(Install-Package jQuery.UI.Combined). This package includes the script fi les (minifi ed and
unminifi ed), CSS fi les, and images used by the core jQueryUI plugins.
> Next, you need to include a script reference to the jQuery UI library. You could either add it immediately
after the jQuery bundle in the _Layout view, or in an individual view where you’ll be using
it. Because you’re going to use it in your MusicScripts and you want to use those throughout the
site, add the reference to the _Layout as shown in the following:
```
@Scripts.Render("~/bundles/jquery")
@Scripts.Render("~/bundles/bootstrap")
 <script src="~/Scripts/jquery-ui-1.10.3.min.js"></script>
@RenderSection("scripts", required: false)
```
> Now you can change the code inside the mouseover event handler:
```js
$(function () {
 $("#album-list img").mouseover(function () {
 $(this).effect("bounce");
 });
});
```
- **JSON объект как результат выполнения метода действия. JSON and Client-Side Templates (общее понимание). Adding Templates (общее понимание).**
- **jQuery альтернатива Ajax хелперу формы (общее понимание).**
> Let’s change the
ArtistSearch action of the HomeController to return JSON instead of a partial view:
public ActionResult ArtistSearch(string q)
{
 var artists = GetArtists(q);
 return Json(artists, JsonRequestBehavior.AllowGet);
}
Now you’ll need to change the script to expect JSON instead of HTML. jQuery provides a method
named getJSON that you can use to retrieve the data:
$("#artistSearch").submit(function (event) {
 event.preventDefault();
 var form = $(this);
 $.getJSON(form.attr("action"), form.serialize(), function (data)
 // now what?
 });
});
> The code didn’t change dramatically from the previous version. Instead of calling load, you call
getJSON. The getJSON method does not execute against the matched set. Given a URL and some
query string data, the method issues an HTTP GET request, deserializes the JSON response into an
object, and then invokes the callback method passed as the third parameter. What do you do inside
of the callback? You have JSON data—an array of artists—but no markup to present the artists.
This is where templates come into play. A template is markup embedded inside a script tag. The following
code shows a template, as well as the search result markup where the results should display:
```html
<script id="artistTemplate" type="text/html">
 <ul>
 {{#artists}}
 <li>{{Name}}</li>
 {{/artists}}
 </ul>
</script>
<div id="searchresults">
</div>
```
> Notice that the script tag is of type text/html. This type ensures the browser does not try to interpret
the contents of the script tag as real code. The {{#artists}} expression tells the template
engine to loop through an array named artists on the data object you’ll use to render the template.
The {{Name}} syntax is a binding expression. The binding expression tells the template engine to
fi nd the Name property of the current data object and place the value of the property between <li>
and </li>. The result will make an unordered list from JSON data. You can include the template
directly below the form, as shown in the following code:
```html
<form id="artistSearch" method="get" action="@Url.Action("ArtistSearch", "Home")">
 <input type="text" name="q"
 data-autocomplete-source="@Url.Action("QuickSearch", "Home")" />
 <input type="submit" value="search" />
 <img id="ajax-loader"
 src="@Url.Content("~/Content/Images/ajax-loader.gif")"
 style="display:none" />
</form>
<script id="artistTemplate" type="text/html">
 <ul>
 {{#artists}}
 <li>{{Name}}</li>
 {{/artists}}
 </ul>
</script>
<div id="searchresults"></div>
```
> To use the template, you need to select it inside the getJSON callback and tell Mustache to render
the template into HTML:
```js
$("#artistSearch").submit(function(event) {
 event.preventDefault();
 var form = $(this);
 $.getJSON(form.attr("action"), form.serialize(), function(data) {
 var html = Mustache.to_html($("#artistTemplate").html(),
 { artists: data });
 $("#searchresults").empty().append(html);
 });
});
```
> The to_html method of Mustache combines the template with the JSON data to produce markup.
> The code takes the template output and places the output in the search results element.

- **jQuery.ajax (общее понимание).**
> When you need complete control over an Ajax request, you can turn to the jQuery ajax method.
The ajax method takes an options parameter where you can specify the HTTP verb (such as GET
or POST), the timeout, an error handler, and more. All the other asynchronous communication
methods you’ve seen (load and getJSON) ultimately call down to the ajax method.
Using the ajax method, you can achieve all the functionality you had with the Ajax helper and still
use client-side templates:
```js
$("#artistSearch").submit(function (event) {
 event.preventDefault();
 var form = $(this);
 $.ajax({
 url: form.attr("action"),
 data: form.serialize(),
 beforeSend: function () {
 $("#ajax-loader").show();
 },
 complete: function () {
 $("#ajax-loader").hide();
 },
 error: searchFailed,
 success: function (data) {
 var html = Mustache.to_html($("#artistTemplate").html(),
 { artists: data });
 $("#searchresults").empty().append(html);
 }
 });
});
```
> The call to ajax is verbose because you customize quite a few settings. The url and data properties
are just like the parameters you passed to load and getJSON. What the ajax method gives you is the
ability to provide callback functions for beforeSend and complete. You will respectively show and
hide the animated, spinning gif during these callbacks to let the user know a request is outstanding.
jQuery will invoke the complete callback even if the call to the server results in an error. Of the
next two callbacks, error and success, however, only one can win. If the call fails, jQuery calls the
searchFailed error function you already defi ned in the “Ajax Forms” section. If the call succeeds,
you will render the template as before.


- **Bootstrap Plugins (общее понимание).**
- **Improving Performance: CDNs, Script Optimizations, Bundling and Minification.**
> When you start sending large amounts of script code to the client, you have to keep performance
in mind. Many tools are available you can use to optimize the client-side performance of your site,
including YSlow for Firebug (see http://developer.yahoo.com/yslow/) and the developer tools
for Internet Explorer (see http://msdn.microsoft.com/en-us/library/bg182326.aspx). 

> **CDN** 
> Although you can certainly work with jQuery by serving the jQuery scripts from your own server,
you might instead consider sending a script tag to the client that references jQuery from a content
delivery network (CDN). A CDN has edge-cached servers located around the world, so there is a
good chance your client will experience a faster download. Because other sites will also reference
jQuery from CDNs, the client might already have the fi le cached locally. Plus, it’s always great when
someone else can save you the bandwidth cost of downloading scripts.
> Microsoft is one such CDN provider you can use. The Microsoft CDN hosts all the fi les used in this
chapter. If you want to serve jQuery from the Microsoft CDN instead of your server, you can use
the following script tag:
```html
<script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js"
 type="text/javascript"></script>
 ```
> **Script Optimizations**
> Many web developers do not use script tags inside the head element of a document. Instead, they
place script tags as close as possible to the bottom of a page. The problem with placing script tags 
inside the <head> tag at the top of the page is that when the browser comes across a script tag, it
blocks other downloads until after it retrieves the entire script. This blocking behavior can make a
page load slowly. Moving all your script tags to the bottom of a page (just before the closing body
tag) yields a better experience for the user.
> Another optimization technique for scripts is to minimize the number of script tags you send to a
client. You have to balance the performance gains of minimizing script references versus caching
individual scripts, but the tools mentioned earlier, like YSlow, can help you make the right decisions.
ASP.NET MVC 5 has the ability to bundle scripts, so you can combine multiple script fi les into a single
download for the client. MVC 5 can also minify scripts on the fl y to produce a smaller download.
 
> **Bundling and Minification**
> Bundling and minifi cation features are provided by classes in the System.Web.Optimization
namespace. As the namespace implies, these classes are designed to optimize the performance of a
web page by minifying fi les (reducing their size) and bundling fi les (combining multiple fi les into a
single download). The combination of bundling and minifi cation generally decreases the amount of
time needed to load a page into the browser.
> When you create a new ASP.NET MVC 5 application, you’ll fi nd bundles are automatically
confi gured for you during application startup. The confi gured bundles will live in a fi le named
BundleConfig.cs in the App_Start folder of a new project. Inside is code like the following to confi
gure script bundles (JavaScript) and style bundles (CSS):
```
bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
 "~/Scripts/jquery-{version}.js"));
bundles.Add(new ScriptBundle("~/bundles/jqueryval").Include(
 "~/Scripts/jquery.validate*"));
bundles.Add(new StyleBundle("~/Content/css").Include(
 "~/Content/bootstrap.css",
 "~/Content/site.css"));
 ```
> A script bundle is a combination of a virtual path (such as ~/bundles/jquery, which is the fi rst
parameter to the ScriptBundle constructor) and a list of fi les to include in the bundle. The virtual
path is an identifi er you’ll use later when you output the bundle in a view. The list of fi les in a
bundle can be specifi ed using one or more calls to the Include method of a bundle, and in the call
to include you can specify a specifi c fi lename or a fi lename with a wildcard to specify multiple fi les
at once.
> In the previous code, the fi le specifi er ~/Scripts/jquery.validate* tells the run time to include
all the scripts matching that pattern, so it picks up both jquery.validate.js and jquery.
validate.unobtrusive.js. The run time is smart enough to differentiate between minifi ed and
unminifi ed versions of a JavaScript library based on standard JavaScript naming conventions. It also
automatically ignores fi les that include IntelliSense documentation or source map information. You
can create and modify your own bundles in BundleConfig.cs. Custom bundles can include custom
minifi cation logic, which can do quite a bit—for example, it takes a few lines of code and a NuGet
package to create a custom bundle that compiles CoffeeScript to JavaScript, then passes it to the
standard minifi cation pipeline.
> After you have bundles confi gured, you can render the bundles with Scripts and Styles helper
classes. The following code outputs the jQuery bundle and the default application style sheet:
```c#
@Scripts.Render("~/bundles/jquery")
@Styles.Render("~/Content/css")
```
> The parameter you pass to the Render methods is the virtual path used to create a bundle. When
the application is running in debug mode (specifi cally, the debug fl ag is set to true in the compilation
section of web.config), the script and style helpers render a script tag for each individual fi le
registered in the bundle. When the application is running in release mode, the helpers combine all
the fi les in a bundle into a single download and place a single link or script element in the output.
In release mode, the helpers also minify fi les by default to reduce the download size.
