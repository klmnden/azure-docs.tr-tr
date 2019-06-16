---
title: Ölçeklenebilir bulut veritabanları oluşturma | Microsoft Docs
description: Elastik veritabanı istemci kitaplığı kullanarak ölçeklenebilir .NET veritabanı uygulamalar oluşturun
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/25/2018
ms.openlocfilehash: 1c6e77f3afc90a8c018296db80253d8b9a22159e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66234116"
---
# <a name="building-scalable-cloud-databases"></a>Ölçeklenebilir bulut veritabanları oluşturma

Veritabanlarını ölçeklendirme, kolayca Azure SQL veritabanı için ölçeklenebilir araçları ve özellikleri kullanılarak gerçekleştirilebilir. Özellikle, kullanabileceğiniz **elastik veritabanı istemci Kitaplığı** ölçeği genişletilmiş veritabanları oluşturma ve yönetme için. Bu özellik, kolayca yüzlerce kullanarak parçalı uygulamalar geliştirmenize olanak sağlar; ve hatta binlerce — Azure SQL veritabanı.

İndirmek için:

* Java sürümü kitaplığının görmek [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* .NET sürümü kitaplığının görmek [NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

## <a name="documentation"></a>Belgeler

1. [Esnek Veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md)
2. [Elastik veritabanı özellikleri](sql-database-elastic-scale-introduction.md)
3. [Parça eşleme yönetimi](sql-database-elastic-scale-shard-map-management.md)
4. [Ölçeği genişletmek için mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)
6. [Çok parçalı sorgular](sql-database-elastic-scale-multishard-querying.md)
7. [Esnek veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md)
8. [Esnek veritabanı araçlarını ve satır düzeyi güvenlik ile çok kiracılı uygulamalar](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [İstemci Kitaplığı uygulamalarını yükseltme](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Elastik sorgular genel bakış](sql-database-elastic-query-overview.md)
11. [Esnek veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md)
12. [Entity Framework ile esnek veritabanı istemci kitaplığı](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Dapper ile esnek veritabanı istemci kitaplığı](sql-database-elastic-scale-working-with-dapper.md)
14. [Ayırma-Birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Parça eşleme yöneticisi için performans sayaçları](sql-database-elastic-database-client-library.md) 
16. [Esnek veritabanı araçları hakkında SSS](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>İstemci özellikleri

Kullanan uygulamaların ölçeklendirilmesi *parçalama* hem geliştiriciler hem de için yönetici zorluklar teşkil etmektedir. İstemci Kitaplığı, hem geliştiriciler olanak tanıyan Araçlar sağlayarak yönetim görevlerini basitleştirir ve ölçeği genişletilmiş veritabanları yöneticileri yönetin. Tipik bir örnekte, çok sayıda veritabanını yönetmek için "parçalar" bilinen, vardır. Müşteriler birlikte aynı veritabanında bulunan ve (tek kiracılı düzeni) müşteri başına bir veritabanı yoktur. İstemci Kitaplığı, bu özellikleri içerir:

- **Parça eşleme Yönetimi**: "Parça eşleme Yöneticisi" adlı özel bir veritabanı oluşturulur. Parça eşleme Yönetimi meta verilerini, parçalar ilgili yönetmek bir uygulama için yeteneğidir. Geliştiriciler, parçalar olarak veritabanları kaydetme, tek tek parçalama anahtarları veya bu veritabanlarını anahtar aralıklarına eşlemelerini tanımlamak ve bu meta veriler bir sayı olarak korumak için bu işlevi kullanabilmeniz için ve kapasite değişiklikleri yansıtacak şekilde veritabanı oluşturma geliştikçe. Elastik veritabanı istemci Kitaplığı ' çok parçalama uygularken yönetim kod yazmaya zaman ayırın gerekecektir. Ayrıntılar için bkz [parça eşleme Yönetimi](sql-database-elastic-scale-shard-map-management.md).

- **Verilere bağımlı yönlendirme**: Uygulamaya gelen bir istek düşünün. İsteğin parçalama anahtarı değerine göre uygulamanın doğru veritabanına anahtar değerine dayalı belirlemesi gerekir. Ardından, isteği işlemek için veritabanına bir bağlantı açar. Verilere bağımlı yönlendirme uygulamanın parça eşlemesi içinde tek bir kolayca çağrısıyla bağlantıları olanağı sağlar. Verilere bağımlı yönlendirme artık elastik veritabanı istemci kitaplığı işlevleri tarafından kapsanan altyapı kodunu başka bir alanı oluştu. Ayrıntılar için bkz [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md).
- **Çok parçalı sorgular (MSQ)** : Bir istek çeşitli (veya tüm) parçalar gerektirdiğinde çok parçalı sorgulama çalışır. Çok parçalı sorgu, tüm parçalar veya parçalar kümesi üzerinde aynı T-SQL kodu yürütür. Katılımcı parçalar sonuçlardan UNION ALL semantiği kullanarak genel bir sonuç birleştirilir. İstemci kitaplığı kullanıma sunulan işlevleri dahil birçok görevi işleme: bağlantı yönetimi, iş parçacığı yönetimi, hata işleme ve işleme Ara sonuçlar. En fazla parça yüzlerce MSQ sorgulayabilirsiniz. Ayrıntılar için bkz [çok parçalı sorgulama](sql-database-elastic-scale-multishard-querying.md).

Genel olarak, esnek veritabanı araçlarını kullanan müşteriler tüm T-SQL yerel parça işlemleri kendi semantiklere sahip parçalar arası işlemleri aksine gönderirken işlevlerinden bekleyebilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

- Elastik veritabanı istemci kitaplığı ([Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22), [.NET](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)) - **indirme** kitaplığı.

- [Esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md) - denemek **örnek uygulaması** olan istemci işlevlerini gösterir.

- GitHub ([Java](https://github.com/Microsoft/elastic-db-tools-for-java/blob/master/README.md), [.NET](https://github.com/Azure/elastic-db-tools)) - katkı sağlamak için kod.
- [Azure SQL veritabanı esnek sorgu genel bakış](sql-database-elastic-query-overview.md) - esnek sorguları kullanmak için.

- [Ölçeği genişletilen bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md) - kullanma yönergeleri için **bölme-birleştirme aracını**.



<!-- Additional resources H2 -->

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Anchors-->
<!--Image references-->

[1]: ./media/sql-database-elastic-database-client-library/glossary.png

