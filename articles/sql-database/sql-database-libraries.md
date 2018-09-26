---
title: SQL veritabanı için bağlantı kitaplıkları | Microsoft Docs
description: İndirmeler modüllerinin istemci programlama dillerinin geniş çeşitli SQL Server ve SQL veritabanı bağlantısını etkinleştirmek için bağlantılar sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 40de6a93516a556958c1fd0cd3f861304e55a600
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165526"
---
# <a name="connectivity-libraries-and-frameworks-for-sql-server"></a>Bağlantı kitaplıkları ve SQL Server için çerçeveleri

Kullanıma sunduğumuz [alma Eğitmenleri](http://aka.ms/sqldev) C#, Java, Node.js, PHP ve Python gibi programlama ile hızlı bir şekilde kullanmaya başlamak için. MacOS üzerinde SQL Server Linux veya Windows veya Docker kullanarak ardından bir uygulama oluşturun.

Aşağıdaki tabloda bağlantı kitaplıkları listeler veya *sürücüleri* istemci uygulamalarını değişik bağlanmak ve şirket içinde çalışan SQL Server kullanmak için dilleri içinde veya bulutta kullanabilirsiniz. Linux, Windows veya Docker kullanın ve Azure SQL veritabanı ve Azure SQL veri ambarı bağlanmak için bunları kullanın. 

| Dil | Platform | Ek kaynaklar | İndirme | başlarken |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [SQL Server için Microsoft ADO.NET](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [İndir](https://www.microsoft.com/net/download/) | [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [SQL Server için Microsoft JDBC sürücüsü](http://msdn.microsoft.com/library/mt484311.aspx) | [İndir](https://go.microsoft.com/fwlink/?linkid=852460) |  [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [SQL Server için PHP SQL sürücüsü](http://msdn.microsoft.com/library/dn865013.aspx) | İşletim Sistemi: <br/> \* [Windows](https://www.microsoft.com/download/details.aspx?id=55642) <br/> \* [Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \* [macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [SQL Server için node.js sürücüsü](http://msdn.microsoft.com/library/mt652093.aspx) | [Yükleme](https://msdn.microsoft.com/library/mt652094.aspx) |  [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [SQL Python sürücüsü](http://msdn.microsoft.com/library/mt652092.aspx) | Seçenekler'i yükleyin: <br/> \* [pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \* [pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [SQL Server için Ruby sürücüsü](http://msdn.microsoft.com/library/mt691981.aspx) | [Yükleme](https://msdn.microsoft.com/library/mt711041.aspx) | [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [SQL Server için Microsoft ODBC sürücüsü](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) | [İndir](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) |  

Aşağıdaki tabloda, bulutta nesne ilişkisel eşleme (ORM) çerçeveleri ve istemci uygulamaları veya şirket içinde çalışan SQL Server ile kullanabileceğiniz web çerçeveleri örneklerini listeler. Linux, Windows veya Docker çerçeveleri kullanın ve SQL veritabanı ve SQL veri ambarına bağlanmak için bunları kullanın. 

| Dil | Platform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Entity Framework Core](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[ORM hazırda bekleme](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [ORM sequelize](http://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](http://rubyonrails.org/) |
||||

## <a name="related-links"></a>İlgili bağlantılar
- [SQL Server sürücüleri](http://msdn.microsoft.com/library/mt654049.aspx) istemci uygulamalarından bağlanmak için kullanılabilir
- SQL veritabanı'na bağlanma:
    - [.NET (C#) kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-dotnet.md)
    - [PHP kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-php.md)
    - [Node.js kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-nodejs.md)
    - [Java kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-java.md)
    - [Python kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-python.md)
    - [Ruby kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-ruby.md)
- Yeniden deneme mantığı kod örnekleri:
    - [Dayanıklı ADO.NET ile SQL bağlantısı kurma][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
    - [Dayanıklı PHP ile SQL bağlantısı kurma][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

