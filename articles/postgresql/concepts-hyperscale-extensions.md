---
title: PostgreSQL - hiper ölçekli (Citus) (Önizleme) için Azure veritabanı'nda PostgreSQL uzantıları
description: PostgreSQL için Azure veritabanı'nda uzantılarını kullanarak veritabanınızı işlevselliğini genişletme özelliğini açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 4022c95bfda8cbdaed75876793bfbba4254a5c54
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410260"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql---hyperscale-citus-preview"></a>PostgreSQL - hiper ölçekli (Citus) (Önizleme) için Azure veritabanı'nda PostgreSQL uzantıları

PostgreSQL Uzantıları'nı kullanarak veritabanını genişletmek olanağı sağlar. Birden çok ilişkili SQL nesneleri birlikte yüklenen ya da tek bir komutla veritabanından kaldırıldı tek bir pakette paketleme için uzantılar sağlar. Yerleşik özellikler gibi veritabanında yüklenen sonra uzantıları çalışabilir. PostgreSQL uzantıları hakkında daha fazla bilgi için bkz. [paketleme ilgili nesneleri uzantı](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>PostgreSQL uzantıları kullanma

Kullanabilmeniz için önce veritabanınızı PostgreSQL uzantıları yüklenmelidir. Belirli bir uzantıyı yüklemek için çalıştırın [oluşturma UZANTISI](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) psql aracından paketlenmiş nesneler veritabanınıza yüklemek için komutu.

PostgreSQL için Azure veritabanı, şu anda aşağıda listelenen anahtar uzantıları kümesini destekler. Uzantılar listelenenlerden ötesinde desteklenmiyor; kendi uzantınızı PostgreSQL hizmeti için Azure veritabanı ile oluşturulamıyor.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı tarafından desteklenen uzantılar

