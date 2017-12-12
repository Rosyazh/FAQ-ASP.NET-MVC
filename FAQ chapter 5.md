# Изучение Главы 5 книги "Professional ASP.NET MVC 5".

Практическое задание к Главе 5 (должно быть создано приложение, выводящее список стран и возможностью добавление/редактирования стран, также некоторые дополнительные хелперы, при создании форм, полей, выпадающих списков используйте хелперы, при вызове действия для обновления модели используйте аттрибут [HttpPost] для метода действия, при создании действий добавления, редактирования страны обращайтесь как к справке к соответвующим методам котроллера городов, который в ходе выполнения задания создается автоматически при помощи шаблона):
- Создать две модели: Города (Id, название) и Страны (Id, название, id города-столицы).
- Создать контроллер для городов, используя шаблон с максимально возможным списком действий и представлений. Добавить функционал для добавления трех городов (Лондон, Париж, Рим) по умолчанию при запуске приложения.
- Для стран создать действие и представление со списком стран и id столиц.
- Создать действие и представление для добавления новой страны (название и id города-столицы вводится вручную, добавить хелперы меток для названия страны и id города-столицы).
- Создать действие и представление для редактирования страны (для id страны используйте хелпер для скрытого поля).
- В представлении списка стран добавить ссылку для добавления новой страны, для каждой страны из списка добавить ссылку для редактирования страны. В представлении добавления новой страны добавить ссылку для возврата к списку стран.
- В представлении добавления/редактирования страны вместо поля с id города сделать выпадающий список из имеющихся городов при помощи хелпера. При редактировании по умолчанию должен быть выбран текущий город-столица для страны.
- В представлении для редактирования названия страны замените хелпер редактирования на хелпер, который в зависимости от аттрибута DataType свойства модели подбирает тот или иной тип поля редактирования, в модели страны для свойства названия объявите [DataType(DataType.Text)].
- Создайте частичное представление с выбором континента (radio button helper) (Азия, Европа, Африка) и флагом (checkbox helper) "Резидент ЕС". В форму редактирования страны добавьте созданное частичное представление при помощи соответствущего хелпера. Модель редактировать не надо.
- В метод действия редактирования страны включите обработку ошибок на случай, если id не задан и если страна не найдена в списке стран (в качестве справки смотрите соответвующий метод в котроллере города).

Контрольные вопросы к Главе 5:
- Для чего нужны HTML Helpers?
> HTML helpers help you work with HTML. Because it seems like a
simple task to type HTML elements into a text editor, you might wonder why you need any
help with your HTML. Tag names are the easy part, however. The hard part of working with
HTML is making sure the URLs inside of links point to the correct locations, form elements
have the proper names and values for model binding, and other elements display the appropriate
errors when model binding fails.
> Tying all these pieces together requires more than just HTML markup. It also requires some
coordination between a view and the runtime. In this chapter, you see how easy it is to establish
this coordination. Before you begin working with helpers, however, you fi rst learn about
forms. Forms are where most of the hard work happens inside an application, and are where
you need to use HTML helpers the most.

- Формы. Action. Method.
> A form is a container for input elements: buttons, checkboxes, text inputs, and more. The input elements
in a form enable a user to enter information into a page and submit information to a server.
The two most important attributes of a form tag: the action and the method attributes.

> The action attribute tells a web browser where to send the information, so naturally the action
contains a URL. The URL can be relative, or in cases where you want to send information to a different
application or a different server, the action URL can also be an absolute URL. The following
form tag sends a search term (the input named q) to the Bing search page from any application:

```<form action="http://www.bing.com/search">```

```     <input name="q" type="text" />```

```     <input type="submit" value="Search!" />```

```</form>```

> The method attribute tells the browser whether to use an HTTP POST or HTTP GET when sending the information.
You might think the default method for a form is HTTP POST. After all, you regularly POST forms
to update your profi le, submit a credit card purchase, and leave comments on the funny animal
videos on YouTube. However, the default method value is “get,” so by default a form sends an
HTTP GET request:

```<form action="http://www.bing.com/search" method="get">```

```    <input name="q" type="text" />```

