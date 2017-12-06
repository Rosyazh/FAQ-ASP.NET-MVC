# FAQ-ASP.NET-MVC

Изучение Главы 1 книги "Professional ASP.NET MVC 5".

Практическое задание к Главе 1: Создайте простое MVC приложение с конфигурацией по умолчанию. Запустите приложение в веб браузере. Изучите структуру каталогов и вспомогательных файлов приложения.

Контрольные вопросы к Главе 1:
- Принцип работы MVC.
> Концепция паттерна (шаблона) MVC (model - view - controller) предполагает разделение приложения на три компонента:
Контроллер (controller) представляет класс, обеспечивающий связь между пользователем и системой, представлением и хранилищем данных. Он получает вводимые пользователем данные и обрабатывает их. И в зависимости от результатов обработки отправляет пользователю определенный вывод, например, в виде представления.
Представление (view) - это собственно визуальная часть или пользовательский интерфейс приложения. Как правило, html-страница, которую пользователь видит, зайдя на сайт.
Модель (model) представляет класс, описывающий логику используемых данных.
- Свойства ASP.NET MVC приложения.
> 
- Что такое ASP.NET Web API?
> ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices. ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.
> оптимизирует работу с HTTP запросами и ответами
- Какие библиотеки .js и .css включены в шаблон ASP.NET MVC приложения по умолчанию?
> Jquery, Bootstrap
- Что такое маршрутизация?
> В ASP.NET MVC 5 все определения маршрутов находятся в файле RouteConfig.cs, который располагается в проекте в папке App_Start. Если мы на него посмотрим, то увидим настройки маршрута по умолчанию
> Система маршрутизации обеспечивает выполнение двух функций:
Исследование входящих URL и выяснение, для каких контроллеров и действий они предназначены. Как и следовало ожидать, это то, что система маршрутизации должна делать при получении клиентского запроса.
Генерация исходящих URL. Это URL, которые появляются в HTML-разметке, визуализированной из представлений, так что специфическое действие может быть инициировано, когда пользователь производит щелчок на ссылке (в этот момент ссылка снова становится входящим URL).
- Фильтры в ASP.NET MVC.
> Фильтры реализованы как атрибуты, благодаря чему позволяют уменьшить объем кода в контроллере. Данные атрибуты могут применяться как ко всему классу, так и к отдельным его методам, свойствам и полям. Например, если у нас есть некоторый контроллер HomeController, и мы хотим, чтобы к нему имели доступ только авторизованные пользователи.
- Как осуществляется поддержка браузеров мобильных устройств?
> через Bootstrap респонс дизайн, и мобильные версии скриптов jquery
- Bundling, минификация: назначение и как поддерживается в ASP.NET MVC.
> концепция бандлов, которая помогает организовать файлы скриптов и стилей более эффективным путем для снижения издержек при передаче клиенту
> Ключевым моментом концепции бандлов является минификация. Ее суть состоит в том, что при развертывании приложения клиенту отдается не полная, а минимизированная версия скриптов или стилей. За счет чего экономятся ресурсы сервера, так как идет отдача файла с меньшим объемом. В то же время в процессе отладки приложение отдает обычную версию, поскольку благодаря ей, нам проще разобраться в возможных ошибках.
- Что такое View engine?
> 
- Какие еще шаблоны существуют в ASP.NET помиом MVC?
> web api, web forms, spa, empty, azure mobile service, facebook
- Как создать Unit Tests проект?
> при создании проекта - галочка, либо самим добавлять проект с тестами
- Начальная конфигурация аутентификации при создании проекта.
> No Authentification (сайты, где не нужна аутентификация пользователей), Individual User Accounts (сайты, где хранятся профили пользователей локально), Organizational Accounts (сайты, где используются аккаунты из других платформ (Active Directory)), Windows Authentication (сайты, где используется внутренняя аутентификация внутри одной сети)
- Назовите основные каталоги и их назначение в структуре приложения ASP.NET MVC.
> [Controllers] классы контроллеров, которые обрабатывают URL запросы
> [Models] классы, которые представляют и управляют данными
> [Views] UI шаблоны которые отвечают за рендеринг HTML
> [Scripts] JS скрипты и файлы библиотек
> [fonts] шрифты
> [Content] изображения, CSS файлы и др. файлы
> [App_Data] храним файлы, которые читаются и записываются
> [App_Start] храним конфигурационные файлы Routing, bundling, Web API
- Соглашения о наименовании контроллеров и каталогов представлений.
> directory-naming structure, позволяет не пописывать пути к тем или иным файлам, ASP.NET MVC автоматом определяет файл представления для контроллера
> классы контроллеров обзываются на конце суффиксом контроллер
> у представлений такое же название, как и у контроллеров, ток без суффикса
> /Controllers
> /Views
> /Contents
> /Scripts
> project for Unit Tests