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

- **Аттрибут, определяющий числовой диапозон для свойства модели.**
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
> Every validation attribute allows you to pass a named parameter with a custom error message. ErrorMessage is the name of the parameter in every validation attribute.
> The custom error message can also have a single format item in the string. The built-in attributes
format the error message string using the friendly display name of a property. As an example, consider the Required
attribute in the following code:
```c#
[Required(ErrorMessage="Your {0} is required.")]
[StringLength(160, ErrorMessage="{0} is too long.")]
public string LastName { get; set; }
```
> The attribute uses an error message string with a format item ({0}).

> In applications built for international markets, the hard-coded error messages are a bad idea.
Instead of literal strings, you’ll want to display different text for different locales. Fortunately, all
the validation attributes also allow you to specify a resource type and a resource name for localized
error messages:
```c#
[Required(ErrorMessageResourceType=typeof(ErrorMessages),
ErrorMessageResourceName="LastNameRequired")]
[StringLength(160, ErrorMessageResourceType = typeof(ErrorMessages),
ErrorMessageResourceName = "LastNameTooLong")]
public string LastName { get; set; }
```
> The preceding code assumes you have a resource fi le in the project named ErrorMessages.resx
with the appropriate entries inside (LastNameRequired and LastNameTooLong). For ASP.NET to
use localized resource fi les, you must have the UICulture property of the current thread set to the
proper culture. 

- **В какой момент осуществляется валидация? Как осуществить явную привязку к модели? Валидация и состояние модели. Хелперы валидации.**
> By default, the ASP.NET MVC framework executes validation logic during model binding. As
discussed in Chapter 4, the model binder runs implicitly when you have parameters to an action
method:
```c#
[HttpPost]
public ActionResult Create(Album album)
{
 // the album parameter was created via model binding
 // ..
}
```
> You can also explicitly request model binding using the UpdateModel or TryUpdateModel methods
of a controller:
```c#
[HttpPost]
public ActionResult Edit(int id, FormCollection collection)
{
 var album = storeDB.Albums.Find(id);
 if(TryUpdateModel(album))
 {
 // ...
 }
}
```
> After the model binder is fi nished updating the model properties with new values, the
model binder uses the current model metadata and ultimately obtains all the validators
for the model. The MVC runtime provides a validator to work with data annotations (the DataAnnotationsModelValidator). This model validator can fi nd all the validation attributes
and execute the validation logic inside. The model binder catches all the failed validation rules and
places them into model state.

> The primary side effect of model binding is model state (accessible in a Controller-derived object
using the ModelState property). Not only does model state contain all the values a user attempted
to put into model properties, but model state also contains all the errors associated with each
property (and any errors associated with the model object itself). If any errors exist in model state,
ModelState.IsValid returns false. 

> Объект ModelState сохраняет все значения, которые пользователь ввел для свойств модели, а также все ошибки, связанные с каждым свойством и с моделью в целом. Если в объекте ModelState имеются какие-нибудь ошибки, то свойство ModelState.IsValid возвратит false.

- **Custom validation logic vs default validation logic.**
> 

- **От какого класса наследуются аттрибуты валидации?**
> All the validation annotations (such as Required and Range) ultimately derive from the
ValidationAttribute base class. The base class is abstract and lives in the System
.ComponentModel.DataAnnotations namespace. Your validation logic will also live in a
class deriving from ValidationAttribute:
```c#
using System.ComponentModel.DataAnnotations;
namespace MvcMusicStore.Infrastructure
{
 public class MaxWordsAttribute : ValidationAttribute
 {
 }
}
```

- **Опишите реализацию кастомизированной логики валидации (custom validation).**
> создать класс, унаследованный от ValidationAttribute и переопределить метод IsValid, где описываем необходимую логику валидации
> The fi rst parameter to the IsValid method is the value to validate. 
> Использование атрибута аналогично использованию других атрибутов валидации

- **Как встроить валидацию в сам класс модели?**
> A self-validating model is a model object that knows how to validate itself. A model object can
announce this capability by implementing the IValidatableObject interface. As an example,
implement the check for too many words in the LastName fi eld directly inside the Order model:
```c#
public class Order : IValidatableObject
{
 public IEnumerable<ValidationResult> Validate(
 ValidationContext validationContext)
 {
 if (LastName != null &&
 LastName.Split(' ').Length > 10)
 {
 yield return new ValidationResult("The last name has too many words!",
 new []{"LastName"});
 }
 }
 // rest of Order implementation and properties
 // ...
}
 ```
>This has a few notable differences from the attribute version:
> ➤ The method the MVC runtime calls to perform validation is named Validate instead of
IsValid, but more important, the return type and parameters are different.
> ➤ The return type for Validate is an IEnumerable<ValidationResult> instead of a single
ValidationResult, because the logic inside is ostensibly validating the entire model and
might need to return more than a single validation error.
> ➤ No value parameter is passed to Validate because you are inside an instance method of the
model and can refer to the property values directly.
Notice that the code uses the C# yield return syntax to build the enumerable return value, and
the code needs to explicitly tell the ValidationResult the name of the fi eld to associate with (in
this case LastName, but the last parameter to the ValidationResult constructor will take an array
of strings so you can associate the result with multiple properties).

- **Как при помощи аттрибутов аннотации установить отображаемую для пользователя версию название свойства и порядок следования свойств в пользовательском интерфейсе?**
> Свойство Name атрибута Display содержит строку, которая будет отображаться вместо имени свойства.

> The Display attribute sets the friendly display name for a model property. You can use the Display
attribute to fi x the label for the FirstName field. In addition to the name, the Display attribute enables you to control the order in which properties will appear in the UI.
```c#
[Required]
[StringLength(160, MinimumLength=3)]
[Display(Name="First Name", Order=15000)]
public string FirstName { get; set; }
```
> The default value for Order is 10,000, and fields appear in ascending order.

- **Аттрибут для исключения свойства модели из представления при использовании шаблонов.**
> The ScaffoldColumn attribute hides a property from HTML helpers such as EditorForModel and DisplayForModel.
> With the attribute in place, EditorForModel will no longer display an input or label for the Id field. Note, however, the model binder might still try to move a value into the Id.
property if it sees a matching value in the request.
> При редактировании модели атрибут HiddenInput полностью не скрывает поля, так как мы можем посмотреть исходный код страницы и найти соответствующие поля. Чтобы полностью скрыть свойство от хелперов, используется атрибут ScaffoldColumn:
```c#
[ScaffoldColumn(false)]
public int Id { get; set; }
```
> Теперь хелперы редактирования не увидят данное свойство и не создадут для него даже скрытое поле на странице.

- **Аттрибут для форматированного отображения значения свойства модели.**
> The DisplayFormat attribute handles various formatting options for a property via named parameters.
You can provide alternate text for display when the property contains a null value, and turn
off HTML encoding for properties containing markup. You can also specify a data format string for the runtime to apply to the property value. In the following code you format the Total property of a
model as a currency value:
```c#
[DisplayFormat(ApplyFormatInEditMode=true, DataFormatString="{0:c}")]
public decimal Total { get; set; }
```
> The ApplyFormatInEditMode parameter is false by default, so if you want the Total value
formatted into a form input, you need to set ApplyFormatInEditMode to true. One reason ApplyFormatInEditMode is false by default is because the MVC model binder might not like to parse a value formatted for display.

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