```    <input type="submit" value="Search!" />```

```</form>```

- GET vs POST.
> the GET verb is the right tool for the job because GET represents an idempotent,
read-only operation. You can send a GET request to a server repeatedly with no ill effects,
because a GET does not (or should not) change state on the server
> A POST, on the other hand, is the type of request you
use to submit a credit card transaction, add an album to
a shopping cart, or change a password. A POST request
generally modifi es state on the server, and repeating the
request might produce undesirable effects (such as double
billing).
> Web applications generally use GET requests for reads and POST requests for writes (which
typically include updates, creates, and deletes). A request to pay for music uses POST. A request to
search for music, a scenario you look at next, uses GET.

- Form Helpers, параметр htmlAttributes, как установить значение аттрибута class? Как установить значение аттрибута data-validatable?
> 
- Класс HtmlHelper и методы расширений для свойства Html в представлении.
- Helper для отображения ошибок в представлении при валидации модели, как изменить стиль отображения ошибок.
> The ValidationSummary helper displays an unordered list of all validation errors in the ModelState
dictionary. The Boolean parameter you are using (with a value of true) is telling the helper to
exclude property-level errors. In other words, you are telling the summary to display only the errors
in ModelState associated with the model itself, and exclude any errors associated with a specifi c
model property. You will be displaying property-level errors separately.
> Но данный хелпер имеет перегруженные версии, которые помогают настроить более точное отображение сообщений об ошибках:
> Html.ValidationSummary() - Отображает общий список ошибок сверху
> Html.ValidationSummary(bool) - Если параметр равен true, то вверху будут отображаться только сообщения об ошибках уровня модели, а специфические ошибки будут отображаться рядом с полями ввода. Если же параметр равен false, то вверху отображаются все ошибки.
> Html.ValidationSummary(string) - Данная перегруженная версия хелпера отображает перед списком ошибок сообщение, которое передается в параметр string
> Html.ValidationSummary(bool, string) - Сочетает две предыдущие перегруженные версии

> Еще один важный момент отображения ошибок - это их стилизация. То, что мы видим ошибки в красном цвете и границы полей ввода также в красном цвете, не жестко установлено, и мы все это можем изменить. В файле стилей Site.css мы можем найти секцию, которая как раз и отвечает за стилизацию.

- Helper'ы для скрытого поля, текстового поля, метки (label), выпадающего списка (единичный и множественный выбор, коллекцию каких объектов содержит список), отображения ошибок валидации свойства модели, многострочного текста.
> @Html.HiddenFor(model => model.AlbumId)
> @Html.Hidden("BookId", "2")

> #### Html.TextBox and Html.TextArea
The TextBox helper renders an input tag with the type attribute set to text. You commonly use
the TextBox helper to accept free-form input from a user. For example, the call to

```@Html.TextBox("Title", Model.Title)```

results in

```<input id="Title" name="Title" type="text" value="For Those About To Rock We Salute You" />```

Use TextArea to render a <textarea> element for multi-line text
entry. The following code:
       
```@Html.TextArea("text", "hello <br/> world")```

produces

```<textarea cols="20" id="text" name="text" rows="2">hello &lt;br /&gt; world </textarea>```

> #### Html.Label
The Label helper returns a <label/> element using the string parameter to determine the rendered
text and for attribute value. A different overload of the helper enables you to independently set the
for attribute and the text. In the preceding code, the call to Html.Label(“GenreId”) produces the
following HTML:

```<label for="GenreId">Genre</label>```

The purpose of a label is to attach information to other input elements, such as text inputs, and boost the accessibility of your application. The for attribute of the label should contain
the ID of the associated input element. Screen readers can use the text of the label to provide a better description of the
input for a user. Also, if a user clicks the label, the browser transfers focus to the associated input
control. This is especially useful with checkboxes and radio buttons in order to provide the user
with a larger area to click (instead of clicking only on the checkbox or radio button itself).

> #### Html.DropDownList and Html.ListBox

> Both the DropDownList and ListBox helpers return a <select /> element. DropDownList allows
single item selection, whereas ListBox allows for multiple item selection (by setting the multiple
attribute to multiple in the rendered markup). Typically, a select element serves two purposes:

