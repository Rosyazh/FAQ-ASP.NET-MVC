# H5 Create local DB:
> - create DB via PSQL:
> ```sql
>	 create database ECL_base;
> ```
> - create a role:
> ```sql
>	 create role ECL_adm with login password 'ECL_adm96541';
> ```
> - grant privileges:
> ```sql
>	 grant all privileges on database ECL_base to ECL_adm;
> ```


**Fill local DB:**
> - restore the database from the backup by writing to the command line:

>> 1. first file:
> ```
>	 psql -U ecl_adm -d ecl_base -f ecl_base_b.sql
> ```
> you can explicitly specify the path to the file, for example:
> ```
>	 psql -U ecl_adm -d ecl_base -f C:\Users\yrassol\source\repos\ECLExtension\main\EmpInfoMain\EmpInfoMain\Utils\LocalDB\ecl_base_b.sql
> ```

> 2. second file:
> ```
>	 psql -U ecl_adm -d ecl_base -f ecl_base_b_2.sql
> ```
> you can explicitly specify the path to the file, for example:
> ```
>	 psql -U ecl_adm -d ecl_base -f C:\Users\yrassol\source\repos\ECLExtension\main\EmpInfoMain\EmpInfoMain\Utils\LocalDB\ecl_base_b_2.sql
> ```


**Create DB for ELMAH**
> - create DB via PSQL:
> ```sql
>	 create database elmah owner postgres;
> ```


Выполнение практического задания.

Практическое задание: 
- Создайте приложение с Windows аутентификацией и с простейшим Web API, возвращающим простые статические данные, например список из 5 стран и из 5 городов (возвращающим полный список и страну/город по id), данные не должны храниться в базе данных!, а должны просто генерироваться кодом.
- Установите на вашей машине СУБД PostgreSQL, скачайте установщик для вашей ОС:
https://www.openscg.com/bigsql/postgresql/installers.jsp/
при возникновении трудностей смотрите инструкцию по установки для вашей ОС:
https://wiki.postgresql.org/wiki/Detailed_installation_guides
- Создайте БД:
команда создания базы geo (командная строка PSQL, находится в папке установки в меню Пуск): CREATE DATABASE geo;
строка подключения в клиенте, например DBVisualizer: jdbc:postgresql://localhost:5432/geo
пользователь по умолчанию и пароль - тот пароль, который задали при установке: postgres/********
для подключения из клиента необходимо указать путь к jdbc драйверу, название драйвера может быть: postgresql.jar
пример команды в клиенте для создания таблицы:
CREATE TABLE country (
id integer PRIMARY KEY,
name varchar(40),
capital_city varchar(40)
);
пример команды вставки данных в таблицу:
insert into country (id, name, capital_city) values (1, 'England', 'London');
просмотр данных в публичной схеме:
select * from public.country
- Создайте подключение к БД в приложении на примере инструкции:
https://metanit.com/sharp/articles/mvc/18.php если на этапе создания контроллера или при генерации Scaffolding Template получите ошибку "exception has been thrown by the target of an invocation", перезапустите VisualStudio
для класса модели определите аттрибут с названием таблицы и схемы, например: [Table("country", Schema = "public")]
если названия полей таблицы и свойств модели отличаются, укажите точное название полей, включая регистр, например:
[Table("country", Schema = "public")]
    public class Country
    {
        [Column("id")]
        public int Id { get; set; }
        [Column("name")]
        public string Name { get; set; }
        [Column("capital_city")]
        public string Capital_city { get; set; }
    }
- Создайте функционал, который по расписанию, например каждые 5 минут обращается к API и обновляет данные в созданной ранее БД и обновлять поле, например LastUpdateDateTime, чтобы было видно когда определенная запись в БД была обновлена. Использовать можно Windows Service в виде простого консольного приложения. https://msdn.microsoft.com/ru-ru/library/zt39148a(v=vs.110).aspx
Для обращения к API из Windows сервиса можно использовать HttpClient:
GET https://stackoverflow.com/questions/12942644/how-to-call-a-webapi-from-windows-service
POST https://stackoverflow.com/questions/15176538/net-httpclient-how-to-post-string-value
- Добавьте логирование ошибок в БД PostgreSQL, установив соответствующий пакет elmah.postgresql. 
Инструкция https://dillieodigital.wordpress.com/2011/03/30/elmah-a-quick-start-tutorial-and-guide/
https://docs.microsoft.com/en-us/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
создайте соответствующие объекты в БД, смотрите файл Elmah.PostgreSQL.txt в папке установки пакета Elmah
Официальный сайт https://code.google.com/archive/p/elmah/
При возникновении ошибки: "An ASP.NET setting has been detected that does not apply in Integrated managed pipeline mode.", смотрите ресурс:
https://stackoverflow.com/questions/4209999/an-asp-net-setting-has-been-detected-that-does-not-apply-in-integrated-managed-p
- Для Windows аутентификации добавьте роли: admin - для просмотра ошибок, country - для просмотра информации о странах, city - для просмотра информации о городах, информация о которых должна храниться в созданной ранее БД.
Информация о работе с ролями может быть получена в Главе 12 онлайн самоучителя https://metanit.com/sharp/mvc5/
Использование ролей с аутентификацией Windows: https://stackoverflow.com/questions/6043100/asp-net-mvc-and-windows-authentication-with-custom-roles
Обработка сценария, когда у пользователя нету требуемой роли (по умолчанию в Windows аутентификации появляется диалоговое окно логина): https://stackoverflow.com/questions/39797026/how-to-display-custom-error-if-authorization-fails-in-asp-net-mvc
Создайте простейший интерфейс (представление) для работы с ролями (grant, revoke). Протестируйте работу ролей, назначив и удалив их для своего id.
