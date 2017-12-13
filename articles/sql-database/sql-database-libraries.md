---
title: "SQL veritabanı için bağlantı kitaplıkları | Microsoft Docs"
description: "SQL Server ve SQL veritabanı geniş çeşitli programlama dillerinde istemci bağlantıyı etkinleştir modüllerin yüklemeler için bağlantılar sağlar."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: genemi
ms.openlocfilehash: 012acd2b53fc9205511530d3cc30803dceef88a0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="connectivity-libraries-and-frameworks-for-sql-server"></a>Bağlantı kitaplıklarını ve çerçevelerini SQL Server için

Kullanıma bizim [alma öğreticileri başlatılan](http://aka.ms/sqldev) ile programlama dilleri C#, Java, Node.js, PHP ve Python gibi hızlı bir şekilde başlamak için. Ardından Linux veya Windows veya Docker macOS üzerinde SQL Server kullanarak bir uygulama oluşturun.

Aşağıdaki tabloda bağlantı kitaplıkları listeler veya *sürücüleri* istemci uygulamaları bağlanmak ve şirket içi çalışan SQL Server kullanmak için dilleri veya bulutta değişik kullanabilirsiniz. Linux, Windows veya Docker kullanın ve bunları Azure SQL Database ve Azure SQL Data Warehouse bağlanmak için kullanın. 

| Dil | Platform | Ek kaynaklar | İndir | başlarken |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [SQL Server için Microsoft ADO.NET](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [İndir](https://www.microsoft.com/net/download/) | [Kullanmaya başlama](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [SQL Server için Microsoft JDBC sürücüsü](http://msdn.microsoft.com/library/mt484311.aspx) | [İndir](https://go.microsoft.com/fwlink/?linkid=852460) |  [Kullanmaya başlama](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [SQL Server için PHP SQL sürücüsü](http://msdn.microsoft.com/library/dn865013.aspx) | İşletim Sistemi: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Kullanmaya başlama](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [SQL Server için node.js sürücüsü](http://msdn.microsoft.com/library/mt652093.aspx) | [Yükleme](https://msdn.microsoft.com/library/mt652094.aspx) |  [Kullanmaya başlama](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Python SQL sürücüsü](http://msdn.microsoft.com/library/mt652092.aspx) | Seçimler yükleyin: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Kullanmaya başlama](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [SQL Server için Söyleniş sürücüsü](http://msdn.microsoft.com/library/mt691981.aspx) | [Yükleme](https://msdn.microsoft.com/library/mt711041.aspx) | [Kullanmaya başlama](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [SQL Server için Microsoft ODBC sürücüsü](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [İndir](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

Nesne İlişkisel eşleme (ORM) çerçeveler ve istemci uygulamaları veya şirket içi çalışan SQL Server ile kullanabileceğiniz web çerçeveleri bulutta örnekleri aşağıdaki tabloda listelenmektedir. Linux, Windows veya Docker çerçeveleri kullanın ve bunları SQL Database ve SQL Data Warehouse bağlanmak için kullanın. 

| Dil | Platform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Entity Framework Çekirdek](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[ORM hazırda bekleme](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [ORM sequelize](http://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby rayları üzerinde](http://rubyonrails.org/) |
||||

## <a name="related-links"></a>İlgili bağlantılar
- [SQL Server sürücüleri](http://msdn.microsoft.com/library/mt654049.aspx) istemci uygulamalarından bağlanmak için kullanılan
- SQL veritabanına bağlan:
    - [.NET (C#) kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-dotnet.md)
    - [PHP kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-php.md)
    - [Node.js kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-nodejs.md)
    - [Java kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-java.md)
    - [Python kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-python.md)
    - [Ruby kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-ruby.md)
- Yeniden deneme mantığı kod örnekleri:
    - [SQL ADO.NET ile resiliently bağlanma][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
    - [PHP ile SQL resiliently bağlanma][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

