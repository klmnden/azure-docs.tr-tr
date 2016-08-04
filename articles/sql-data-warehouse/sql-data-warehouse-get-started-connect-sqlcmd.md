<properties
   pageTitle="Başlarken: Azure SQL Data Warehouse'a Bağlanma | Microsoft Azure"
   description="SQL Data Warehouse'a bağlanıp birkaç sorgu çalıştırarak başlayın."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/16/2016"
   ms.author="mausher;barbkess;sonyama"/>

# SQLCMD ile bağlanma ve sorgulama

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)
- [AAD](sql-data-warehouse-get-started-connect-aad-authentication.md)


Adım adım ilerleyeceğimiz bu bölümde sqlcmd.exe yardımcı programını kullanarak yalnızca birkaç dakika içinde bir Azure SQL Data Warehouse veritabanına nasıl bağlanacağınız ve veritabanını nasıl sorgulayacağınız gösterilmiştir. Bu bölümde şunları yapacaksınız:

+ Önkoşul yazılımını yükleme
+ AdventureWorksDW örnek veritabanını içeren bir veritabanına bağlanma
+ Örnek veritabanında bir sorgu yürütme  

## Önkoşullar

+ [sqlcmd.exe][]'yi indirmek için lütfen bkz. [SQL Server için Microsoft Komut Satırı Yardımcı Programları 11][].

## Tam Azure SQL sunucu adınızı alma

Veritabanınıza bağlanmak için, bağlanmak istediğiniz veritabanının bulunduğu sunucunun tam adı (***sunucuadı**.database.windows.net*) gerekir.

1. [Azure portalına][] gidin.
2. Bağlanmak istediğiniz veritabanına gözatın.
3. Tam sunucu adını bulun (Bunu aşağıdaki adımlarda kullanacağız):

![][1]


## sqlcmd ile SQL Data Warehouse'a bağlanma

sqlcmd kullanarak SQL Data Warehouse'un belirli bir örneğine bağlanmak için komut istemini açıp **sqlcmd** komutunu ve ardından SQL Data Warehouse veritabanınızın bağlantı dizesini girin. Bağlantı dizesi için şu parametreler gereklidir:

+ **Server (-S):** `<`Sunucu Adı`>`.database.windows.net biçiminde belirtilmiş sunucu
+ **Database (-d):** Veritabanı adı.
+ **User (-U):** `<`Kullanıcı biçimindeki sunucu kullanıcısı`>`
+ **Password (-P):** Kullanıcıyla ilişkili parola.
+ **Enable Quoted Identifiers (-I):** Bir SQL Data Warehouse örneğine bağlanmak için tırnak işaretli tanımlayıcıların etkinleştirilmesi gerekir.

Bu nedenle bir SQL Data Warehouse örneğine bağlanmak için şunları girmeniz gerekir:

```sql
C:\>sqlcmd -S <Server Name>.database.windows.net -d <Database> -U <User> -P <Password> -I
```

## Örnek sorgu çalıştırma

Bağlantının ardından desteklenen herhangi bir Transact-SQL deyimini örnekte yayımlayabilirsiniz.

```sql
C:\>sqlcmd -S <Server Name>.database.windows.net -d <Database> -U <User> -P <Password> -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

sqlcmd hakkında ek bilgi için bkz. [sqlcmd belgeleri][sqlcmd.exe].


## Sonraki adımlar

Artık bağlanıp sorgulama yapabildiğinize göre [PowerBI ile bağlantı kurmayı][] deneyin.

Ortamınızı Windows kimlik doğrulaması için yapılandırmak üzere bkz. [Azure Active Directory Kimlik Doğrulamasını Kullanarak SQL Database'e veya SQL Data Warehouse'a Bağlanma][].

<!--Articles-->
[Azure Active Directory Kimlik Doğrulamasını Kullanarak SQL Database'e veya SQL Data Warehouse'a Bağlanma]: ../sql-data-warehouse/sql-data-warehouse-get-started-connect-aad-authentication.md
[PowerBI ile bağlantı kurmayı]: ./sql-data-warehouse-integrate-power-bi.md
[Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[SQLCMD]: ./sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other-->
[sqlcmd.exe]: https://msdn.microsoft.com/en-us/library/ms162773.aspx
[SQL Server için Microsoft Komut Satırı Yardımcı Programları 11]: http://go.microsoft.com/fwlink/?LinkId=321501
[Azure portalına]: https://portal.azure.com

<!--Image references-->
[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png



<!----HONumber=Jun16_HO2-->


