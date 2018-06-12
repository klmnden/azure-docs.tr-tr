---
title: MySQL için Azure veritabanı için bağlantı kitaplıkları
description: Bu makalede, her istemci programları için Azure veritabanı için MySQL bağlanırken kullanabileceği kitaplığı veya sürücü listelenmektedir.
services: mysql
author: mswutao
ms.author: wutao
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: c826bf1cf17230563b608e764c443b6166f13924
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264012"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için bağlantı kitaplıkları
Bu makalede, her istemci programları için Azure veritabanı için MySQL bağlanırken kullanabileceği kitaplığı veya sürücü listelenmektedir.

## <a name="client-interfaces"></a>İstemci arabirimleri
MySQL uygulama ve ODBC ve JDBC endüstri standartları ile uyumlu olan araçlar MySQL kullanmak için standart veritabanı sürücü bağlantısı sunar. ODBC veya JDBC çalışır sistem MySQL kullanabilirsiniz.

| **Dil** | **Platform** | **Ek kaynaklar** | **İndir** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [MySQL için PHP - mysqlnd yerel sürücü](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [İndir](http://php.net/downloads.php) |
| ODBC | Windows, Linux, Mac OS X ve UNIX platformları | [MySQL bağlayıcı/ODBC Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-odbc/en/) | [İndir](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [MySQL Connector/Net Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-net/en/) | [İndir](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Platform bağımsız | [MySQL bağlayıcı/J 5.1 Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-j/5.1/en/) | [İndir](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares/node-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [İndir](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [MySQL bağlayıcı/Python Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-python/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [MySQL bağlayıcı/C++ Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-cpp/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [MySQL bağlayıcı/C Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-c/en/) | [İndir](https://dev.mysql.com/downloads/connector/c/)
| Perl | Windows, Linux, Mac OS X ve UNIX platformları | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [İndir](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Sonraki adımlar
Bu quickstarts bağlanmak ve Azure veritabanı için MySQL sorguyu dilinizi tercih kullanarak hakkında okuyun:

[PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (C#)](./connect-csharp.md) | [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md)  |  [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [gidin](./connect-go.md)

