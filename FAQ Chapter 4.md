# Изучение Главы 4 книги "Professional ASP.NET MVC 5".
              
Практическое задание к Главе 4: создайте приложение, которое будет позволять создавать, редактировать, удалять, просматривать информацию о городах (контроллеры, действия, представления создать, используя возможности Scaffolding). При запуске приложения локальная база данных приложения должна содержать информацию о трех городах (Лондон, Париж, Рим). Модель данных города должна содержать два поля: id и название города. Просмотрите объекты созданной базы данных (команда меню View -> SQL Server Object Explorer или View -> Server Explorer) и получите T-SQL команду создания таблицы городов в базе данных.
              
Контрольные вопросы к Главе 4:
- Как создать модель?
> Модели представляют собой простые классы и располагаются в проекте в каталоге Models. Модели описывают логику данных.

> To add a new Album class to the Models folder, right-click the Models folder, select Add… Class, and name the class Album. Leave the existing using and namespace statements intact and enter the properties.
- Как скомпилировать приложение, не запуская его и почему может возникнуть необходимость предварительной компиляции при работе с моделями?
> You can compile your application either with the Visual Studio Build ➪ Build Solution menu item or the keyboard shortcut, Ctrl+Shift+B. Compiling your newly added model classes is important for two reasons:

> ➤ It serves as a quick check to catch any simple syntax errors.

> ➤ Nearly as important, the newly added classes won’t show up in the Visual Studio scaffolding dialogs in the next section until you’ve compiled the application. Compiling before using the scaffolding system is not just a good p
- Scaffolding: что такое, опишите имеющиеся шаблоны.
> Шаблоны формирования позволяют по заданной модели и контексту данных сформировать весь необходимый базовый код контроллеров, а также всю разметку для представлений, с помощью которых можно управлять записями в БД.

>  The scaffolding knows how to name controllers, how to name views, what code needs to go in each component, and where to place all these pieces in the project for the application to work.

> ### MVC 5 Controller—Empty
Этот шаблон добавляет в папку Controllers пустой контроллер, который имеет один единственный метод Index. Данный шаблон не создает представлений.
> ### MVC 5 Controller with read/write Actions
Данный шаблон добавляет в проект контроллер, который содержит методы Index, Details, Create, Edit и Delete. Однако эти методы не содержат никакой логики работы с базой данных. И нам предлагается самим создать для них код и представления.
> ### Web API 2 API Controller Scaffolders
Several templates add a controller derived from the ApiController base class. You can use these templates to build a Web API for your application. Chapter 11 covers Web API in more detail.
> ### MVC 5 Controller with Views, Using Entity Framework
Шаблон, который создает контроллер с методами Index, Details, Create, Edit и Delete, а также все необходимые представления для этих действий и добавляет код для извлечения информации из базы данных.
- Что такое Entity Framework? Code-first и связанные соглашения об именовании, custom conventions.
> EF is an object-relational mapping (ORM) framework and understands how to store .NET objects in a relational database and retrieve those same objects given a LINQ query.

> Entity Framework позволяет абстрагироваться от написания sql-запросов, от строения базы данных и полностью сосредоточиться на логике приложения.

> Entity Framework представляет специальную объектно-ориентированную технологию на базе фреймворка .NET для работы с данными. Если традиционные средства ADO.NET позволяют создавать подключения, команды и прочие объекты для взаимодействия с базами данных, то Entity Framework представляет собой более высокий уровень абстракции, который позволяет абстрагироваться от самой базы данных и работать с данными независимо от типа хранилища. Если на физическом уровне мы оперируем таблицами, индексами, первичными и внешними ключами, но на концептуальном уровне, который нам предлагает Entity Framework, мы уже работает с объектами.

> Code first: разработчик создает класс модели данных, которые будут храниться в бд, а затем Entity Framework по этой модели генерирует базу данных и ее таблицы.

> Entity Framework поддерживает подход "Code first", который предполагает сохранение или извлечение информации из БД на SQL Server без создания схемы базы данных или использования дизайнера в Visual Studo. Наоборот, мы создаем обычные классы, а Entity Framework уже сам определяет, как и где сохранять объекты этих классов.

>  if you want to store an object of type Album in the database, EF assumes you want to store the data in a table named Albums. If you have a property on the object named ID, EF assumes the property holds the primary key value and sets up an auto-incrementing (identity) key column in SQL Server to hold the property value.

> Названия столбцов получают названия свойств модели.

> You can use custom conventions to override primary key defi nitions, or to change the table mapping defaults to meet your teams naming conventions. Better still, you can create reusable convention classes and attributes that you can apply to any model or property. This gives you the best of both worlds: you get the power of confi guring things exactly how you’d like them with the ease and simplicity of standard EF conventional development.

> Если нас не устраивают названия таблиц и столбцов по умолчанию, то мы можем переопределить данный механизм с помощью Fluent API или аннотаций.
- Контекст данных.
> Для подключения к базе данных через Entity Framework, нам нужен посредник - контекст данных. Контекст данных представляет собой класс, производный от класса DbContext. Контекст данных содержит одно или несколько свойств типа DbSet<T>, где T представляет тип объекта, хранящегося в базе данных.

> You can think of a DbSet<T> as a special, data-aware generic list that knows how to load and save data from its parent context. 

- Eager loading vs lazy loading и соображения производительности.
> 
- Конфигурация источников данных. Что если не сконфигурировать? Что если не создать таблицы?
- Как посмотреть табличные данные и объекты базы данных в локальной базе?
> команда меню View -> SQL Server Object Explorer или View -> Server Explorer или SSMS
- Как Entity Framework добивается синхронизации таблиц и моделей? Какие стратегии инициализации источника данных можно задать в приложении ASP.NET MVC? Миграция данных при изменении модели.
- Seeding a Database. Initializer Seeds vs Migration Seeds
> In this case you can derive a class from the DropCreateDatabaseAlways class and override the Seed
method. The Seed method enables you to create some initial data for the application.
Calling into the base class implementation of the Seed method saves your new objects into the database.
For the new database initializer to work, you need to change the application startup code to register the initializer.
> Все действия по инициализации происходят в методе Seed, а сама инициализация предполагает простое сохранение данных в бд с помощью контекста данных.

>Чтобы инициализатор сработал, надо его вызвать. Один из способов вызова инициализатора предполагет вызов его в статическом конструкторе класса контекста.
- View-specific models.
- GET, POST запросы.
- Model Binding, DefaultModelBinder.
- Explicit Model Binding. Model state.
- Model Binding Security & Over-posting Attack.
