---
title: MySQL için Azure veritabanı için bağlantı kitaplıkları
description: Bu makalede, MySQL için Azure veritabanına bağlanırken istemci programları kullanabileceğiniz her kitaplığı veya sürücü listelenmektedir.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 6ce0f2c761ede7d326f52f4d93d7f1b0bfa98cb2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60525543"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için bağlantı kitaplıkları
Bu makalede, MySQL için Azure veritabanına bağlanırken istemci programları kullanabileceğiniz her kitaplığı veya sürücü listelenmektedir.

## <a name="client-interfaces"></a>İstemci arabirimleri
MySQL ile uygulama ve ODBC ve JDBC sektör standartları ile uyumlu olan araçların kullanarak MySQL için standart veritabanı sürücü bağlantısı sunar. MySQL, ODBC veya JDBC ile çalışan tüm sistem kullanabilirsiniz.

| **Dil** | **Platform** | **Ek kaynak** | **İndir** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [MySQL için PHP - mysqlnd yerel sürücü](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [İndir](https://secure.php.net/downloads.php) |
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

[PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (C#)](./connect-csharp.md) | [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [Go](./connect-go.md)

