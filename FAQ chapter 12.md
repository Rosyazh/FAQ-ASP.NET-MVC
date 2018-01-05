# Изучение Главы 12 книги "Professional ASP.NET MVC 5".

В главе упоминается JSON спецификация    https://ru.wikipedia.org/wiki/JSON

Практическое задание к Главе 12: созайте приложение по шаблону Web API, добавьте модель City (Id, Name), добавьте контроллер по шаблону Web API 2 Controller with Actions, using Entity Framework, добавьте несколько городов, которые будут храниться в базе по умолчанию, проверьте корректность возвращаемых данных в Postman (или каким-либо альтернативным инструментом, формат возвращаемых данных должен соответствовать JSON спецификации), подключите Angular в проект, добавьте необходимый код для обращения к API приложения и вывода списка городов (при возникновении проблем с функцией $http.get() обратитесь к ресурсу https://stackoverflow.com/questions/41169385/http-get-success-is-not-a-function и используйте другую функцию:
```js
$http({
        method: 'GET',
        url: '/api/cities'
    }).then(function (success) {
        $scope.cities = success.data;
    }, function (error) {

    });
)
```

Контрольные вопросы к Главе 12:
- **Что такое AngularJS?**
> AngularJS is a JavaScript framework developed by a team inside Google. The team built an extensible,
testable framework that provides data binding and server communication features as well as
view management, history management, localization, validation, and more. AngularJS (hereafter
referred to as Angular) also uses controllers, models, and views, which should sound familiar to you
because ASP.NET MVC also has the concept of controllers, models, and views, but Angular is all
about JavaScript and HTML instead of C# and Razor. 

- **Подключение Angular в проект.**
> You have different approaches for installing Angular. If you are particular about the Angular version
and features you need, you might go to the Angular website (angularjs.org) and download
the script fi les directly. The easiest approach, however, is to use NuGet and the Package Manager
Console.
`Install-Package AngularJS.core`
> The package installs a number of new script fi les into the Scripts folder of the project. The most
important fi le is the angular.js fi le (see Figure 12-4) because it contains the essential pieces of the
Angular framework.

- **Создание модуля в Angular и его загрузка в представление.**
> A module in Angular is an abstraction that allows you to group various components to keep them
isolated from other components and pieces of code in an application. One of the benefi ts of isolation
is to make Angular code easier to unit test, and easy testability is one of the goals the framework
creators set out to achieve. 
> Inside the Client folder, create a subfolder named Scripts. Inside this Scripts folder, create a
new JavaScript fi le named atTheMovies.js with the following code:
```js
(function () {
 var app = angular.module("atTheMovies", []);
}());
```
> The angular variable is the global Angular object. Just like the jQuery API is available
through a global $ variable, Angular exposes a top-level API through angular. In the previous
code, the module function creates a new module named atTheMovies, whereas the
second parameter, the empty array, declares dependencies for the module (technically this
module depends on the core Angular module “ng”, but you don’t need to list it explicitly
and you’ll see examples of other dependencies later in the chapter). 

> Also modify the Index view to include the new script:
```html
@section scripts {
 <script src="~/Scripts/angular.js"></script>
 <script src="~/Client/Scripts/atTheMovies.js"></script>
}
<div ng-app="atTheMovies">
</div>
```
- **MVC в Angular.**
> 

- **Предосторожности по минификации скриптов Angular (общее ознакомление).**
> 

- **Маршрутизация в Angular (общее ознакомление).**
> 
