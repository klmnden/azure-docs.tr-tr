---
title: "PostgreSQL uzantıları için PostgreSQL Azure veritabanı'ndaki kullanarak | Microsoft Docs"
description: "Uzantıları için PostgreSQL Azure veritabanı'nda kullanarak veritabanını işlevselliğini genişletme olanağı açıklar."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: a80b27dc8f1a15bf2e62c9992be8bfa02cacb2f6
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>Azure veritabanı PostgreSQL için PostgreSQL uzantıları
PostgreSQL uzantıları kullanarak veritabanını işlevselliğini genişletme olanağı sağlar. Birden çok ilişkili SQL nesneleri birlikte yüklenen veya tek bir komutla veritabanınızdan kaldırılan tek bir pakette paketleme için uzantılar sağlar. Yerleşik özellikler gibi veritabanında yüklenen sonra uzantıları çalışabilir. PostgreSQL uzantıları hakkında daha fazla bilgi için bkz: [uzantı paketleme ilgili nesnelerini](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>PostgreSQL uzantıları kullanma
Kullanabilmek için önce PostgreSQL uzantıları veritabanınızda yüklenmesi gerekir. Belirli bir uzantıyı yüklemek için [UZANTI Oluştur](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) paketlenmiş nesneleri veritabanınıza yük psql aracından komutu.

Azure veritabanı PostgreSQL için şu anda aşağıda listelenen anahtar uzantıları kümesini destekler. Uzantıları listelenenlerden ötesinde desteklenmez; kendi uzantısı PostgreSQL hizmeti için Azure veritabanı oluşturulamıyor.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Azure veritabanı tarafından PostgreSQL için desteklenen uzantıları
Azure veritabanı tarafından PostgreSQL için şu anda desteklenen standart PostgreSQL uzantıları aşağıdaki tablolarda listelenmektedir. Bu bilgiler ayrıca sorgulayarak kullanılabilir `pg\_available\_extensions`.

### <a name="data-types-extensions"></a>Veri türleri uzantıları

> [!div class="mx-tableFixed"]
| **Uzantısı** | **Açıklama** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Bir veri türü için otomatik şifrelenmiş parolalar sağlar. |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Bir küçük harf karakter dize türü sağlar. |
| [Küp](https://www.postgresql.org/docs/9.6/static/cube.html) | Bir veri türü için boyutlu küp sağlar. |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Anahtar/değer çiftleri kümesi depolamak için bir veri türü sağlar. |
| [yüklenmez](https://www.postgresql.org/docs/9.6/static/isn.html) | Veri türleri standartları numaralandırma Uluslararası Ürün sağlar. |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Bir veri türü için hiyerarşik ağaç benzeri yapıları sağlar. |

### <a name="functions-extensions"></a>İşlevler uzantıları

> [!div class="mx-tableFixed"]
| **Uzantısı** | **Açıklama** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Dünya yüzeyinde mükemmel bir daire uzaklıklar hesaplamak için bir yol sağlar. |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Benzerlikler ve dizeler arasındaki uzaklığı belirlemek için çeşitli işlevleri sağlar. |
| [int](https://www.postgresql.org/docs/9.6/static/intarray.html) | İşlevler ve işleçler null serbest diziler tamsayıların yönlendirmek için sağlar. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Şifreleme işlevleri sağlar. |
| [PG\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Bölümlenmiş tablolar zaman ya da kimliği tarafından yönetir. |
| [PG\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | İşlevler ve işleçler trigram eşleşmesini temel alan alfasayısal metin benzerlik belirlemek için sağlar. |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Çapraz dahil olmak üzere tüm tablolar işlemek işlevleri sağlar. |
| [UUID ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Evrensel benzersiz tanımlayıcıları (UUID) oluşturur. |

### <a name="full-text-search-extensions"></a>Tam metin araması uzantıları

> [!div class="mx-tableFixed"]
| **Uzantısı** | **Açıklama** |
|---|---|
| [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Metin arama sözlük şablonu için tamsayılar sağlar. |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Lexemes vurgular (vurgu işaretlerini) kaldırır metin arama sözlük. |

### <a name="index-types-extensions"></a>Dizin türleri uzantıları

> [!div class="mx-tableFixed"]
| **Uzantısı** | **Açıklama** |
|---|---|
| [BTree\_GIN](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Gibi belirli veri türleri için davranış B-Ağacı uygulayan örnek GIN işleci sınıflar sağlar. |
| [BTree\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | B-Ağacı uygulayan GiST dizin işleci sınıflar sağlar. |

### <a name="language-extensions"></a>Dil uzantıları

> [!div class="mx-tableFixed"]
| **Uzantısı** | **Açıklama** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL yordam dili yüklenebilir. |

### <a name="miscellaneous-extensions"></a>Çeşitli uzantıları

> [!div class="mx-tableFixed"]
| **Uzantısı** | **Açıklama** |
|---|---|
| [PG\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Gerçek zamanlı paylaşılan arabellek önbelleğindeki neler olduğunu incelemek için bir yol sağlar. |
| [PG\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Arabellek önbelleğine ilişkisi verilerini yüklemek için bir yol sağlar. |
| [PG\_stat\_deyimleri](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Bir sunucu tarafından yürütülen tüm SQL deyimlerini Yürütme istatistiklerini izlemek için bir yol sağlar. |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Satır düzeyi kilitleme bilgi göstermek için bir yol sağlar. |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Tuple düzeyi istatistikleri göstermek için bir yol sağlar. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Dış PostgreSQL sunucuları depolanan verilere erişmek için kullanılan yabancı veri sarmalayıcı. |

### <a name="postgis-extensions"></a>PostGIS uzantıları

> [!div class="mx-tableFixed"]
| **Uzantısı** | **Açıklama** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topoloji, postgis\_tiger\_geocoder, postgis\_sfcgal | Uzamsal ve coğrafi nesneleri PostgreSQL için. |
| Adres\_standardizer, adresi\_standardizer\_veri\_bize | Bir adresi bağlı elemanlara ayrıştırmak için kullanılır. Coğrafi kodlama adresi normalleştirme adım desteklemek için kullanılır. |
| [grouting](http://pgrouting.org/) | PostGIS genişletir / PostgreSQL Jeo-uzamsal veritabanı Jeo-uzamsal sağlamak için yönlendirme işlevi. |

## <a name="next-steps"></a>Sonraki adımlar
Kullanmak için bize bildirin istediğiniz uzantı görmüyorsanız. Var olan istekleri için oy verin veya yeni geri bildirim ve istekleri oluşturmak bizim [müşteri geri bildirim Forumunda](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
