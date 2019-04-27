---
title: PostgreSQL için Azure veritabanı için bağlantı kitaplıkları
description: Bu makalede çeşitli kitaplıkları ve geliştiricilerin ne zaman kullanabileceğini sürücüleri kodlama bağlanmak ve PostgreSQL için Azure veritabanı sorgulamak için uygulamalar.
services: postgresql
author: WenJason
ms.author: v-jay
manager: digimobile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
origin.date: 02/28/2018
ms.date: 12/03/2018
ms.openlocfilehash: 0e762a2d7cf82e2957fb276fcea0a20553f719e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60559781"
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı için bağlantı kitaplıkları
Bu makalede, kitaplıkları ve geliştiricilerin bağlanmak ve PostgreSQL için Azure veritabanını sorgulamak için uygulamalar geliştirmek için kullanabilecekleri sürücüleri listeler.

## <a name="client-interfaces"></a>İstemci arabirimleri
PostgreSQL sunucusuna bağlanmak için kullanılan birçok dil istemci kitaplıkları, dış projeler ve bağımsız olarak dağıtılır. Listelenen kitaplıkları, PostgreSQL için Azure veritabanı'na bağlanmak için Windows, Linux ve Mac platformları üzerinde desteklenir. Birkaç Hızlı Başlangıç örnekleri sonraki adımlar bölümünde listelenir.

| **Dil** | **İstemci arabirimi** | **Ek bilgi** | **İndir** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2.0 uyumlu | [İndir](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Veritabanı uzantısı | [Yükleme](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [PG npm paketi](https://www.npmjs.com/package/pg) | Engelleyici olmayan saf JavaScript istemcisi | [Yükleme](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Tür 4 JDBC sürücüsü | [İndirme](https://jdbc.postgresql.org/download.html)  |
| Ruby | [PG gem](https://deveiate.org/code/pg/) | Ruby arabirimi | [İndir](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Başlayın | [Paket pq](https://godoc.org/github.com/lib/pq) | Pure Go postgres sürücüsünü | [Yükleme](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](https://www.npgsql.org/) | ADO.NET veri sağlayıcısı | [İndir](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC Sürücüsü | [İndir](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Birincil C dili arabirimi | Dahil |
| C++ | [libpqxx](http://pqxx.org/) | Yeni stil C++ arabirimi | [İndir](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçlara bağlanın ve istediğiniz dilde kullanarak PostgreSQL için Azure veritabanı sorgulama hakkında okuyun:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
