# 1) Изучение раздела "Understanding unit-testing and test driven development" Главы 14 книги "Professional ASP.NET MVC 5".
# 2) Изучение Главы 18 онлайн самоучителя    https://metanit.com/sharp/mvc5/

Ознакомиться с аспектами и видами тестирования ПО можно здесь: https://en.wikipedia.org/wiki/Software_testing

Практическое задание к Главе 14:
- создайие проект по шаблону ASP.NET MVC приложения, сразу включив опцию модульного тестирования, запустите модульное тестирование и убедитесь, что результаты выполнения трех тестов появляются во вкладке Test Explorer, в случае если результаты тестировния не появляются во вкладке Test Explorer, скомпилируйте проект (Build) и если не помогло - перезагрузите Visual Studio
- создайте простую модель, например Города и добавьте простой контроллер и представление для действия, создайте тесты для тестирования созданного метода действия
- добавьте валидацию в модель и добавьте тест для проверкиработы валидации
- добавьте переадресацию в метод действия и добавьие тест для проверки работы переадресации

Контрольные вопросы к Главе 14:
- **Определение модульного тестирования. Тестирование небольших фрагментов кода. Изолированное тестирование. Тестирование только общедоступных конечных точек. Автоматические результаты. Модульное тестирование как деятельность по повышению качества.**
> *Тестирование небольших участков кода ("юнитов")*
> При создании юнит-тестов выбираются небольшие участки кода, которые надо протестировать. Как правило, тестируемый участок должен быть меньше класса, а в большинстве случаев тестируется отдельный метод класса. Упор на небольшие участки позволяет довольно быстро писать простенькие тесты.
> Однажды написанный код нередко читают многократно, поэтому важно писать понятный код. Особенно это важно в юнит-тестах, где в случае неудачи при тестировании разработчик должен быстро прочитать исходный код и понять в чем проблема и как ее исправить. А использование небольших участков кода значительно упрощает подобную работу.

> *Тестирование в изоляции от остального кода*
> При тестировании важно изолировать тестируемый код от остальной программы, с которой он взаимодействует, чтобы потом четко определить возможность ошибок именно в этом изолированном коде. Что упрощает и повышает контроль над отдельными компонентами программы.

> *Тестирование только общедоступных конечных точек*
> Всего лишь небольшие изменения в классе могут привести к неудаче многих юнит-тестов, поскольку реализация используемого класса изменилась. Поэтому при написании юнит-тестов ограничиваются только общедоступными конечными точками, что позволяет изолировать юнит-тесты от многих деталей внутренней реализации компонента. В итоге уменьшается вероятность, что изменения в классах могут привести к провалу юнит-тестов.

> *Автоматизация тестов*
> Написание юнит-тестов для небольших участков кода ведет к тому, что количество этих юнит-тестов становится очень велико. И если процесс получения результатов и проведения тестов не автоматизирован, то это может привести к непроизводительному расходу рабочего времени и снижению производительности. Поэтому важно, чтобы результаты юнит-тестов представляли собой простое решение, означающее, пройден тест или нет. Для автоматизации процесса разработчики обычно обращаются к фреймворкам юнит-тестирования

> *Unit Testing as a Quality Activity*
> Most developers choose to write unit tests because it increases the quality of their software. In this
situation, unit testing acts primarily as a quality assurance mechanism, so writing the production
code fi rst, and then writing the unit tests afterward, is fairly common for developers. Developers use 
their knowledge of the production code and the desired end-user behavior to create the list of tests
that help assure them that the code behaves as intended.
> Unfortunately, weaknesses exist with this ordering of tests after production code. Developers can
easily overlook some piece of the production code that they’ve written, especially if the unit tests
are written long after the production code was written. Writing production code for days or weeks
before getting around to the fi nal part of unit testing is not uncommon for developers, and it
requires an extremely detail-oriented person to ensure that every avenue of the production code is
covered with an appropriate unit test. What’s worse, after several weeks of coding, developers are
more likely to want to write more production code than to stop and write unit tests. TDD works to
solve some of those shortcomings.