> ➤ To show a list of possible options.

> ➤ To show the current value for a field.

> #### Html.ValidationMessage
> When an error exists for a particular fi eld in the ModelState dictionary, you can use the
ValidationMessage helper to display that message. For example, in the following controller action,
you purposely add an error to model state for the Title property: 

```[HttpPost]```

```public ActionResult Edit(int id, FormCollection collection)```

```{ var album = storeDB.Albums.Find(id);```

```    ModelState.AddModelError("Title", "What a terrible name!");```

```    return View(album);```

```}```

In the view, you can display the error message (if any) with the following code:

```@Html.ValidationMessage("Title")```

which results in

```<span class="field-validation-error" data-valmsg-for="Title" data-valmsg-replace="true">```
```    What a terrible name!```
```</span>```

This message appears only if there is an error in the model state for the key Title. You can also call
an override that allows you to override the error message from within the view:

```@Html.ValidationMessage("Title", "Something is wrong with your title")```
 
 which results in
 
```<span class="field-validation-error" data-valmsg-for="Title" data-valmsg-replace="false">Something is wrong with your title```

- Где helper ищет значения, например Html.Textbox("Title")
>

- Строго типизированные helper'ы и их преимущейства перед обычными helper'ами.
> 

- Helpers and Model Metadata.
> Helpers do more than just look up data inside ViewData; they also take advantage of available
model metadata. For example, the album edit form uses the Label helper to display a label element
for the genre selection list:
```@Html.Label("GenreId")```
The helper produces the following output:
```<label for="GenreId">Genre</label>```
Where did the Genre text come from? The helper asks the runtime whether any model metadata is
available for GenreId, and the runtime provides information from the DisplayName attribute decorating
the Album model: 

```[DisplayName("Genre")]```

```public int GenreId { get; set; }```

- Helpers and Model State.
> All the helpers you use to display form values also interact with ModelState. Remember,
ModelState is a byproduct of model binding and holds all validation errors detected during model
binding. Model state also holds the raw values the user submits to update a model.
Helpers used to render form fi elds automatically look up their current value in the ModelState
dictionary. The helpers use the name expression as a key into the ModelState dictionary. If an
attempted value exists in ModelState, the helper uses the value from ModelState instead of a value
in view data.
The ModelState lookup allows bad values to preserve themselves after model binding fails. For
example, if the user enters the value abc into the editor for a DateTime property, model binding
will fail and the value abc will go into model state for the associated property. When you re-render
the view for the user to fi x validation errors, the value abc will still appear in the DateTime editor,
allowing the users to see the text they tried as a problem and allowing them to correct the error.
When ModelState contains an error for a given property, the form helper associated with the error
renders a CSS class of input-validation-error in addition to any explicitly specifi ed CSS classes.
The default style sheet, style.css, included in the project template contains styling for this class.

- Helper для поля ввода пароля.
> The Html.Password helper renders a password fi eld. It’s much like the TextBox helper, except that it
does not retain the posted value, and it uses a password mask. The following code:
@Html.Password("UserPassword")
results in
<input id="UserPassword" name="UserPassword" type="password" value="" />
The strongly typed syntax for Html.Password, as you would expect, is Html.PasswordFor. Here’s
how to use it to display the UserPassword property:

```@Html.PasswordFor(m => m.UserPassword)```

- Helpers for radio buttons, checkboxes.
Radio buttons are generally grouped together to provide a range of possible options for a single
value. For example, if you want the user to select a color from a specifi c list of colors, you can use
multiple radio buttons to present the choices. To group the radio buttons, you give each button the
same name. Only the selected radio button is posted back to the server when the form is submitted.
The Html.RadioButton helper renders a simple radio button:

```@Html.RadioButton("color", "red")```

```@Html.RadioButton("color", "blue", true)```

```@Html.RadioButton("color", "green")```

and results in

```<input id="color" name="color" type="radio" value="red" />```

```<input checked="checked" id="color" name="color" type="radio" value="blue" />```

```<input id="color" name="color" type="radio" value="green" />```

