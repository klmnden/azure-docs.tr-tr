---
title: "Ölçeklenebilir bulut veritabanları oluşturma | Microsoft Docs"
description: "Esnek veritabanı istemci kitaplığı ile ölçeklenebilir .NET veritabanı uygulamalar oluşturma"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 1f11c52d-13c1-4994-b9b1-5b1ae2f9255f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: a8bfbf2e30f8568212ab49ba1772588cc9e45b38
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="building-scalable-cloud-databases"></a>Ölçeklenebilir bulut veritabanları oluşturma
Veritabanları ölçeklendirme Azure SQL veritabanı için ölçeklenebilir araçları ve özelliklerinin kullanılarak kolayca gerçekleştirilebilir. Özellikle, kullanabileceğiniz **esnek veritabanı istemci Kitaplığı** oluşturmak ve ölçeklendirilmiş veritabanları yönetmek için. Bu özellik kolayca yüzlerce kullanarak parçalı uygulamalar geliştirmenize olanak sağlar; veya hatta binlerce — Azure SQL veritabanlarının. [Esnek iş](sql-database-elastic-jobs-powershell.md) daha sonra bu veritabanlarının kolaylığı yönetim yardımcı olmak için kullanılabilir.

Kitaplığı yüklemek için Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="documentation"></a>Belgeler
1. [Esnek Veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md)
2. [Esnek veritabanı özellikleri](sql-database-elastic-scale-introduction.md)
3. [Parça eşleme yönetimi](sql-database-elastic-scale-shard-map-management.md)
4. [Ölçeği genişletilen mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)
6. [Çok parça sorguları](sql-database-elastic-scale-multishard-querying.md)
7. [Esnek veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md)
8. [Çok kiracılı uygulamalarla esnek veritabanı araçlarını ve satır düzeyi güvenlik](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [İstemci Kitaplığı uygulamaları yükseltme](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Esnek sorgular genel bakış](sql-database-elastic-query-overview.md)
11. [Esnek veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md)
12. [Entity Framework ile esnek veritabanı istemci kitaplığı](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Dapper ile esnek veritabanı istemci kitaplığı](sql-database-elastic-scale-working-with-dapper.md)
14. [Bölünmüş Birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Parça eşleme yöneticisi için performans sayaçları](sql-database-elastic-database-client-library.md) 
16. [Esnek veritabanı araçlarını hakkında SSS](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>İstemci özellikleri
Ölçeklendirme kullanarak uygulamaları *parçalama* zorluklar hem Geliştirici yanı sıra yönetici gösterir. Her iki geliştiriciler olanak sağlayan araçlar vererek istemci kitaplığı yönetim görevlerini basitleştirir ve yöneticiler ölçeklendirilmiş veritabanları yönetin. Tipik bir örnekte yönetmek için "parça," bilinen birçok veritabanı vardır. Müşteriler birlikte aynı veritabanında bulunan ve müşteri (tek Kiracı düzeni) başına tek bir veritabanı yok. İstemci Kitaplığı, şu özellikleri içerir:

- **Parça eşleme Yönetim**: "parça eşleme Yöneticisi" adlı özel bir veritabanı oluşturulur. Parça eşleme yönetim kendi parça hakkındaki meta verileri yönetmek bir uygulama için özelliğidir. Geliştiriciler veritabanları parça kaydetme, tek tek parçalama anahtarları ve bu veritabanları anahtar aralıklarına eşlemeleri açıklar ve bu meta verileri sayı olarak korumak için bu işlevi kullanabilirsiniz ve kapasite değişiklikleri yansıtacak şekilde veritabanlarının birleşim dönüşmesi. Esnek veritabanı istemci kitaplığı çok parçalama uygularken yönetim kod yazma zaman kullanması gerekir. Ayrıntılar için bkz [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md).

- **Veri bağımlı yönlendirme**: uygulamasına gelen bir istek düşünün. İstek parçalama anahtar değerine bağlı olarak, anahtar değere göre doğru veritabanı belirlemek uygulamanın gerekir. Ardından, isteği işlemek için veritabanına bir bağlantı açar. Veri bağımlı yönlendirme uygulama parça haritasını tek kolay çağrısını ile bağlantıları olanağı sağlar. Veri bağımlı yönlendirme şimdi esnek veritabanı istemci Kitaplığı'nda işlevi tarafından kapsanan altyapı kodu başka bir alanı oluştu. Ayrıntılar için bkz [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md).
- **Çok parça sorgular (MSQ)**: çok parça sorgulama bir istek çeşitli (veya tüm) parça gerektirdiğinde çalışır. Bir çok parça sorgusunu aynı T-SQL kodunu tüm parça veya parça kümesi çalıştırır. Katılımcı parça sonuçlarından UNION ALL semantiği kullanılarak ayarlanan bir genel sonuç birleştirilir. İstemci Kitaplığı aracılığıyla kullanıma sunulan işlevleri dahil birçok görevi işler: bağlantı yönetimi, iş parçacığı yönetimi, hata işleme ve Ara sonuçların işleniyor. MSQ kadar parça yüzlerce sorgulayabilirsiniz. Ayrıntılar için bkz [çok parça sorgulama](sql-database-elastic-scale-multishard-querying.md).

Genel olarak, esnek veritabanı araçlarını kullanan müşteriler, kendi semantiklerine sahip arası parça işlemleri aksine parça yerel işlemleri gönderirken tam T-SQL işlevselliğini kazanması bekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Deneyin [örnek uygulaması](sql-database-elastic-scale-get-started.md) istemci işlevleri gösterir. 

Kitaplığı yüklemek için Git [esnek veritabanı istemci Kitaplığı](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Bölünmüş birleştirme aracını kullanma ile ilgili yönergeler için bkz: [bölünmüş birleştirme aracına genel bakış](sql-database-elastic-scale-overview-split-and-merge.md).

[Esnek veritabanı istemci kitaplığı olan şimdi açık kaynaklı!](https://azure.microsoft.com/blog/elastic-database-client-library-is-now-open-sourced/)

Kullanım [esnek sorgular](sql-database-elastic-query-overview.md).

Kitaplık, açık kaynak yazılımının olarak kullanılabilir [GitHub](https://github.com/Azure/elastic-db-tools). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-database-client-library/glossary.png

