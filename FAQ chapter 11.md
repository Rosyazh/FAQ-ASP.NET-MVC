# Изучение Главы 11 книги "Professional ASP.NET MVC 5".

Практическое задание к Главе 11:
- создайте приложение по шаблону ASP.NET MVC без авторизации/аутентификации с поддержкой Web API (соответствующая опция при создании проекта), настройте маршрутизацию для Web API, если она не настроена по умолчанию, создайте модель городов со свойствами Id, Name
- создайте контроллер для обработки Web API запросов на просмотр, добавление, обновление, удаление городов.
- протестируйте GET запросы через браузер, замените XML на JSON формат (результат метода действия JsonResult<T> и метод, генерирующий JSON из объекта Json<T>). POST, PUT, DELETE запросы должны возвращать HTTP код результата, в случае ошибки и текст ошибки (возвращаемый тип HttpResponseMessage и метод, генерирующий результат, например: return Request.CreateErrorResponse(HttpStatusCode.BadRequest, "Data not inserted");). Замените параметр методов POST и PUT на обект класса города, возвращаемый результат замените на объект JSON, например: return Json<string>("Success"); или return Json<T>(new T {});
- протестируйте работу API POST, PUT, DELETE при помощи Google Chrome расширения Postman https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop (выберите тип API, вставьте URL, например DELETE и http://localhost:52459/api/values/5 , во вкладке Headers установите Content-Type в application/json, во вкладке Body установите raw и введите JSON спецификацию, например:
{
    "Name": "Sample"
}
нажмите Send и посмотрите возвращаемый результат, при необходимости получения дополнительной информации обратитесь к справке по Postman)

Контрольные вопросы к Главе 11:
- В чем отличия ASP.NET Web API от ASP.NET MVC?
- Какой базовый класс и из какого пространства имен используется при создании контроллеров API? Какие методы и какие аттрибуты?
- Параметры методов действий Web API контроллера.
- Результаты методов действий Web API контроллера.
- Конфигурация Web API.
- Configuration in Web-Hosted Web API.
- Configuration in Self-Hosted Web API (общее ознакомление). Configurating WCF Self-Host (общее ознакомление). Configuration for OWIN Self-Host (общее ознакомление).
- Добавление маршрутов. Маршрутизация посредством аттрибутов.
- Привязка параметров, аттрибуты.
- Фильтрование запросов.
- Enabling Dependency Injection (общее ознакомление).
- Exploring API's programmatically (общее ознакомление).
- Tracing the Application (общее ознакомление).
