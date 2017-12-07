# Изучение Главы 4 книги "Professional ASP.NET MVC 5".
              
Практическое задание к Главе 4: создайте приложение, которое будет позволять создавать, редактировать, удалять, просматривать информацию о городах (контроллеры, действия, представления создать, используя возможности Scaffolding). При запуске приложения локальная база данных приложения должна содержать информацию о трех городах (Лондон, Париж, Рим). Модель данных города должна содержать два поля: id и название города. Просмотрите объекты созданной базы данных (команда меню View -> SQL Server Object Explorer или View -> Server Explorer) и получите T-SQL команду создания таблицы городов в базе данных.
              
Контрольные вопросы к Главе 4:
- Как создать модель?
> Модели представляют собой простые классы и располагаются в проекте в каталоге Models. Модели описывают логику данных.

> To add a new Album class to the Models folder, right-click the Models folder, select Add… Class, and name the class Album. Leave the existing using and namespace statements intact and enter the properties.
- Как скомпилировать приложение, не запуская его и почему может возникнуть необходимость предварительной компиляции при работе с моделями?
> You can compile your application either with the Visual Studio Build ➪ Build Solution menu item or the keyboard shortcut, Ctrl+Shift+B. Compiling your newly added model classes is important for two reasons:

> ➤ It serves as a quick check to catch any simple syntax errors.

> ➤ Nearly as important, the newly added classes won’t show up in the Visual Studio scaffolding dialogs in the next section until you’ve compiled the application.
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
> Eager Loading
Суть Eager Loading заключается в том, чтобы использовать для подгрузки связанных по внешнему ключу данных метод Include. 
An eager loading strategy attempts to load all data using a single query
> Lazy Loading
Еще один способ представляет так называемая "ленивая загрузка" или lazy loading. При таком способе подгрузки при первом обращении к объекту, если связанные данные не нужны, то они не подгружаются. Однако при первом же обращении к навигационному свойству эти данные автоматически подгружаются из бд.
With
lazy loading, EF loads only the data for the primary object in the LINQ query (the
album), and leaves the Genre and Artist properties unpopulated

- Конфигурация источников данных. Что если не сконфигурировать? Что если не создать таблицы?
> If you don’t confi gure specifi c mappings from your models to database tables and columns, EF uses
conventions to create a database schema. If you don’t confi gure a specifi c database connection to use
at runtime, EF creates one using a convention.

> This allows you to control the context’s database connections in two ways.
First, you can modify the connection string in web.config.
Second, you can override the database name EF will use for a given DbContext by
altering the name argument passed into the DbContext’s constructor.

> Without a specifi c connection confi gured, EF tries to connect to a LocalDB instance of SQL Server
and fi nd a database with the same name as the DbContext derived class. If EF can connect to the
database server, but doesn’t fi nd a database, the framework creates the database. 

> The EF automatically creates tables to store album, artist, and genre information. The framework
uses the model’s property names and data types to determine the names and data types of the table
column. Notice how the framework also deduced each table’s primary key column and the foreign
key relationships between tables.
- Как посмотреть табличные данные и объекты базы данных в локальной базе?
> команда меню View -> SQL Server Object Explorer или View -> Server Explorer или SSMS
- Как Entity Framework добивается синхронизации таблиц и моделей? Какие стратегии инициализации источника данных можно задать в приложении ASP.NET MVC? Миграция данных при изменении модели.
- Seeding a Database. Initializer Seeds vs Migration Seeds
> In this case you can derive a class from the DropCreateDatabaseAlways class and override the Seed
method. The Seed method enables you to create some initial data for the application.
Calling into the base class implementation of the Seed method saves your new objects into the database.
For the new database initializer to work, you need to change the application startup code to register the initializer.
> Все действия по инициализации происходят в методе Seed, а сама инициализация предполагает простое сохранение данных в бд с помощью контекста данных.

> Чтобы инициализатор сработал, надо его вызвать. Один из способов вызова инициализатора предполагет вызов его в статическом конструкторе класса контекста.

