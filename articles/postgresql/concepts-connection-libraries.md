---
title: "Azure veritabanı PostgreSQL için için bağlantı kitaplıkları | Microsoft Docs"
description: "Bu makalede birkaç kitaplıkları ve geliştiricilerin ne zaman kullanabileceğini sürücüleri kodlama bağlanmak ve PostgreSQL için Azure veritabanını sorgulamak için uygulamalar."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: f371d5cd4e20096d5101fadf9066e3a135218d0b
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Azure veritabanı PostgreSQL için için bağlantı kitaplıkları
Bu makalede, kitaplıklar ve geliştiriciler bağlanmak ve PostgreSQL için Azure veritabanını sorgulamak için uygulamaları geliştirmek için kullanabilir sürücüleri listeler.

## <a name="client-interfaces"></a>İstemci arabirimleri
Çoğu dil istemci kitaplıkları PostgreSQL sunucusuna bağlanmak için kullanılan dış projeler ve bağımsız olarak dağıtılır. Listelenen kitaplıkları için PostgreSQL Azure veritabanına bağlanmak için Windows, Linux ve Mac platformları üzerinde desteklenir. Birkaç Hızlı Başlangıç örnekleri sonraki adımlar bölümünde listelenir.

| **Dil** | **İstemci arabirimi** | **Ek bilgi** | **İndir** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2.0 uyumlu | [İndir](http://initd.org/psycopg/download/) |
| PHP | [PHP pgsql](https://php.net/manual/en/book.pgsql.php) | Veritabanı uzantısı | [Yükleme](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [PG npm paket](https://www.npmjs.com/package/pg) | Saf JavaScript engelleyici olmayan istemci | [Yükleme](https://www.npmjs.com/package/pg) |
| Java | [JDBC](http://jdbc.postgresql.org/) | Tür 4 JDBC sürücüsü | [İndir](https://jdbc.postgresql.org/download.html)  |
| Ruby | [PG gem](https://deveiate.org/code/pg/) | Söyleniş arabirimi | [İndir](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Başlayın | [Paket pq](https://godoc.org/github.com/lib/pq) | Saf Git postgres sürücüsü | [Yükleme](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](http://www.npgsql.org/) | ADO.NET veri sağlayıcı | [İndir](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC Sürücüsü | [İndir](http://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Birincil C dil arabirimi | Dahil |
| C++ | [libpqxx](http://pqxx.org/) | Yeni stil C++ arabirimi | [İndir](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Sonraki adımlar
Bu quickstarts bağlanmak ve dilinizi tercih kullanarak PostgreSQL için Azure veritabanını sorgulamak nasıl okuyun:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md)  |  [.NET (C#)](./connect-csharp.md) | [gidin](./connect-go.md)
