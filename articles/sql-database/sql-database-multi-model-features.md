---
title: Azure SQL veritabanı çok model özellikleri | Microsoft Docs
description: Azure SQL veritabanı, aynı veritabanında birden çok veri modelleri ile çalışmanıza olanak sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
manager: craigg
ms.date: 12/17/2018
ms.openlocfilehash: 4351017cc1848e29cca038f82fd96548ae3492e0
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892475"
---
# <a name="multi-model-capabilities-of-azure-sql-database"></a>Azure SQL veritabanı çok modelli özellikleri

Çok modelli veritabanları depolamak ve ilişkisel veri, grafikler, JSON/XML belgeleri, anahtar-değer çiftleri, vb. gibi birden çok veri biçimini temsil verilerle çalışmanıza olanak sağlar.

## <a name="when-to-use-multi-model-capabilities"></a>Çok modelli özellikleri kullanmak ne zaman

Azure SQL veritabanı, çeşitli genel amaçlı uygulamalar için en iyi durumda en iyi performans sağlayan ilişkisel modeli ile çalışmak üzere tasarlanmıştır. Ancak, Azure SQL veritabanı, ilişkisel veri için yalnızca sınırlı değildir. Azure SQL veritabanı, ilişkisel model tümleştirildiği ilişkisel olmayan biçimleri çeşitli kullanmanıza olanak sağlar.
Azure SQL veritabanı çok modelli özellikleri aşağıdaki durumlarda kullanmayı düşünmeniz gerekir:
- Bazı bilgiler varsa veya NoSQL modelleri ve sizin için daha iyi uyacak yapıları ayrı bir NoSQL veritabanı kullanmak istemiyorsanız.
- Verilerinizi çoğunu ilişkisel modeli için uygundur ve bazı bölümlerini bir NoSQL verilerinizi modellemek gerekir.
- Zengin Transact-SQL dili de sorgulanması ve çözümlenmesi ilişkisel ve NoSQL veri yararlanın ve çeşitli araçları ve SQL dil kullanan uygulamalar ile tümleştirmek istediğiniz.
- Veritabanı özellikleri gibi uygulamak istediğiniz [bellek içi teknolojileri](sql-database-in-memory.md) , analiz performansını artırmak veya NoSQL veri strucutres işleme [işlemsel çoğaltma](sql-database-managed-instance-transactional-replication.md) veya [okunabilir çoğaltma](sql-database-read-scale-out.md) üzerinde başka bir yerde verilerinizi bir kopyasını oluşturun ve birincil veritabanından analitik bazı iş yüklerini boşaltma.

## <a name="overview"></a>Genel Bakış