- **Разработка через тестирование. Красный/зеленый цикл. Рефакторинг.**
> Разработка через тестирование (TDD) процесс применения юнит-тестов, при котором сначала пишутся тесты, а потом уже программный код, достаточный для выполнения этих тестов.
> Использование TDD позволяет снизить количество потенциальных багов в приложении. Создавая тесты перед написанием кода, мы тем самым описываем способ поведения будущих компонентов, не связывая себя при этом с конкретной реализацией этих тестируемых компонентов (тем более что реализация на момент создания теста еще не существует). Таким образом, тесты помогают оформить и описать API будущих компонентов.

> Порядок написания кода при TTD довольно прост:
>> 1. Пишем юнит-тест
>> 2. Запускаем его и видим, что он завершился неудачей (программный код ведь еще не написан)
>> 3. Пишем некоторое количество кода, достаточное для запуска теста
>> 4. Снова запускаем тест и видим его результаты
> Этот цикл повторяется снова и снова, пока не будет закончена работа над программным кодом. Так как большинство фреймворков юнит-тестирования помечают неудавшиеся тесты с красного цвета (например, выводится текст красного цвета), а удачный тест отмечается зеленым цветом (опять же выводится текст зеленого цвета), то данный цикл часто называют красным/зеленым циклом.

- **Создание проекта для модульного тестирования.**
> Посмотрим на примере, как создавать юнит-тесты. По умолчанию при создании проекта в любой версии Visual Studio 2013 для создаваемого проекта веб-приложения нам уже предлагается включить дополнительный проект с тестами с помощью опции Add unit tests.

> Если вдруг нам надо добавить тесты к уже существующему проекту, то мы можем добавить в решение новый тип проекта Unit Test Project. Как правило, для проекта тестов название выбирается по следующей схеме [название_главного_проекта].Tests.

- **Создание тестов модульного тестирования. Аттрибуты классов и методов тестирования.**
> Теперь перейдем к проекту тестов и добавим в него новый класс тестов. Для этого мы можем добавить либо стандартный класс, либо использовать специальный шаблон файлов. Для этого в проекте тестов нажмем правой кнопкой мыши на каталог Controllers и в появившемся контекстном меню выберем Add->Unit Test...

```c#
using System;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using UnitTestApp.Controllers;
using System.Web.Mvc;
 
namespace UnitTestApp.Tests.Controllers
{
    [TestClass]
    public class StoreControllerTest
    {
        [TestMethod]
        public void IndexViewResultNotNull()
        {
            StoreController controller = new StoreController();
 
            ViewResult result = controller.Index() as ViewResult;
 
            Assert.IsNotNull(result);
        }
    }
}
```
> Атрибут TestInitialize позволяет задать метод, который выполняет начальную инициализацию для каждого отдельного теста. Благодаря этому код сокращен, а в тестовых методах оставлены только части Assert. Однако подобный подход надо принимать с осторожностью, так как он осложняет возможности по изменению кода. В данном случае общий контекст очень прост, но если при изменении методов будет изменяться и их контекст, то придется вносить большие изменения во всех классах тестов, а не только в отдельный метод для тестов.

- **Класс Assert и его методы.**
> Класс Assert из пространства имен Microsoft.VisualStudio.TestTools.UnitTesting с помощью своих статических методов позволяет верифицировать результат выполнения некоторого действия. Ранее уже было рассмотрено несколько методов, в частности, метод 
> 1. Assert.IsNotNull(), проверяющий, не равен ли некоторый объект значению null. Кроме того, при тестировании нам доступен еще ряд методов:

> 2. AreEqual(object expected, object actual): проверяет, равны ли оба объекта. Имеет различные перегруженные версии, позволяющие сравнивать различные типы объектов

> 3. AreEqual<T>(T expected, T actual): обобщенная версия предыдущего метода. Например, Assert.AreEqual<string>("Index", result.MasterName)

> 4. AreNotEqual(object expected, object actual): проверяет, не равны ли оба объекта. Тест проходит успешно, если объекты не равны

> 5. AreNotEqual<T>(T expected, T actual): обобщенная версия предыдущего метода

> 6. AreSame(object expected, object actual): проверяет, указывают ли оба объекта на один и тот же объект в памяти

> 7. AreNotSame(object expected, object actual): проверяет, указывают ли оба объекта на разные объекты в памяти. Если они указывают на один и тот же объект, то тест заканчивается неудачно

> 8. Equals(object objA, object objB): проверяет на равенство оба объекта

