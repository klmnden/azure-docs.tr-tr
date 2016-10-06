<properties
   pageTitle="Azure SQL Data Warehouse'a Bağlanma | Microsoft Azure"
   description="Azure SQL Veri Ambarı için sunucu adı ve bağlantı dizesini bulma"
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
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>


# Azure SQL Data Warehouse’a bağlanma

Bu makale SQL Veri Ambarı’na ilk kez bağlanmanıza yardımcı olur.

## Sunucu adınızı bulma

SQL Veri Ambarına bağlanmanın ilk adımı, sunucunuzun adını bulmayı bilmektir.  Örneğin, aşağıdaki örnekte sunucu adı sample.database.windows.net şeklindedir. Tam sunucu adını bulmak için:

1. [Azure Portal][] gidin.
2. **SQL veritabanları**’na tıklayın 
3. Bağlanmak istediğiniz veritabanına tıklayın.
4. Tam sunucu adını bulun.

    ![Tam sunucu adı][1]

## Desteklenen sürücüler ve bağlantı dizeleri

Azure SQL Veri Ambarı [ADO.NET][], [ODBC][], [PHP][] ve [JDBC][]’yi destekler. En son sürümü ve belgeleri bulmak için yukarıdaki sürücülerden birine tıklayın. Azure portalından kullandığınız sürücünün bağlantı dizesini otomatik olarak oluşturmak için önceki örnekte bulunan **Veritabanı bağlantı dizelerini göster**’e tıklayabilirsiniz.  Aşağıda ayrıca her sürücü için bir bağlantı dizesinin nasıl göründüğü ile ilgili bazı örnekler verilmiştir.

> [AZURE.NOTE] Bağlantınızın kısa süreli kesintiler sırasında devam etmesi için bağlantı zaman aşımını 300 saniyeye ayarlayın.

### ADO.NET bağlantı dizesi örneği

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### ODBC bağlantı dizesi örneği

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### PHP bağlantı dizesi örneği

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### JDBC bağlantı dizesi örneği

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## Bağlantı ayarları

SQL Veri Ambarı, bağlantı ve nesne oluşturma sırasında bazı ayarları standart hale getirir. Bu ayarlar geçersiz kılınamaz ve şunları içerir:

| Veritabanı Ayarı       | Değer                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | AÇIK                           |
| [QUOTED_IDENTIFIERS][] | AÇIK                           |
| [DATEFORMAT][]         | mdy                          |
| [DATEFIRST][]          | 7                            |

## Sonraki adımlar

Visual Studio ile bağlantı kurmak ve sorgulamak için bkz. [Visual Studio ile Sorgulama][]. Kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz. [Azure SQL Veri Ambarı’nda kimlik doğrulama][].

<!--Articles-->
[Visual Studio ile Sorgulama]: ./sql-data-warehouse-query-visual-studio.md
[Azure SQL Veri Ambarı’nda kimlik doğrulama]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure Portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=Sep16_HO4-->