Aşağıdaki tablolar, şu anda PostgreSQL için Azure veritabanı tarafından desteklenen standart PostgreSQL uzantıları listeler. Bu bilgiler ayrıca çalıştırarak kullanılabilir `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Veri türü uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Büyük küçük harf duyarsız bir karakter dizesi türü sağlar. |
> | [Küp](https://www.postgresql.org/docs/9.6/static/cube.html) | Bir veri türü için çok boyutlu küpler sağlar. |
> | [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Anahtar/değer çiftleri kümesi depolamak için bir veri türü sağlar. |
> | [yüklenmez](https://www.postgresql.org/docs/9.6/static/isn.html) | Veri türleri için Uluslararası Ürün standartları numaralandırma sağlar. |
> | [Lo](https://www.postgresql.org/docs/current/lo.html) | Büyük nesne Bakımı. |
> | [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Bir veri türü, hiyerarşik ağaç benzeri yapıları için sağlar. |
> | [seg](https://www.postgresql.org/docs/current/seg.html) | Çizgi segmentleri veya kayan nokta aralıklarını temsil etmek için veri türü. |
> | [üst n](https://github.com/citusdata/postgresql-topn/) | Üst n JSONB türü. |

### <a name="functions-extensions"></a>İşlevleri uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [autoinc](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.7) | Otomatik artan alanlar işlevleri. |
> | [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Dünya yüzeyinde Great daire uzaklıkları hesaplamak için bir yol sağlar. |
> | [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Benzerlikleri ve dizeler arasındaki uzaklığı belirlemek için çeşitli işlevler sağlar. |
> | [INSERT\_kullanıcı adı](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.8) | İşlevler, tablo kimin değiştirdiğini izlemek için. |
> | [intagg](https://www.postgresql.org/docs/current/intagg.html) | Tamsayı toplayıcısını ve Numaralandırıcı (eski). |
> | [int](https://www.postgresql.org/docs/9.6/static/intarray.html) | Tamsayı dizileri null ücretsiz işlemek için İşlevler ve işleçler sağlar. |
> | [moddatetime](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.9) | Son değiştirme zamanı izleme işlevleri. |
> | [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Şifreleme işlevlerini sağlar. |
> | [PG\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Bölümlenmiş tablolar zaman ya da kimliği tarafından yönetir. |
> | [PG\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Alfasayısal metin trigram eşleşmesi temeline göre benzerlik belirlemek için İşlevler ve işleçler sağlar. |
> | [refint](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.5) | Bilgi tutarlılığı (eski) uygulamak için işlevleri. |
> | oturum\_analizi | Hstore diziler sorgulamak için işlev. |
> | [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Çapraz dahil olmak üzere tüm tablolar işleme işlevleri sağlar. |
> | [tcn](https://www.postgresql.org/docs/current/tcn.html) | Değişiklik bildirimleri tetiklenir. |
> | [timetravel](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.6) | Zaman seyahat uygulamak için işlevleri. |
> | [uuid ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Evrensel benzersiz tanımlayıcı (UUID) oluşturur. |

### <a name="full-text-search-extensions"></a>Tam metin arama uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Tamsayılar için metin arama sözlük şablonu sağlar. |
> | [dict\_xsyn](https://www.postgresql.org/docs/current/dict-xsyn.html) | Metin arama sözlük şablonu genişletilmiş eş anlamlı işleme. |
> | [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Lexemes vurgular (aksan işaretleri) kaldıran metin arama sözlüğü. |

### <a name="index-types-extensions"></a>Dizin türleri uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [çiçek tomurcukları](https://www.postgresql.org/docs/current/bloom.html) | Çiçek tomurcukları erişim yöntemi - imza dosyası dizini temel. |
> | [rowsetbulk\_GIN](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | B-ağacı gibi belirli veri türleri için davranışı uygulayan örnek GIN işleci sınıflar sağlar. |
> | [rowsetbulk\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | B-Ağacı uygulayan GiST dizin işleci sınıflar sağlar. |

### <a name="language-extensions"></a>Dil uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL yordam dili yüklenebilir. |

### <a name="hyperscale-extensions"></a>Hiper ölçekli uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [citus](https://github.com/citusdata/citus) | Citus dağıtılmış bir veritabanı. |
> | parça\_rebalancer | Güvenli bir şekilde yeniden dengelemeniz verilerini bir sunucu grubundan düğüm ekleme veya kaldırma durumunda. |

### <a name="miscellaneous-extensions"></a>Diğer uzantılar

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [Yönetim Paketi'nin](https://www.postgresql.org/docs/current/adminpack.html) | PostgreSQL için yönetim işlevlerini sağlar. |
> | [amcheck](https://www.postgresql.org/docs/current/amcheck.html) | İlişki bütünlüğünü doğrulamak için işlevleri. |
> | [Dosya\_fdw](https://www.postgresql.org/docs/current/file-fdw.html) | Düz dosya erişimi için yabancı veri sarmalayıcıdır. |
> | [pageinspect](https://www.postgresql.org/docs/current/pageinspect.html) | Düşük bir düzeyde veritabanlarının içeriğini inceleyin. |
> | [PG\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Gerçek zamanlı olarak paylaşılan arabellek önbelleğinde neler olduğunu İnceleme için bir yol sağlar. |
> | [PG\_cron](https://github.com/citusdata/pg_cron) | PostgreSQL için İş Zamanlayıcısı. |
> | [PG\_freespacemap](https://www.postgresql.org/docs/current/pgfreespacemap.html) | Boş alan eşlemesi (FSM) inceleyin. |
> | [PG\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | İlişki verileri arabellek önbelleğine yüklemek için bir yol sağlar. |
> | [PG\_stat\_deyimleri](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Sunucu tarafından yürütülen tüm SQL deyimleri yürütme istatistikleri izlemek için bir yol sağlar. (Aşağıda bir not üzerinde bu uzantı için bakın). |
> | [PG\_görünürlük](https://www.postgresql.org/docs/current/pgvisibility.html) | Görünürlük harita (VM) ve sayfa düzeyi görünürlük bilgileri inceleyin. |
> | [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Satır düzeyi kilitleme bilgilerini göstermek için bir yol sağlar. |
> | [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Tanımlama grubu düzeyinde istatistikleri göstermek için bir yol sağlar. |
> | [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Dış PostgreSQL sunucuları depolanan verilere erişmek için kullanılan yabancı veri sarmalayıcıdır. (Aşağıda bir not üzerinde bu uzantı için bakın).|
> | [sslinfo](https://www.postgresql.org/docs/current/sslinfo.html) | SSL sertifikaları hakkında bilgi. |
> | [TSM\_sistem\_satırları](https://www.postgresql.org/docs/current/tsm-system-rows.html) | Satır sayısı bir sınır olarak kabul eden TABLESAMPLE yöntemi. |
> | [TSM\_sistem\_zaman](https://www.postgresql.org/docs/current/tsm-system-time.html) | Milisaniye cinsinden süre sınırı olarak kabul eden TABLESAMPLE yöntemi. |
> | [hypopg](https://hypopg.readthedocs.io/en/latest/) | CPU veya disk maliyet yoktur kuramsal dizinler oluşturma bir yol sağlar. |
> | [dblink](https://www.postgresql.org/docs/current/dblink.html) | Veritabanı oturum içindeki başka bir PostgreSQL veritabanına bağlantıları destekleyen bir modül. (Aşağıda bir not üzerinde bu uzantı için bakın). |
> | [xml2](https://www.postgresql.org/docs/current/xml2.html) | XPath sorgulama ve XSLT. |


### <a name="postgis-extensions"></a>PostGIS uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [PostGIS](https://www.postgis.net/), postgis\_topoloji, postgis\_tiger\_geocoder, postgis\_sfcgal | PostgreSQL için uzamsal ve coğrafi nesneleri. |
> | adresi\_standardizer, adresi\_standardizer\_veri\_ABD | Bir adresi şekli oluşturan öğeler ayrıştırmak için kullanılır. Coğrafi kodlama adresi normalleştirme adım desteklemek için kullanılır. |
> | postgis\_sfcgal | PostGIS SFCGAL işlevleri. |
> | postgis\_tiger\_geocoder | PostGIS tiger geocoder ve ters geocoder. |
> | postgis\_topolojisi | PostGIS topolojisi uzamsal türleri ve işlevleri. |


## <a name="pgstatstatements"></a>pg_stat_statements
[Pg\_stat\_deyimleri uzantısı](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) her yürütme istatistikleri SQL deyimlerinin izleme bir yol sağlamak için Azure veritabanı üzerinde PostgreSQL sunucusu önceden yüklenmiş olduğu.
Ayar `pg_stat_statements.track`, denetleyen hangi deyimleri varsayılan uzantı tarafından sayılır `top`, doğrudan istemciler tarafından gönderilen tüm deyimleri izlenen anlamına gelir. İki diğer izleme düzeyleri `none` ve `all`. Bu ayar bir sunucu parametresi olarak yapılandırılabilir [Azure portalında](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) veya [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Sorgu yürütme bilgilerini pg_stat_statements sağlar ve sunucu performansı üzerindeki etkisini arasında bir denge yok olarak her SQL deyimi kaydeder. Etkin bir şekilde pg_stat_statements uzantı kullanmıyorsanız, ayarlamanızı öneririz `pg_stat_statements.track` için `none`. Bazı üçüncü taraf hizmetleri izleme sorgu performansı öngörüleri sunmak için bu nedenle bu durum sizin olup olmadığını onaylayın pg_stat_statements üzerinde System.Environment.UserInteractive unutmayın.

## <a name="dblink-and-postgresfdw"></a>dblink ve postgres_fdw
dblink ve postgres_fdw bir PostgreSQL sunucudan diğerine ya da aynı sunucuda başka bir veritabanına bağlanmanızı sağlar. Alıcı sunucu gönderen sunucu, güvenlik duvarı üzerinden gelen bağlantılara izin ver gerekir. Bu uzantı, PostgreSQL sunucuları için Azure veritabanı arasında bağlantı kurmak için kullanırken bu "ON Azure hizmetlerine erişime izin ver" ayarı yapılabilir. Bu, ayrıca aynı sunucuya geri dönmeye uzantıları kullanmak istiyorsanız gereklidir. "Azure hizmetlerine erişime izin ver" ayarı, bağlantı güvenliği altında Postgres server için Azure portal sayfasında bulunabilir. "İzin ver Azure hizmetlerine erişime izin" üzerinde beyaz tüm Azure IP'lere kapatma.

Şu anda, PostgreSQL için Azure veritabanı'ndan giden bağlantılar, diğer PostgreSQL sunucuları için Azure veritabanı bağlantılar dışında desteklenmez.
