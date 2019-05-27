---
title: Azure SQL veri ambarı için bağlantı dizelerini | Microsoft Docs
description: SQL veri ambarı için bağlantı dizeleri
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 8f7843714395664b98383c32911de40ca064779e
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873624"
---
# <a name="connection-strings-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için bağlantı dizeleri
Gibi birçok farklı uygulama protokolü ile SQL Data Warehouse'a bağlanma [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP] [ PHP] ve [JDBC][JDBC]. Bağlantı dizeleri her protokol için bazı örnekler aşağıda verilmiştir.  Azure portalı, bağlantı dizenizi oluşturmak için de kullanabilirsiniz.  Azure portalını kullanarak bağlantı dizenizi oluşturmak için veritabanı dikey penceresine, altında gidin *Essentials* tıklayarak *veritabanı bağlantı dizelerini Göster*.

## <a name="sample-adonet-connection-string"></a>Örnek ADO.NET bağlantı dizesi
```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## <a name="sample-odbc-connection-string"></a>ODBC bağlantı dizesi örneği
```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## <a name="sample-php-connection-string"></a>Örnek PHP bağlantı dizesi
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## <a name="sample-jdbc-connection-string"></a>JDBC bağlantı dizesi örneği
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [!NOTE]
> Kısa süreli kesintiler varlığını sürdürmesi bağlantıya izin vermek için bağlantı zaman aşımı 300 saniye olarak ayarlamayı düşünün.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio ve diğer uygulamalarla veri Ambarınızı sorgulamaya başlamak için bkz: [Visual Studio ile sorgulama][Query with Visual Studio].

<!--Image references-->

<!--Azure.com references-->
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->
