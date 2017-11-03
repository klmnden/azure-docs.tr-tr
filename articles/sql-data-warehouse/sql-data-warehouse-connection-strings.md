---
title: "SQL veri ambarı için sürücüleri | Microsoft Docs"
description: "Bağlantı dizeleri ve SQL Data Warehouse için sürücüleri"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 5c91f423-b550-4734-8094-c7f2c418ac8d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: e71ea1d23f68ed41c03bbce88b08863d2831c1bd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="drivers-for-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse için sürücüler
Gibi birçok farklı uygulama protokollerle SQL veri ambarına bağlanabilir [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP] [ PHP] ve [JDBC][JDBC]. Bağlantı dizeleri her protokol için bazı örnekleri aşağıda verilmiştir.  Azure portalı, bağlantı dizesi oluşturmak için de kullanabilirsiniz.  Azure Portalı'nı kullanarak bağlantı dizenizi oluşturmak için veritabanı dikey penceresine, altında gidin *Essentials* tıklayın *veritabanı bağlantı dizelerini Göster*.

## <a name="sample-adonet-connection-string"></a>Örnek ADO.NET bağlantı dizesi
```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## <a name="sample-odbc-connection-string"></a>Örnek ODBC bağlantı dizesi
```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## <a name="sample-php-connection-string"></a>Örnek PHP bağlantı dizesi
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## <a name="sample-jdbc-connection-string"></a>Örnek JDBC bağlantı dizesi
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [!NOTE]
> Kullanılamazlık kısa süreyle varlığını sürdürmesi bağlantıya izin vermek için 300 saniye olarak bağlantı zaman aşımı ayarlamayı göz önünde bulundurun.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio ve diğer uygulamalarla veri ambarınız sorgulama başlatmak için bkz: [sorgu Visual Studio ile][Query with Visual Studio].

<!--Image references-->

<!--Azure.com references-->
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->
