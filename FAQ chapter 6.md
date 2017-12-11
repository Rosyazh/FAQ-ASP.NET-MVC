# Изучение Главы 6 книги "Professional ASP.NET MVC 5".
              
Практическое задание к Главе 6 (для создания контроллеров, представлений, контекста данных используйте Scaffolding Template ! ! !):
- Создайте модель данных со следующими свойствами: Id, Имя, Фамилия, Логин, Возраст, Адрес электронной почты, Подтверждение адреса электронной почты, Донат, Дата внесения доната, Код подразделения. Все свойства должны быть обязательными. Id должен быть скрытым от пользователя. Имя и фамилия должны быть длиной от 3 до 20 симоволов. Логин не должен быть включен в представления при использовании Scaffolding Templates. Возраст должен быть от 21 до 55. Адрес электронной почты и подтвержденный адрес электронной почты должны совпадать. Донат должен иметь денежный формат. Дата внесения доната должна быть соответсвующего типа/формата. Код подразделения должен быть нередактируемым. Фамилия должна принимать одно из трех значений (Иванов, Петров, Сидоров) - для этих целей создайте кастомный аттрибут и эти три значения должны задаваться при декларации аттрибута в модели ( [RestrictedNames(new[] { "Иванов", "Петров", "Сидоров" })] ). Модель должна считаться также невалидной, если Фамилия = Иванов и Донат < 10. Во всех аттрибутах валидации должны быть определены тексты сообщения об ошибках. Все свойства должны иметь дружественные пользователю имена при отображении в представлениях.
- Создайте контроллер и представления, а также контекст данных при помощи Scaffolding Template.
              
Контрольные вопросы к Главе 6:

- **Что такое аннотации валидации и к какому пространству имен они относятся?**
> Data annotations are attributes you can fi nd in the System.ComponentModel.DataAnnotations
namespace (although one attribute is defi ned outside this namespace, as you will see). These
attributes provide server-side validation, and the framework also supports client-side validation
when you use one of the attributes on a model property.
- **Аттрибут, декларирующий свойство модели как обязательное.**
> Required
Because you need the customers to give you their first name, you can decorate the
FirstName property of the Order model with the Required attribute:
```c#
[Required]
public string FirstName { get; set; }
```
> The attribute raises a validation error if either property value is null or empty.
> Применение этого атрибута к свойству модели означает, что данное свойство должно быть обязательно установлено.

- **Аттрибут, декларирующий длину строки текстового свойства модели.**
> The StringLength attribute can ensure the string value
provided by the customer will fi t in the database:
MinimumLength is an optional, named parameter you can use to specify the minimum length for a
string.
```c#
[StringLength(160, MinimumLength=3)]
public string FirstName { get; set; }
```
> Чтобы пользователь не мог ввести очень длинный текст, используется атрибут StringLength. Особенно это актуально, если в базе данных установлено ограничение на размер хранящихся строк.

- **Аттрибут, определяющий текстовый шаблон для свойства модели.**
> What you can do instead is ensure the value looks like a working e-mail address using a regular expression.
Использование данного атрибута предполагает, что вводимое значение должно соответствовать указанному в этом атрибуте регулярному выражению.
> Наиболее распространенный пример - это проверка адреса электронной почты на корректность. 
```c#
[RegularExpression(@"[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,4}")]
public string Email { get; set; }
```
### - Аттрибут, определяющий числовой диапозон для свойства модели.
> Атрибут Range определяет минимальные и максимальные ограничения для числовых данных.
```c#
[Range(35,44)]
public int Age { get; set; }
```
> Атрибут Range может работать как с целочисленными значениями, так и с числами с плавающей точкой. А еще одна перегруженная версия его конструктора принимает параметр Type и две строки (which can allow you to add a range to date and decimal properties).
```c#
[Range(typeof(decimal), "0.00", "49.99")]
public decimal Price { get; set; }
```
- **Аттрибут, обязывающий свойства иметь одинаковые значения.**
> Атрибут Compare гарантирует, что два свойства объекта модели имеют одно и то же значение. Если, например, надо, чтобы пользователь ввел пароль дважды:
```c#
[DataType(DataType.Password)]
public string Password { get; set; }

[Compare("Password",ErrorMessage="Пароли не совпадают")]
[DataType(DataType.Password)]
public  string PasswordConfirm { get; set; }
```
> Если пользователь введет второй раз другой пароль, отличный от первого, то он увидит ошибку

