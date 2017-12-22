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
# 225
- Ajax helpers и их включение в проект.
- Как работает Ajax хелпер действия?
- Ajax хелпер формы и как он работает.
- Подключение валидации на стороне клиента для представления, с использованием хелпером, для всего приложения.
- Хелперы и валидация.
- Кастомная валидация на стороне клиента. Какие преимущества(о) можете отметить по сравнению с серверной валидацией?
- jQuery UI.
- JSON объект как результат выполнения метода действия. JSON and Client-Side Templates (общее понимание). Adding Templates (общее понимание).
- jQuery альтернатива Ajax хелперу формы (общее понимание).
- jQuery.ajax (общее понимание).
- Bootstrap Plugins (общее понимание).
- Improving Performance: CDNs, Script Optimizations, Bundling and Minification.