> 9. IsFalse(bool condition): проверяет, равно ли условие condition значению false

> 10. IsTrue(bool condition): проверяет, равно ли условие condition значению true

> 11. IsNull(object value): проверяет, имеет ли объект value значение null

> 12. IsInstanceOfType(object value, Type expectedType): проверяет, представляет ли объект value тип expectedType

- **Слабосвязанные объекты и тестирование работы с БД (общее ознакомление), фреймворк Moq (общее ознакомление)**
> Такой метод сложно тестировать в силу жесткой связи с базой данных. Если что-то случится с базой данных, и она окажется недоступна, тогда тест завершится неудачей. Но это не значит, что что-то не так в самом коде или что логика построена неправильно.
> Поэтому таких ситуаций обычно избегают. И для этого вместо жесткой связи объектов используют слабосвязанные объекты (loosely coupled objects).
> Непосредственно в данном случае проблема решается перенесением уровня взаимодействия с бд в специальный класс, который представляет реализацию паттерна репозиторий.
> Для этого добавим в главный проект новый интерфейс и класс, его реализующий:
```c#
interface IRepository : IDisposable
{
    List<Computer> GetComputerList();
    Computer GetComputer(int id);
    void Create(Computer item);
    void Update(Computer item);
    void Delete(int id);
    void Save();
}
 
class ComputerRepository : IRepository
{
    private CompContext db;
    public ComputerRepository()
    {
        this.db = new CompContext();
    }
    public List<Computer> GetComputerList()
    {
        return db.Computers.ToList();
    }
    public Computer GetComputer(int id)
    {
        return db.Computers.Find(id);
    }
  
    public void Create(Computer c)
    {
        db.Computers.Add(c);
    }
  
    public void Update(Computer c)
    {
        db.Entry(c).State = EntityState.Modified;
    }
  
    public void Delete(int id)
    {
        Computer c = db.Computers.Find(id);
        if(c!=null)
            db.Computers.Remove(c);
    }
  
    public void Save()
    {
        db.SaveChanges();
    }
  
    private bool disposed = false;
  
    public virtual void Dispose(bool disposing)
    {
        if(!this.disposed)
        {
            if(disposing)
            {
                db.Dispose();
            }
        }
        this.disposed = true;
    }
  
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
}
```
> Теперь в контроллере установим взаимодействие с базой данных через репозиторий:
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;
using MoqMvcApp.Models;
 
namespace MoqMvcApp.Controllers
{
    public class HomeController : Controller
    {
        IRepository repo;
 
        public HomeController(IRepository r)
        {
            repo = r;
        }
        public HomeController()
        {
            repo = new ComputerRepository();
        }
 
        public ActionResult Index()
        {
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            repo.Dispose();
            base.Dispose(disposing);
        }
    }
}
```
> Теперь мы избавились от жесткой связи с базой данных. Но теперь нам надо будет в тестах сымитировать объект репозитория. И в этом нам поможет Moq-фреймворк.
> Кроме собственно фреймворков для создания и проведения юнит-тестов при тестировании часто бывают полезны такие фреймворки, которые позволяют имитировать или эмулировать какую-то функциональность или создавать мок-объекты. Подобных фреймворков существует множество, и одним из самых популярных является Moq.
> Теперь изменим класс тестов следующим образом:
```c#
using Moq;
.................
 
