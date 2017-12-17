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
- **Что такое аутентификация? Что такое авторизация?**
> Authentication is verifying that users are who they say they are, using
some form of login mechanism (username/password, OpenID, OAuth and so on—
something that says “this is who I am”).

> Authorization is verifying that they can do
what they want to do with respect to your site. This is usually achieved using some
type of role-based or claim-based system.

- **Как предотвратить анонимное обращение к действию контроллера? ко всем действиям контроллера? ко всему ASP.NET MVC приложению? как разрешить анонимный доступ к определенному действию или контроллеру?**
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
> You accomplish this by putting
the AuthorizeAttribute on the CheckoutController, like this:
```c#
[Authorize]
public class CheckoutController : Controller
```
> This says that all actions in the CheckoutController will allow any registered user, but will not
allow anonymous access.

> To register the AuthorizeAttribute as a global fi lter, add it to the global fi lters collection in the
RegisterGlobalFilters method, located in \App_Start\FilterConfig.cs:
```c#
public static void RegisterGlobalFilters(GlobalFilterCollection filters) {
filters.Add(new System.Web.Mvc.AuthorizeAttribute());
filters.Add(new HandleErrorAttribute());
}
```
> This applies the AuthorizeAttribute to all controller actions in the application.

> The obvious problem with a global authentication is that it restricts access to the entire site, including
the AccountController, which would result in the users’ having to log in before being able
to register for the site, except they don’t yet have an account—how absurd! Prior to MVC 4, if
you wanted to use a global fi lter to require authorization, you had to do something special to
allow anonymous access to the AccountController. A common technique was to subclass the
AuthorizeAttribute and include some extra logic to selectively allow access to specifi c actions.
MVC 4 added a new AllowAnonymous attribute. You can place AllowAnonymous on any methods
(or entire controllers) to opt out of authorization as desired.
For an example, you can see the default AccountController in a new MVC 5 application using
Individual Accounts for authentication. All methods that would require external access if the
AuthorizeAttribute were registered as a global fi lter are decorated with the AllowAnonymous
attribute. For example, the Login HTTP Get action appears, as follows:
```c#
// GET: /Account/Login
[AllowAnonymous]
public ActionResult Login(string returnUrl)
{
ViewBag.ReturnUrl = returnUrl;
return View();
}
```
> This way, even if you register the AuthorizeAttribute as a global fi lter, users can access the login
actions.

> Although AllowAnonymous solves this specifi c problem, it only works with the standard
AuthorizeAttribute; it won’t necessarily work with custom authorization fi lters. If you’re using
custom authorization fi lters, you’ll want to use a new feature in MVC 5: override fi lters. These allow
you to locally override any fi lter (for example, any custom authorization fi lter that derives from
IAuthorizationFilters).

- **Как аттрибут Authorize работает?**
> The AuthorizeAttribute is an authorization fi lter, which means that it executes before the associated
controller action. The AuthorizeAttribute performs its main work in the OnAuthorization
method, which is a standard method defi ned in the IAuthorizationFilter interface.

- **Как работает аутентификация Windows и как ее сконфигурировать для приложения (IIS 7/8, IIS Express)?**
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

- **Как ограничить действия контроллера для определенных ролей и/или пользователей?**
> So far you’ve looked at the use of AuthorizeAttribute to prevent anonymous access to a controller
or controller action. However, as mentioned, you can also limit access to specifi c users or roles.
A common example of where this technique is used is in administrative functions.

> Fortunately, AuthorizeAttribute allows you to specify both roles and users, as
shown here:
```c#
[Authorize(Roles="Administrator")]
public class StoreManagerController : Controller
```
This restricts access to the StoreManagerController to users who belong to the Administrator
role. Anonymous users, or registered users who are not members of the Administrator role, are prevented
from accessing any of the actions in the StoreManagerController.
As implied by the name, the Roles parameter can take more than one role. You can pass in a
comma-delimited list:
```c#
[Authorize(Roles="Administrator,SuperAdmin")]
public class TopSecretController:Controller
```
You can also authorize by a list of users:
```c#
[Authorize(Users="Jon,Phil,Scott,Brad,David")]
public class TopSecretController:Controller
```
And you can combine them, as well:
```c#
[Authorize(Roles="UsersNamedScott", Users="Jon,Phil,Brad,David")]
public class TopSecretController:Controller
```
- **Users vs Roles. Roles vs Claims.**
> Managing your permissions based on roles instead of users is generally considered
a better idea, for several reasons:

> ➤ Users can come and go, and a specifi c user is likely to require (or lose) permissions
over time.

> ➤ Managing role membership is generally easier than managing user membership.
If you hire a new offi ce administrator, you can easily add her to an
Administrator role without a code change. If adding a new administrative
user to your system requires you to modify all your Authorize attributes and
deploy a new version of the application assembly, people will laugh at you.

> ➤ Role-based management enables you to have different access lists across
deployment environments. You might want to grant developers Administrator
access to a payroll application in your development and stage environments,
but not in production.

> When you’re creating role groups, consider using privileged-based role groups.
For example, roles named CanAdjustCompensation and CanEditAlbums are
more granular and ultimately more manageable than overly generic groups like
Administrator followed by the inevitable SuperAdmin and the equally inevitable
SuperSuperAdmin.

> When you head down this direction, you’re bordering on claims-based authorization.
Under the hood, ASP.NET has supported claims-based authorization since
.NET 4.5, although it’s not surfaced by AuthorizeAttribute. Here’s the easy
way to understand the difference between roles and claims: Role membership
is a simple Boolean—a user either is a member of the role or not. A claim can
contain a value, not just a simple Boolean. This means that users’ claims might
include their username, their corporate division, the groups or level of other
users they are allowed to administer, and so on. So with claims, you wouldn’t
need a bunch of roles to manage the extent of compensation adjustment powers
(CanAdjustCompensationForEmployees, CanAdjustCompensationForManagers,
and so on). A single claim token can hold rich information about exactly which
employees you rule.

> This means that roles are really a specifi c case of claims, because membership in a
role is just one simple claim.

- **Как добавить дополнительные поля в профайл пользователя?**
> One of the design requirements for ASP.NET Identity is to allow for extensive customization without
undue pain. Some of the extensibility points include:

> ➤ Adding additional user profi le data is now trivial.

> ➤ Persistance control is supported through the use of UserStore and RoleStore abstractions over
the data access layer.

> ➤ The RoleManager makes it easy to create roles and manage role membership.

> **Storing additional user profi le data**
> It’s a very common requirement to store additional information about your users: birthday, Twitter
handle, site preferences, etc. In the past, adding additional profi le data was unnecessarily diffi cult.
> In ASP.NET Identity, users are modeled using an Entity Framework Code First model, so adding
additional information to your users is as simple as adding properties to your ApplicationUser
class (found in /Models/IdentityModels.cs). For example, to add an Address and Twitter handle
to your user, youd just add the following properties:
```c#
public class ApplicationUser : IdentityUser
{
public string Address { get; set; }
public string TwitterHandle { get; set; }
}
```
- **Какие классы используются для управления ролями и пользователями в ASP.NET Identity?**
> ASP.NET Identity includes a UserManager and RoleManager which make it easy to perform common
tasks like creating users and roles, adding users to roles, checking if a user is in a role, etc.

> It’s great that you’ve got these extensibility points when you need them. For the most part, if you’re
using the standard AccountController and storing user information via Entity Framework, you
just code away without considering the extensibility points—until you want them.

- **Стороння аутентификация и ее преимущества.**
> Although traditional membership is a great fi t in a lot of web applications, it comes with some serious
downsides:

> ➤ Maintaining a local database of usernames and secret passwords is a large security liability.
Large security breaches involving hundreds of thousands of users’ account information (often
including unencrypted passwords) have become common. Worse, because many users reuse
passwords on multiple websites, compromised accounts may affect your users’ security on
their banking or other sensitive websites.

> ➤ Website registration is annoying. Users have gotten tired of fi lling out forms, complying with
widely differing password policies, remembering passwords, and worrying whether your
site is going to keep their information secure. A signifi cant percentage of potential users will
decide they would rather not bother with registering for your site.
OAuth and OpenID are open standards for authentication. These protocols allow your users to log
in to your site using their existing accounts on other trusted sites (called providers), such as Google,
Twitter, Microsoft, and others.