Html.RadioButton has a strongly typed counterpart, Html.RadioButtonFor. Rather than a name
and a value, the strongly typed version takes an expression that identifi es the object that contains
the property to render, followed by a value to submit when the user selects the radio button.

```@Html.RadioButtonFor(m => m.GenreId, "1") Rock```

```@Html.RadioButtonFor(m => m.GenreId, "2") Jazz```

```@Html.RadioButtonFor(m => m.GenreId, "3") Pop```

> The CheckBox helper is unique because it renders two input elements. Take the following code, for xample:

```@Html.CheckBox("IsDiscounted")```

This code produces the following HTML:
<input id="IsDiscounted" name="IsDiscounted" type="checkbox" value="true" />
<input name="IsDiscounted" type="hidden" value="false" />
You are probably wondering why the helper renders a hidden input in addition to the checkbox
input. The helper renders two inputs because the HTML specifi cation indicates that a browser will
submit a value for a checkbox only when the checkbox is on (selected). In this example, the second
input guarantees a value will appear for IsDiscounted even when the user does not check the
checkbox input.

- Rendering helpers.
> Rendering helpers produce links to other resources inside an application, and can also enable you to
build those reusable pieces of UI known as partial views.

- Rendering partial views with helpers.
> The Partial helper renders a partial view into a string. Typically, a partial view contains reusable
markup you want to render from inside multiple different views. Partial has four overloads:
```c#
public void Partial(string partialViewName);
public void Partial(string partialViewName, object model);
public void Partial(string partialViewName, ViewDataDictionary viewData);
public void Partial(string partialViewName, object model, ViewDataDictionary viewData);
```
> Notice that you do not have to specify the path or fi le extension for a view because the logic the
runtime uses to locate a partial view is the same logic the runtime uses to locate a normal view. For
example, the following code renders a partial view named AlbumDisplay. The runtime looks for the
view using all the available view engines.
`@Html.Partial("AlbumDisplay")`
> The RenderPartial helper is similar to Partial, but RenderPartial writes directly to the response
output stream instead of returning a string. For this reason, you must place RenderPartial inside
a code block instead of a code expression. To illustrate, the following two lines of code render the
same output to the output stream:
```c#
@{Html.RenderPartial("AlbumDisplay "); }
@Html.Partial("AlbumDisplay ")
```
> So, which should you use, Partial or RenderPartial? In general, you should prefer Partial to
RenderPartial because Partial is more convenient (you don’t have to wrap the call in a code
block with curly braces). However, RenderPartial might result in better performance because it
writes directly to the response stream, although it would require a lot of use (either high site traffi c
or repeated calls in a loop) before the difference would be noticeable.

- Action helpers.
> Action and RenderAction are similar to the Partial and RenderPartial helpers. The Partial
helper typically helps a view render a portion of a view’s model using view markup in a separate fi le.
Action, on the other hand, executes a separate controller action and displays the results. Action
offers more fl exibility and reuse because the controller action can build a different model and make
use of a separate controller context.
> Once again, the only difference between Action and RenderAction is that RenderAction writes
directly to the response (which can bring a slight effi ciency gain). Here’s a quick look at how you
might use this method. Imagine you are using the following controller:
```c#
public class MyController : Controller {
 public ActionResult Index() {
 return View();
 }
 
 [ChildActionOnly]
 public ActionResult Menu() {
 var menu = GetMenuFromSomewhere();
 return PartialView(menu);
 }
}
```
> The Menu action builds a menu model and returns a partial view with just the menu:
```html
@model Menu
<ul>
@foreach (var item in Model.MenuItem) {
 <li>@item.Text</li>
}
</ul>
```
> In your Index.cshtml view, you can now call into the Menu action to display the menu:
```html
<html>
<head><title>Index with Menu</title></head>
<body>
 @Html.Action("Menu")
 <h1>Welcome to the Index View</h1>
</body>
</html>
```
> Notice that the Menu action is marked with a ChildActionOnlyAttribute. The attribute prevents
the runtime from invoking the action directly via a URL. Instead, only a call to Action or
RenderAction can invoke a child action. The ChildActionOnlyAttribute isn’t required, but is
generally recommended for child actions.

