# Изучение Главы 10 книги "Professional ASP.NET MVC 5" (создание и публикацие пакетов - прочитать для общего понимания).

Практическое задание к Главе 10: в консоли NuGet продемонстрируйте использованией справочной информации по командам, установку Route Debugger, получение информации об установленном пакете, удаление пакета (для справки используйте следующий ресурс https://docs.microsoft.com/en-us/nuget/tools/package-manager-console)

Контрольные вопросы к Главе 10:
- **Назначение NuGet.**
> NuGet is a package-management system for .NET and Visual Studio that makes it easy to
add, update, and remove external libraries and their dependencies in your application. NuGet
also makes creating packages that you can share with the world easy. 

- **Поиск, установка и обновление пакетов.**
> *Finding*
>  you can use the paging links at the bottom of the Manage NuGet
Packages dialog to page through the list of packages until you fi nd the one you want, but the quickest
way is to use the search bar in the top right.
> When you fi nd a package, the pane on the right displays information about the package. 

> *Installing*
> To install a package, perform the following steps:
> 1. Type ELMAH in the search box to fi nd it. In this case, there are several ELMAH related
packages, but the top result is the main ELMAH package, as indicated by both the
Description and Download count.
> 2. After you’ve found the package, click the Install button to install it. This downloads the
package, as well as all the packages it depends on, before it installs the package to your
project.

> In some cases, you’re prompted to accept the license terms for the package,
as well as any dependencies that also require license acceptance. Figure 10-4
shows what happens when you install the Microsoft.AspNet.SignalR package.
Requiring license acceptance is a setting in the package set by the package
author. If you decline the license terms, the packages are not installed.

> *Updating*
> NuGet doesn’t just help you install packages, it also helps you maintain them after installation. For
example, let’s assume you’ve installed 10 or so packages in your project. At some point, you’re going
to want to update some (or all) of your packages to the latest version of each. Before NuGet, this
was a time-consuming process of searching for and visiting the homepage of each library and checking
the latest version against the one you have.
With NuGet, updating is as easy as clicking the Updates node in the left pane. This displays a list of
packages in the current project that have newer versions available. Click the Update button next to
each package to upgrade the package to the latest version. This also updates all the dependencies of
the packages, ensuring that only compatible versions of the dependencies are installed.

- **Где хранится информация об установленных пакетах и какие детали она содержит. Где хранятся ссылки на установленные пакеты. Что представляет собой установленный пакет на диске и где он расположен?**
> When NuGet installs the ELMAH package, it makes a few changes to your project. The fi rst time
any package is installed into a project, a new fi le named packages.config is added to the project.
```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
 <package id="elmah" version="1.2.2" targetFramework="net451" />
 <package id="elmah.corelibrary" version="1.2.2" targetFramework="net451" />
</packages>
```
> Also notice that you now have an assembly reference to the Elmah.dll assembly.

> Where is that assembly referenced from? To answer that, you need to look at what fi les are added to
your solution when a package is installed. When the fi rst package is installed into a project, a packages
folder is created in the same directory as the solution file.
> The packages folder contains a subfolder for each installed package.
> To answer that, you need to look at what fi les are added to
your solution when a package is installed. When the fi rst package is installed into a project, a packages
folder is created in the same directory as the solution file.
> The packages folder contains a subfolder for each installed package. 
> Note that the name of each package folder includes a version number
because this folder stores all the packages installed for a given solution. The
possibility exists for two projects in the same solution to each have a different
version of the same package installed.


- **Пакеты и система контроля версий.**
>  the one possible NuGet workfl ow assumes developers will commit the
Packages folder into version control. One benefi t of this approach is that retrieving the solution from
version control ensures that everything needed to build the solution is available. The packages do
not need to be retrieved from another location.

> NuGet Package Restore is now enabled by default, but users can opt out
using two options in the Package Manager settings in Visual Studio:

> ➤ Allow NuGet to download missing packages

> ➤ Automatically check for missing packages during build in Visual
Studio



- **Консоль менеджера пакетов. В чем его преимущества по сравнению с оконным режимом.**
> This is a PowerShell-based console within Visual Studio
that provides a powerful way to fi nd and install packages and supports a few additional scenarios
that the Manage NuGet Packages dialog doesn’t.

> One nice thing about PowerShell commands is that they support tab expansions, which
means you can type the fi rst few letters of a command and press the Tab key to see a range
of options.

> PowerShell also enables composing commands together by piping one
command into another. For example, if you want to install a package into every project in
your solution, you can run the following command:
'Get-Project -All | Install-Package log4net'
> The first command retrieves every project in your solution and pipes the output to the
second command, which installs the specifi ed package into each project.

> Generally the decision whether to use the Manage NuGet Packages dialog
versus the Package Manager Console comes down to a matter of preference:
Do you like clicking or typing? However, the Package Manager Console supports
a few scenarios that are not available via the dialog:
1. Install a specifi c version using the -Version fl ag (for example,
Install-Package EntityFramework -Version 4.3.1).
2. Reinstall a package that has already been installed using the
-Reinstall fl ag (for example, Install-Package JQueryUI
-Reinstall). This is useful if you’ve removed fi les that were
installed by the package or in certain build scenarios.
3. Ignore dependencies using the -ignoreDependencies fl ag (InstallPackage
jQuery.Validation –ignoreDependencies). This is
often useful if you’ve already installed dependencies outside of NuGet.
4. Force the uninstallation of a package despite dependencies
(Uninstall-Package jQuery -force). Again, this is helpful if
you want to manage a dependency outside of NuGet.

- **Создание и публикация пакета - что из себя представляет.**
>