- **Аттрибут, позволяющий осуществлять валидацию на стороне клиента с обращениями к серверу.**
> Атрибут Remote в отличие от других классов атрибутов находится в пространстве имен System.Web.Mvc. Он позволяет выполнять валидацию на стороне клиента с обратными вызовами на сервер.
> Например, название книги, представленое свойством Name, не должно быть равно определенному значению. При валидации на стороне клиента трудно гарантировать соблюдение ряда проверок, особенно если мы задействуем базы данных. А с помощью атрибута Remote мы можем послать значение свойства Name на сервер, а там уже проверить.
```c#
[Remote("CheckName", "Home", ErrorMessage = "Name is not valid.")]
public string Name { get; set; }
```
> В атрибуте можно установить имя действия и имя контроллера, которые должны вызываться кодом на стороне клиента, а также еще ряд именованных параметров, в частности, сообщение об ошибке с помощью параметра ErrorMessage. Клиентский код посылает введенное пользователем значение для свойства Name автоматически, а перегруженный конструктор атрибута позволяет указать дополнительные поля, значения которых надо посылать на сервер.
```c#
[HttpGet]
public JsonResult CheckName(string name)
{
var result = !(name=="Название");
return Json(result, JsonRequestBehavior.AllowGet);
}
```
> Это действие контроллера принимает в качестве параметра имя свойства, подлежащего валидации, и возвращает true или false в форме объекта в формате JSON. При этом если возвращается false, то мы увидим сообщение об ошибке, заданное парааметром ErrorMessage.
- **Как установить текст ошибок для вывода при валидации? Как обеспечить вывод читабельной версии свойства модель в тексте ошибки? Как обеспечить вывод текста ошибки валидации на различных языках для различных локаций?**
> 
- **В какой момент осуществляется валидация? Как осуществить явную привязку к модели? Валидация и состояние модели. Хелперы валидации.**
> 
- **Custom validation logic vs default validation logic.**
> 
- **От какого класса наследуются аттрибуты валидации?**
> 
- **Опишите реализацию кастомизированной логики валидации (custom validation).**
> 
- **Как встроить валидацию в сам класс модели?**
> 
- **Как при помощи аттрибутов аннотации установить отображаемую для пользователя версию название свойства и порядок следования свойств в пользовательском интерфейсе?**
> 
- **Аттрибут для исключения свойства модели из представления при использовании шаблонов.**
> 
- **Аттрибут для форматированного отображения значения свойства модели.**
> 
- **Аттрибут, запрещающий редактировать свойство модели.**
> Place the ReadOnly attribute on a property if you want to make sure the default model binder does
not set the property with a new value from the request:
```c#
[ReadOnly(true)]
public decimal Total { get; set; }
```
> Note the EditorForModel helper will still display an enabled input for the property, so only the
model binder respects the ReadOnly attribute.

- **Аттрибут, определяющий тип значения свойства модели, например пароль. Какие еще типы могут быть определены посредством данного аттрибута.**
> The DataType attribute enables you to provide the runtime with information about the specifi c purpose
of a property. For example, a property of type string can fi ll a variety of scenarios—it might
hold an e-mail address, a URL, or a password.
```c#
[DataType(DataType.Password)]
public string Password { get; set; }
```
> Перечисление DataType может принимать несколько различных значений:
> Currency - Отображает текст в виде валюты
> DateTime - Отображает дату и время
> Date - Отображает только дату, без времени
> Time - Отображает только время
> Text - Отображает однострочный текст
> MultilineText - Отображает многострочный текст (элемент textarea)
> Password - Отображает символы с использованием маски
> Url - Отображает строку URL
> EmailAddress - Отображает электронный адрес

- **Посредством какого аттрибута можно задать шаблон для свойства модели при его отображении хелперами DisplayFor, EditorFor?**
> The UIHint attribute gives the ASP.NET MVC runtime the name of a template to use when rendering
output with the templated helpers (such as DisplayFor and EditorFor). You can define your
own template helpers to override the default MVC behavior. If the template specified by the UIHint is not found, MVC will find an appropriate fallback to use.
> Данный атрибут указывает, какой будет использоваться шаблон отображения при создании разметки html для данного свойства. Шаблон управляет, как свойство будет рендерится на странице.
```c#
[UIHint("Url")]
public string Name { get; set; }
```
- **Аттрибут, скрывающий свойство модели в представлении.**
> The HiddenInput attribute lives in the System.Web.Mvc namespace and tells the runtime to render
an input element with a type of hidden. Hidden inputs are a great way to keep information in a
form so the browser will send the data back to the server, but the user won’t be able to see or edit
the data (although a malicious user could change submitted form values to change the input value,
so don’t consider the attribute as foolproof).

> Чтобы скрыть это поле мы можем применить атрибут HiddenInput:
```c#
[HiddenInput (DisplayValue=false)]
public int Id { get; set; }
```
> Свойство DisplayValue=false указывает, что надо скрыть данное поле. В итоге вы его не увидите.
> При использовании хелперов редактирования (Html.EditorFor/Html.EditorForModel) для данного свойства будет сгенерировано скрытое поле: <input type="hidden" id="Id" name="Id" value="1" />