[TestClass]
public class HomeControllerTest
{
    [TestMethod]
    public void IndexViewModelNotNull()
    {
        // Arrange
        var mock = new Mock<IRepository>();
        mock.Setup(a => a.GetComputerList()).Returns(new List<Computer>());
        HomeController controller = new HomeController(mock.Object);
 
        // Act
        ViewResult result = controller.Index() as ViewResult;
 
        // Assert
        Assert.IsNotNull(result.Model);
    }  
}
```
> Для доступа к функциональности Moq вначале подключается соответствующее пространство имен using Moq;.
> Moq предназначен для имитации объектов. В данном случае имитируется функциональность репозитория. Для этого объект Mock типизируется соответствующим типом: var mock = new Mock<IRepository>()
> Затем выполняется настройка mock объекта с помощью метода Setup. Так как нам надо имитировать возвращение методом GetComputerList() набора объектов, то данный метод вызывается в методе Setup, а с помощью метода Returns определяем данный набор объектов.
> Поскольку контроллер HomeController теперь в конструкторе принимает объект репозитория, то мы можем передать в конструктор мок-объект, который имитирует функциональность репозитория: HomeController controller = new HomeController(mock.Object)
> Теперь запустим тест. Он должен завершиться неудачей, так как у нас не передается в методе Index в представление никакой модели. 
> Поэтому изменим метод Index:

```c#
public ActionResult Index()
{
    var model = repo.GetComputerList();
    return View(model);
}
```

- **Тестирование создания (состояния, валидации) модели и переадресации**
> Рассмотрим на примере тестирование создания модели и переадресации. Во-первых, нам нужен минимальный код для запуска тестов. Для этого возьмем проект веб-приложения из прошлой темы и определим в нем стандартный метод Create:
```c#
public ActionResult Create()
{
    return View();
}
[HttpPost]
public ActionResult Create(Computer c)
{
    if(ModelState.IsValid)
    {}
    return View("Create");
}
```
> Это минимальный код, достаточный для написания тестов. Теперь в класс тестов HomeControllerTest добавим ряд методов:
```c#
[TestMethod]
public void CreatePostAction_ModelError()
{
    // arrange
    string expected = "Create";
    var mock = new Mock<IRepository>();
    Computer comp = new Computer();
    HomeController controller = new HomeController(mock.Object);
    controller.ModelState.AddModelError("Name", "Название модели не установлено");
    // act
    ViewResult result = controller.Create(comp) as ViewResult;
    // assert
    Assert.IsNotNull(result);
    Assert.AreEqual(expected, result.ViewName);
}
```
> В данном методе проверяется механизм валидации. Через объект контроллера мы можем добавить ряд ошибок к свойству ModelState. И в случае ошибок, идет обращение к представлению Create.

> Добавим в класс тестов следующий метод:
```c#
[TestMethod]
public void CreatePostAction_RedirectToIndexView()
{
    // arrange
    string expected = "Index";
    var mock = new Mock<IRepository>();
    Computer comp = new Computer();
    HomeController controller = new HomeController(mock.Object);
    // act
    RedirectToRouteResult result = controller.Create(comp) as RedirectToRouteResult;
             
    // assert
    Assert.IsNotNull(result);
    Assert.AreEqual(expected, result.RouteValues["action"]);
}
```
> Тестирование переадресации делается также просто, как и тестирование представлений. Объект RedirectToRouteResult, который представляет переадресацию, имеет свойство RouteValues, которое позволяет получить данные маршрута - в данном случае получаем метод и сравниваем его с ожидаемым.
> И поскольку данный тест опять завершится неудачно, изменим метод Create:
```c#
[HttpPost]
public ActionResult Create(Computer c)
{
    if(ModelState.IsValid)
    {
        return RedirectToAction("Index");
    }
    return View("Create");
}
```
> И теперь добавим третий метод тестов:
```c#
[TestMethod]
public void CreatePostAction_SaveModel()
{
    // arrange
    var mock = new Mock<IRepository>();
    Computer comp = new Computer();
    HomeController controller = new HomeController(mock.Object);
    // act
    RedirectToRouteResult result = controller.Create(comp) as RedirectToRouteResult;
    // assert
    mock.Verify(a => a.Create(comp));
    mock.Verify(a => a.Save());
}
```
> В тестах мы легко можем взять результат тестируемых методов и сравнить этот результат с определенным значением, чтобы понять, правильно ли все работает. Но не все методы возвращают определенные значения. Некоторые методы возвращают тип void, однако и тоже необходимо тестировать. И чтобы протестировать подобные методы, в классе Mock определен метод Verify.
> Его синтаксис прост: mock.Verify(a=>a.Method(parameter)), где mock - это объект Mock, а parameter - значение, передаваемое в качестве параметра в метод.
> В данном случае вызывается метод Create, в который передается модель для сохранения, и метод Save.
> Опять же тест не сработает, поэтому изменим метод Create следующим образом:
```c#
[HttpPost]
public ActionResult Create(Computer c)
{
    if(ModelState.IsValid)
    {
        repo.Create(c);
        repo.Save();
        return RedirectToAction("Index");
    }
    return View("Create");
}
```