Azure SQL, çok modelli aşağıdaki özellikleri sağlar:
- [Grafik özellikleri](#graph-features) grafik ile Gelişmiş standart Transact-SQL sorguları kullanın ve verilerinizi temsil eden düğümler ve kenarlar kümesi olarak tanır `MATCH` grafik verilerini sorgulamak için işleci.
- [JSON özellikleri](#json-features) , JSON belgelerini tablolarında yerleştirme, JSON belgeleri ve ilişkili verileri dönüştürme sağlar. JSON belgelerini ayrıştırma işlevleri ile Gelişmiş standart Transact-SQL dili kullanın ve olmayan Kümelenmiş dizinler, columnstore dizinleri veya bellek için iyileştirilmiş tablolar, sorgularınızı iyileştirmeniz için kullanın.
- [Uzamsal Özellikler](#spatial-features) coğrafi ve geometrik verilerini depolamak, uzamsal dizinler kullanarak bunları dizin ve uzamsal sorgular kullanarak verileri almak sağlar.
- [XML özelliklerini](#xml-features) depolamak ve veritabanınızdaki XML verileri dizin ve XML verileri ile çalışmak için yerel XQuery/XPath işlemlerini kullanmak olanak sağlar. Azure SQL veritabanı, XML verilerini işleme yerleşik XML sorgu altyapısı özelleştirilmiş.
- [Anahtar-değer çiftleri](#key-value-pairs) anahtar-değer paris, iki sütunlu tabloları olarak yerel olarak modellenebilir olduğundan özel özellikler açıkça desteklenmez.

  > [!Note]
  > Veritabanında depolanan tüm verilere erişmek için aynı Transact-SQL sorgusuna JSON yolu ifadesini, XQuery/XPath ifadeleri, uzamsal işlevler ve grafik sorgu ifadeleri kullanabilirsiniz. Ayrıca, herhangi bir aracı veya Transact-SQL sorguları yürütebilir programlama dili de sorgu arabirimi çok modelli verilere erişmek için kullanabilirsiniz. Gibi çok modelli veritabanlarına kıyasla önemli bir fark budur [Azure Cosmos DB](/azure/cosmos-db/) farklı veri modelleri için özel bir API sağlar.

Aşağıdaki bölümlerde, Azures SQL veritabanı çok modelli en önemli özellikleri hakkında bilgi edinebilirsiniz.

## <a name="graph-features"></a>Grafik özellikleri

Azure SQL veritabanı, veritabanında çok-çok ilişkileri modellemek için grafik veritabanı işlevleri sunar. Bir grafik düğümleri (veya köşelerini) bir koleksiyonudur ve kenarlar (veya ilişkileri). Bir varlığa (örneğin, bir kişi veya kuruluş) bir düğümü temsil eder ve bir kenarı (örneğin, beğenilerin veya arkadaş) bağlanan iki düğüm arasındaki bir ilişkiyi temsil eder. Bir grafik veritabanı benzersiz hale özelliklerinden bazıları şunlardır:
- Kenarları veya ilişkileri birinci sınıf bir grafik veritabanı içinde varlıklardır ve öznitelikleri veya özellikleri kendileriyle ilişkili.
- Tek bir kenar birden çok düğüm bir grafik veritabanı, esnek bir şekilde bağlanabilirsiniz.
- Desen eşleştirme ve çok atlamalı Gezinti sorguları kolayca ifade edebilirsiniz.
- Geçişli kapatma ve çok biçimli sorgu kolayca ifade edebilirsiniz.

Graf ilişkileri ve grafik sorgu işlevleri Transact-SQL ile Tümleştirildi ve temel veritabanı yönetim sistemi olarak SQL Server'ı kullanmanın avantajları alma.
[Grafik işleme](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) olduğundan çekirdek SQL Server veritabanı altyapısı özelliği var. işleme Graph hakkında daha fazla bilgi bulabilirsiniz.

### <a name="when-to-use-a-graph-capability"></a>Graf özellikten yararlanabilmek ne zaman

Bir şey yok bir grafik veritabanı ulaşabilir, hangi ilişkisel veritabanı kullanarak elde edemiyor. Ancak, bir grafik veritabanı bazı sorgular express daha kolay yapabilirsiniz. Kararınız birinin yerine diğerini seçmek için aşağıdaki etmenlere dayalı olabilir:

- Bu nedenle HierarchyId kullanılamaz bir düğümü birden çok üst öğeye burada sahip olabilir hiyerarşik veri modeli
- Modeli vardır, uygulamanızın karmaşık çok-çok ilişkisi; var Uygulama geliştikçe yeni ilişkiler eklenir.
- Birbirine bağlı veri ve ilişkilerini analiz etmeniz.

## <a name="json-features"></a>JSON özellikleri

Azure SQL veritabanı sağlar ayrıştırma ve JavaScript nesne gösterimi ' gösterilen veri sorgulama [(JSON)](https://www.json.org/) biçimlendirmek ve JSON metni olarak, ilişkisel verilerinizi dışarı aktarın.

JSON, modern web ve mobil uygulamalarda veri değişimi için kullanılan popüler veri biçimidir. JSON, yarı yapılandırılmış verileri gibi NoSQL veritabanları veya günlük dosyalarını depolamak için de kullanılır [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Sonuçlar JSON metni olarak biçimlendirilmiş veya verileri kabul birçok REST web hizmetleri JSON tarafından biçimlendirilmiş. Çoğu Azure Hizmetleri gibi [Azure Search](https://azure.microsoft.com/services/search/), [Azure depolama](https://azure.microsoft.com/services/storage/), ve [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) döndürür veya JSON tüketen bir REST uç noktaları vardır.

Azure SQL veritabanı, JSON ile kolayca çalışmanızı ve veritabanınızı modern Hizmetleri ile tümleştirme sağlar. Azure SQL veritabanı, JSON verileri ile çalışmak için aşağıdaki işlevleri sağlar:

![JSON işlevleri](./media/sql-database-json-features/image_1.png)

JSON metnini varsa JSON'dan veri ayıklama veya yerleşik işlevleri kullanarak JSON düzgün biçimlendirildiğini doğrulayın [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), ve [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). [Json_modıfy](https://msdn.microsoft.com/library/dn921892.aspx) işlevi değeri JSON metnine içinde güncelleştirmenize olanak tanır. Daha gelişmiş sorgulama ve analiz için [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) işlevi bir dizi satır bir JSON nesne dizisi dönüştürme. Herhangi bir SQL sorgu döndürülen sonuç kümesi üzerinde çalıştırılabilir. Son olarak, bir [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) olanak sağlayan yan tümcesi biçimlendirme ilişkisel tabloları JSON metni olarak depolanan veriler.

Daha fazla bilgi için [JSON verilerini azure SQL veritabanı ile nasıl çalışılacağını](sql-database-json-features.md).
[JSON](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) olduğundan çekirdek SQL Server veritabanı altyapısı özelliği var. JSON özelliği hakkında daha fazla bilgi bulabilirsiniz.

### <a name="when-to-use-a-json-capability"></a>Ne zaman bir JSON özellik kullanılır?

Belge modelleri yerine ilişkisel model belli başlı bazı senaryolarda kullanılabilir:
- Yüksek normalleştirme şemasının nesnelerin tüm alanlar aynı anda erişim veya hiçbir zaman nesneleri normalleştirilmiş bölümlerini güncelleştirmek için önemli avantajlar getirmek değil. Ancak, normalleştirilmiş modeli çok sayıda veri almak için katılmak için gereken tabloları nedeniyle, sorguların karmaşıklığı artırır.
- Yerel olarak iletişim veya veri modelleri kullanım JSON belgeleri olan ve ilişkisel veri, JSON ve dönüştüren ek Katmanlar tanıtmak istemediğiniz uygulamalar ile çalışıyoruz.
- Veri modelinizi XML'deki normalleştirilmesi alt tablolar veya varlık nesne değeri desenleri basitleştirmek gerekir.
- Yükleme veya verileri ayrıştırmak için bazı ek aracı olmadan JSON biçiminde depolanan verileri dışarı aktarmak gerekir.

## <a name="spatial-features"></a>Uzamsal Özellikler

Uzamsal veriler fiziksel konuma ve nesneleri geometrik şeklin ilgili bilgileri temsil eder. Bu nesneler noktası konumu veya ülkeler, yolların veya bellek gibi daha karmaşık nesneler olabilir.

Azure SQL veritabanı, geometri veri türü ve coğrafi veriler'e yazın, iki uzamsal veri türleri - destekler.
- Geometri türü Euclidean (düz) bir koordinat sisteminde verileri temsil eder.
- Coğrafi konum türü bir gidiş-dünya koordinat sistemi verileri temsil eder.

Azure SQL veritabanı'nda aşağıdaki gibi kullanılabilir uzamsal nesneleri sayıda [noktası](https://docs.microsoft.com/sql/relational-databases/spatial/point), [LineString](https://docs.microsoft.com/sql/relational-databases/spatial/linestring), [Çokgen](https://docs.microsoft.com/sql/relational-databases/spatial/polygon)vb.

Azure SQL veritabanı ayrıca sağlayan özelleştirilmiş [uzaysal dizinler](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-indexes-overview) uzamsal sorgularınızı performansını artırmak için kullanılabilir.

[Uzamsal Destek](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) çekirdek SQL Server veritabanı altyapısı özelliği olduğundan, uzamsal özelliği hakkında daha fazla bilgi bulabilirsiniz.

## <a name="xml-features"></a>XML özellikleri

SQL Server, yarı yapılandırılmış veri yönetimi için zengin uygulamalar geliştirmeye yönelik güçlü bir platform sağlar. XML için destek, SQL Server'daki tüm bileşenleri tümleşik ve aşağıdakileri içerir:

- Xml veri türü. XML değerleri, yerel olarak bir xml veri türü sütununda XML şema koleksiyonu göre belirlenmiş veya sol türsüz depolanabilir. XML sütunu dizine ekleyebilir.
- XML veri sütunları ve xml türünde değişkenler, depolanan bir XQuery sorgusunu belirtme olanağı. Veritabanınızda kullanan herhangi bir veri modeli erişim herhangi bir Transact-SQL sorgu XQuery işlevleri kullanılabilir.
- XML belgeleri kullanarak tüm öğeler otomatik olarak dizinini [birincil XML dizini](https://docs.microsoft.com/sql/relational-databases/xml/xml-indexes-sql-server#primary-xml-index) veya kullanılarak dizine tam yollarını belirtin [ikincil XML dizini](https://docs.microsoft.com/sql/relational-databases/xml/xml-indexes-sql-server#secondary-xml-indexes).
- Bu OPENROWSET XML veri toplu yüklenmesine izin verir.
- İlişkisel verileri XML biçimine dönüştürün.

[XML](https://docs.microsoft.com/sql/relational-databases/xml/xml-data-sql-server) olduğundan çekirdek SQL Server veritabanı altyapısı özelliği, XML özellik var. hakkında daha fazla bilgi bulabilirsiniz.

### <a name="when-to-use-an-xml-capability"></a>Ne zaman bir XML özellik kullanılır?

Belge modelleri yerine ilişkisel model belli başlı bazı senaryolarda kullanılabilir:
- Yüksek normalleştirme şemasının nesnelerin tüm alanlar aynı anda erişim veya hiçbir zaman nesneleri normalleştirilmiş bölümlerini güncelleştirmek için önemli avantajlar getirmek değil. Ancak, normalleştirilmiş modeli çok sayıda veri almak için katılmak için gereken tabloları nedeniyle, sorguların karmaşıklığı artırır.
- Yerel olarak kullanım XML belgeleri iletişim veya veri modelleri olan ve ilişkisel verileri XML ve dönüştüren ek Katmanlar tanıtmak istemediğiniz uygulamalar ile çalışıyoruz.
- Veri modelinizi XML'deki normalleştirilmesi alt tablolar veya varlık nesne değeri desenleri basitleştirmek gerekir.
- Yükleme veya verileri ayrıştırmak için bazı ek aracı olmadan XML biçiminde depolanan verileri dışarı aktarmak gerekir.

## <a name="key-value-pairs"></a>Anahtar-değer çiftleri

Azure SQL veritabanı özel türleri veya anahtar-değer çiftleri standart ilişkisel tabloları anahtar-değer yapıları yerel olarak temsil edilebilir beri destekleyen yapıları gerekmez:

```sql
CREATE TABLE Collection (
  Id int identity primary key,
  Data nvarchar(max)
)
```

Bu anahtar-değer yapısı kısıtlama olmadan kendi gereksinimlerinize uyacak şekilde özelleştirebilirsiniz. Örneğin, değer yerine XML belgesi olabilir `nvarchar(max)` türü, değer JSON belgesini ise, koyabilir `CHECK` JSON içeriği geçerliliğini doğrular kısıtlaması. Herhangi bir sayıda ek sütunlar bir anahtarda ilgili değerleri yerleştirme, hesaplanan sütunlar ekleyip basitleştirmek ve veri erişimini iyileştirmek için dizinler, tablonun vb. daha iyi performans almak için belleği ve iyileştirilmiş yalnızca şema tablo olarak tanımlayın.

Bkz: [BWin benzeri görülmemiş bir performans ve ölçek elde etmek için bellek içi OLTP nasıl kullandığını](https://blogs.msdn.microsoft.com/sqlcat/20./../how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/) kendi ASP.NET önbelleğe alma için 1.200.000 elde çözüm toplu işlemleri nasıl ilişkisel bir örnek olarak, saniye başına model etkin olarak kullanılabilir uygulamada bir anahtar-değer çifti çözüm.

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanları, çok modelli özellikleri, ayrıca Azure SQL veritabanı ve SQL Server arasında paylaşılan temel SQL Server veritabanı altyapısı özellikleri değildir. Bu özellikler hakkında daha fazla bilgi edinmek için SQL ilişkisel veritabanı belge sayfaları ziyaret edin:

* [Grafik işleme](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview)
* [JSON verileri](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server)
* [Uzamsal destek](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [XML verileri](https://docs.microsoft.com/sql/relational-databases/xml/xml-data-sql-server)
