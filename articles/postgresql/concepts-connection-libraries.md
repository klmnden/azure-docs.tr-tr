---
title: PostgreSQL - tek bir sunucu için Azure veritabanı için bağlantı kitaplıkları
description: Bu makalede çeşitli kitaplıklarını ve sürücüleri geliştiricilerin ne zaman kullanabileceğini kodlama bağlanıp - tek bir sunucu PostgreSQL için Azure veritabanı sorgulamak için uygulamalar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 85110126f9bdec225b1644860814cd89832132a1
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65073605"
---
# <a name="connection-libraries-for-azure-database-for-postgresql---single-server"></a>PostgreSQL - tek bir sunucu için Azure veritabanı için bağlantı kitaplıkları
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
