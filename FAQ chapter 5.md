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
> 

- Form Helpers, параметр htmlAttributes, как установить значение аттрибута class? Как установить значение аттрибута data-validatable?
- Класс HtmlHelper и методы расширений для свойства Html в представлении.
- Helper для отображения ошибок в представлении при валидации модели, как изменить стиль отображения ошибок.
- Helper'ы для скрытого поля, текстового поля, метки (label), выпадающего списка (единичный и множественный выбор, коллекцию каких объектов содержит список), отображения ошибок валидации свойства модели, многострочного текста.
> #### Html.TextBox and Html.TextArea
The TextBox helper renders an input tag with the type attribute set to text. You commonly use
the TextBox helper to accept free-form input from a user. For example, the call to

```@Html.TextBox("Title", Model.Title)```

results in

```<input id="Title" name="Title" type="text"
 value="For Those About To Rock We Salute You" />```

Use TextArea to render a <textarea> element for multi-line text
entry. The following code:
       
```@Html.TextArea("text", "hello <br/> world")```

produces

```<textarea cols="20" id="text" name="text" rows="2">hello &lt;br /&gt; world
</textarea>```

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
Both the DropDownList and ListBox helpers return a <select /> element. DropDownList allows
single item selection, whereas ListBox allows for multiple item selection (by setting the multiple
attribute to multiple in the rendered markup).
Typically, a select element serves two purposes:
➤ To show a list of possible options
➤ To show the current value for a field

> #### Html.ValidationMessage
When an error exists for a particular fi eld in the ModelState dictionary, you can use the
ValidationMessage helper to display that message. For example, in the following controller action,
you purposely add an error to model state for the Title property:
[HttpPost]

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
- Строго типизированные helper'ы и их преимущейства перед обычными helper'ами.
- Helpers and Model Metadata.
- Helpers and Model State.
- Helper для поля ввода пароля.
- Helpers for radio buttons, checkboxes.
- Rendering helpers.
- Rendering partial views with helpers.
- Action helpers.
