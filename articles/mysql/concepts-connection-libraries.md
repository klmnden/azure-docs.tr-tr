---
title: MySQL için Azure veritabanı için bağlantı kitaplıkları
description: Bu makalede, MySQL için Azure veritabanına bağlanırken istemci programları kullanabileceğiniz her kitaplığı veya sürücü listelenmektedir.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 3f49065d4f66f55ed728626764d9cac2aa5c3c69
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42060665"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için bağlantı kitaplıkları
Bu makalede, MySQL için Azure veritabanına bağlanırken istemci programları kullanabileceğiniz her kitaplığı veya sürücü listelenmektedir.

## <a name="client-interfaces"></a>İstemci arabirimleri
MySQL ile uygulama ve ODBC ve JDBC sektör standartları ile uyumlu olan araçların kullanarak MySQL için standart veritabanı sürücü bağlantısı sunar. MySQL, ODBC veya JDBC ile çalışan tüm sistem kullanabilirsiniz.

| **Dil** | **Platform** | **Ek kaynak** | **İndir** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [MySQL için PHP - mysqlnd yerel sürücü](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [İndir](http://php.net/downloads.php) |
| ODBC | Windows, Linux, Mac OS X ve UNIX platformları | [MySQL Connector/ODBC Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-odbc/en/) | [İndir](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [MySQL Connector/Net Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-net/en/) | [İndir](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Platform bağımsız | [MySQL Connector/J 5.1 Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-j/5.1/en/) | [İndir](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares/node-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [İndir](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [MySQL Connector/Python Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-python/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [MySQL Connector/C++ Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-cpp/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [MySQL Connector/C Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-c/en/) | [İndir](https://dev.mysql.com/downloads/connector/c/)
| Perl | Windows, Linux, Mac OS X ve UNIX platformları | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [İndir](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçlara bağlanın ve istediğiniz dilde kullanarak MySQL için Azure veritabanı sorgulama hakkında okuyun:

[PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (C#)](./connect-csharp.md) | [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md)  |  [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [gidin](./connect-go.md)