- **Registering External Login Providers. Configuring OpenID Providers. Configuring OAuth Providers.**
> You need to explicitly enable external sites for login. Fortunately, this task is extremely simple to do.
Authorization providers are confi gured in App_Start\Startup.Auth.cs. When you create a new
External Login via OAuth and OpenID ❘ 177
application, all authentication providers in Startup.Auth.cs are commented out and will appear as
follows:
```c#
public partial class Startup
{
    // For more information on configuring authentication,
    // please visit http://go.microsoft.com/fwlink/?LinkId=301864
    public void ConfigureAuth(IAppBuilder app)
    {
        // Enable the application to use a cookie to store
        // information for the signed in user
        app.UseCookieAuthentication(new CookieAuthenticationOptions
        {
            AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie, 
            LoginPath = new PathString("/Account/Login")
        });
        // Use a cookie to temporarily store information about
        // a user logging in with a third party login provider
        app.UseExternalSignInCookie(DefaultAuthenticationTypes.ExternalCookie);
        // Uncomment the following lines to enable logging in
        // with third party login providers
        //app.UseMicrosoftAccountAuthentication(
        // clientId: "",
        // clientSecret: "");
        //app.UseTwitterAuthentication(
        // consumerKey: "",
        // consumerSecret: "");
        //app.UseFacebookAuthentication(
        // appId: "",
        // appSecret: "");
        //app.UseGoogleAuthentication();
    }
}
```
> Sites that use an OAuth provider (Facebook, Twitter, and Microsoft) require you to register your site
as an application. When you do, you’ll be provided a client id and a secret. Your site uses these to
authenticate with the OAuth provider. Sites that implement OpenID (such as Google and Yahoo!) do
not require you to register an application, and you won’t need a client id or secret.

- **Соображения безопасности при использовании сторонней аутентификации.**
> Although OAuth and OpenID simplify your site’s security code, they introduce other potential
attack vectors into your application. If either a provider site or the security communication between
your sites is compromised, an attacker could either subvert the login to your site or capture the
user’s information. Continuing to pay attention to security when you’re using delegated authentication
is important. Security for your site is always your responsibility, even if you’re making use of
external services for authentication.

> Trusted External Login Providers
Supporting only providers whose security you trust, which generally means sticking with wellknown
providers, is important for a couple reasons:

> ➤ When you are redirecting your users to external sites, you want to make sure that these sites
are not malicious or poorly secured ones that will leak or misuse your users’ login data or
other information.

> ➤ Authentication providers are giving you information about users—not just their registration
state, but also e-mail addresses and potentially other provider-specifi c information. Although
by default this additional information isn’t stored, reading provider data such as e-mail to
prevent the user from having to re-enter it is not uncommon. A provider could potentially—
either accidentally or maliciously—return information. Displaying any provider information
to the user before you store it is generally a good idea.

- **SSL/HTTPS при аутентификации.**
> The callback from an external provider to your site contains security tokens that allow access to
your site and contain user information. Transmitting this information over HTTPS to prevent interception
while this information travels over the Internet is important.
To enforce HTTPS for this callback, applications that support external logins should require
HTTPS for access to the AccountController’s Login GET method using the RequireHttps
attribute:
```c#
// GET: /Account/Login
[RequireHttps]
[AllowAnonymous]
public ActionResult Login(string returnUrl)
{
    ViewBag.ReturnUrl = returnUrl;
    return View();
}
```
> Enforcing HTTPS during login to your site causes all calls to external providers to occur over
HTTPS, which, in turn, causes the providers to make their callbacks to your site using HTTPS.
Additionally, using HTTPS with Google authentication is important. Google reports a user who
logs in once via HTTP and later via HTTPS as two different people. Always requiring HTTPS
prevents this problem.

- **Виды угроз безопасности веб приложений и способы их предотвращения, в частности в контексте ASP.NET MVC.**
> 
- **Proper Error Reporting and the Stack Trace. Using Configuration Transforms. Using Retail Deployment Configuration in Production. Using a dedicated Error Loging System.**
> 207-209