> Migrations also support seed methods, so when you make the move from the
quick and easy database initializer approach to the more sophisticated migrations
approach, you’ll want to convert any necessary seed methods to work with
your migrations.
You need to be aware of an important difference between initializer seeds and
migration seeds. Because a database initializer seed method runs against an empty
database, you don’t need to worry about inserting duplicate data. Migration seed
methods run every time you update the database, so you’ll need to take care to prevent
adding duplicate data if your seed runs multiple times on the same database.
The DbSet.AddOrUpdate() extension method was added to EF 4.3 and above to
make this easier.
- View-specific models.
> The
album edit scenario is a good example, where your model object (an Album object) doesn’t quite
contain all the information required by the view. You need the lists of all possible genres and artists,
too. Two possible solutions exist to this problem .
The scaffolding-generated code demonstrates the fi rst option: pass the extra information along in
the ViewBag structure. This solution is entirely reasonable and easy to implement, but some people
want all the model data to be available through a strongly typed model object.
The strongly typed model fans will probably look at the second option: build a view-specifi c model
to carry both the album information and the genre and artists information to a view. Such a model
might use the following class defi nition:
public class AlbumEditViewModel
{
 public Album AlbumToEdit { get; set; }
 public SelectList Genres { get; set; }
 public SelectList Artists { get; set; }
}
Instead of putting information in ViewBag, the Edit action would need to instantiate the
AlbumEditViewModel, set all the object’s properties, and pass the view model to the view. 
- GET, POST запросы.
> 

> 
- Model Binding, DefaultModelBinder.
> Механизм, который способствует сопоставлению значений с используемыми в методах контроллера параметрами. И этот механизм называется привязкой модели.

> Привязчик DefaultModelBinder используется по умолчанию, если для данного типа не определен другой привязчик. Чтобы DefaultModelBinder мог связать значение с параметром, элемент данных запроса должен обязательно иметь то же имя, что и параметр.
In the case of an Album object, the default model binder inspects the album and fi nds all the album properties available for
binding. Following the naming convention you examined earlier, the default model binder can automatically
convert and move values from the request into an album object (the model binder can also
create an instance of the object to populate).
In other words, when the model binder sees that an Album has a Title property, it looks for a value
named “Title” in the request. Notice the model binder looks “in the request” and not “in the form
collection.” The model binder uses components known as value providers to search for values in
different areas of a request. The model binder can look at route data, the query string, and the form
collection, and you can add custom value providers if you so desire.
- Explicit Model Binding. Model state.
> При использовании параметра в методе действия привязка модели работает неявно. Но мы можем вызвать на контроллере и явную привязку модели с помощью методов UpdateModel и TryUpdateModel. Если модель не прошла валидацию, то метод UpdateModel выбрасывает исключение.
TryUpdateModel также вызывает привязку модели, но не выбрасывает исключение. Этот метод возвращает значение типа bool - если это значение равно true, модель прошла привязку, если false, то валидация прошла неудачно.

> A byproduct of model binding is model state. For every value the model binder moves into a model,
it records an entry in model state. You can check model state any time after model binding occurs to
see whether model binding succeeded. ModelState.IsValid

> Объект ModelState сохраняет все значения, которые пользователь ввел для свойств модели, а также все ошибки, связанные с каждым свойством и с моделью в целом. Если в объекте ModelState имеются какие-нибудь ошибки, то свойство ModelState.IsValid возвратит false
- Model Binding Security & Over-posting Attack.
> Sometimes the aggressive search behavior of the model binder can have unintended
consequences. You’ve already seen how the default model binder looks at
the available properties on an Album object and tries to fi nd a matching value for
each property by looking around in the request. Occasionally there is a property
you don’t want (or expect) the model binder to set, and you need to be careful to
avoid an “over-posting” attack. A successful over-posting attack might allow a
malicious person to destroy your application and your data, so do not take this
warning lightly.
ASP.NET MVC 5 now includes a comment with warning about over-posting
attacks as well as the Bind attribute that restricts the binding behavior.

> As mentioned,
in addition to using the Bind attribute to restrict implicit model binding,
you can also restrict binding when you use UpdateModel and TryUpdateModel.
Both methods have an override allowing you to specify an includeProperties
parameter. This parameter contains an array of property names you’re explicitly
allowing to be bound, as shown in the following code:
UpdateModel(product, new[] { "Title", "Price", "AlbumArtUrl" });

