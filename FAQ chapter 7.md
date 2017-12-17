# Изучение Главы 7 книги "Professional ASP.NET MVC 5".
              
Практическое задание к Главе 7:
- создайте приложение с аутентификацией, предусматривающей индивидуальные учетные записи пользователей (см. соответвующую опцию)
- в созданном приложении посмотрите код контроллеров и приведите пример контроллера, обязывающего пройти аутентификацию/авторизацию, приведите пример действия контроллера, явно разрешающего анонимный доступ и объясните, зачем в данном случае разрешен анонимный доступ
- запретите анонимный доступ к контроллеру HomeController, явно разрешите анонимный доступ к действию About контроллера Home, запретите анонимный доступ ко всем контроллерам приложения, разрешите доступ к действию Index контроллера HomeController только для пользователя John и для пользователей с ролью Admin
- добавьте свойство Address в учетную запись, обновив классы RegisterViewModel (AccountViewModels.cs), ApplicationUser (IdentityModels.cs) и соответвующие действие контроллера и представление для регистрации учетной записи. Есть пример добавления новых свойств в учетную запись: https://blogs.msdn.microsoft.com/webdev/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates/
- добавьте возможность обращение к вашему приложению при использовании SSL/HTTPS, для контроллера Home разрешите обращение только посредством SSL/HTTPS
- добавьте возможность логиниться посредством сторонней аутентификации, одного из распространенных провайдеров аутентификации, например Google
(инструкция: https://docs.microsoft.com/en-us/aspnet/mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
используйте URL без / в конце: https://localhost:44300
вызов NuGet: TOOLS > NuGet Package Manager > Manage NuGet Packages for Solution)
- проверьте представление Index для контроллера Home, обеспечивается ли предотвращение XSS-аттак
- проверьте действия контроллера Home, установлен ли аттрибут, предотвращающий Cross-Site Request Forgery аттаку, если не установлен, то установите его
- обеспечьте сохранность кукисов (для модели учетной записи), используемых в приложении, от хищения посредством JavaScript https://msdn.microsoft.com/en-us/library/ms228262(v=vs.100).aspx
- проверьте, устойчив ли POST метод Login контроллера Account к Open Redirect аттакам
- в корневом каталоге приложения создайте страницу Error.html и обеспечьте перенаправление на эту страницу в случае возникновения ошибки, запустите приложение и сгенерируйте ошибку посредством обращения к несуществующему контроллеру
              
Контрольные вопросы к Главе 7:
- Что такое аутентификация? Что такое авторизация?
> Authentication is verifying that users are who they say they are, using
some form of login mechanism (username/password, OpenID, OAuth and so on—
something that says “this is who I am”).

> Authorization is verifying that they can do
what they want to do with respect to your site. This is usually achieved using some
type of role-based or claim-based system.

- Как предотвратить анонимное обращение к действию контроллера? ко всем действиям контроллера? ко всему ASP.NET MVC приложению? как разрешить анонимный доступ к определенному действию или контроллеру?
> You need to know who the users are when they buy the album. You can resolve
this by adding the AuthorizeAttribute to the Buy action, like this:
```c#
[Authorize]
public ActionResult Buy(int id)
{
    var album = GetAlbums().Single(a => a.AlbumId == id);
    //Charge the user and ship the album!!!
    return View(album);
}
```
> 

> 

> 

- Как аттрибут Authorize работает?
> The AuthorizeAttribute is an authorization fi lter, which means that it executes before the associated
controller action. The AuthorizeAttribute performs its main work in the OnAuthorization
method, which is a standard method defi ned in the IAuthorizationFilter interface.

- Как работает аутентификация Windows и как ее сконфигурировать для приложения (IIS 7/8, IIS Express)?
> When you select the Windows Authentication option, authentication is effectively handled outside
of the application by the web browser, Windows, and IIS. For this reason, Startup.Auth.cs is not
included in the project, and no authentication middleware is confi gured.

> Because Registration and Log On with Windows Authentication are handled outside of the web
application, this template also doesn’t require the AccountController or the associated models
and views. To confi gure Windows Authentication, this template includes the following line in web
.config:
```<authentication mode="Windows" />```

> To use the Intranet authentication option, you’ll need to enable Windows authentication and disable Anonymous authentication.

> **IIS 7 and IIS 8**

> Complete the following steps to confi gure Intranet authentication when running under IIS 7 and
> IIS 8:
> 1. Open IIS Manager and navigate to your website.
> 2. In Features View, double-click Authentication.
> 3. On the Authentication page, select Windows authentication. If Windows authentication is
not an option, you’ll need to make sure Windows authentication is installed on the server.

> To enable Windows authentication in Windows:
> > a. In the Control Panel, open Programs and Features.

> > b. Select Turn Windows features on or off.

> > c. Navigate to Internet Information Services ➪ World Wide Web Services ➪ Security and
make sure the Windows authentication node is checked.

> To enable Windows authentication on Windows Server:
> > a. In the Server Manager, select Web Server (IIS) and click Add Role Services.

> > b. Navigate to Web Server ➪ Security and make sure the Windows authentication node is
checked.

> 4. In the Actions pane, click Enable to use Windows authentication.
> 5. On the Authentication page, select Anonymous authentication.
> 6. In the Actions pane, click Disable to disable anonymous authentication.

> **IIS Express**
> Complete the following steps to confi gure Intranet authentication when running under IIS 7
and IIS 8:
> 1. Click your project in the Solution Explorer to select the project.
> 2. If the Properties pane is not open, open it (F4).
> 3. In the Properties pane for your project:
> > a. Set Anonymous Authentication to Disabled.

> > b. Set Windows Authentication to Enabled.

- Как ограничить действия контроллера для определенных ролей и/или пользователей?
- Users vs Roles. Roles vs Claims.
- Как добавить дополнительные поля в профайл пользователя?
- Какие классы используются для управления ролями и пользователями в ASP.NET Identity?
- Стороння аутентификация и ее преимущества.
- Registering External Login Providers. Configuring OpenID Providers. Configuring OAuth Providers.
- Соображения безопасности при использовании сторонней аутентификации.
- SSL/HTTPS при аутентификации.
- Виды угроз безопасности веб приложений и способы их предотвращения, в частности в контексте ASP.NET MVC.
- Proper Error Reporting and the Stack Trace. Using Configuration Transforms. Using Retail Deployment Configuration in Production. Using a dedicated Error Loging System.
