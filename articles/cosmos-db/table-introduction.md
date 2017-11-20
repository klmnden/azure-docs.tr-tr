---
title: "Azure Cosmos DB Tablo API'sine Giriş | Microsoft Docs"
description: "Azure Cosmos DB ile popüler OSS MongoDB API'lerini kullanarak çok büyük hacimlerdeki anahtar-değer verilerini düşük gecikme süreleriyle nasıl depolayabileceğinizi ve sorgulayabileceğinizi öğrenin."
services: cosmos-db
author: mimig
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/15/2017
ms.author: mimig1
ms.openlocfilehash: 8e8a5d89080df54da1a951d94fac12592de29c50
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Azure Cosmos DB Tablo API’sine Giriş

[Azure Cosmos DB](introduction.md), Azure Tablo depolaması için yazılmış olan ve aşağıdaki gibi üst düzey özelliklere ihtiyaç duyan uygulamalar için Tablo API'sini sunar:

* [Anahtar teslimi genel dağıtım](distribute-data-globally.md).
* Dünya genelinde [adanmış aktarım hızı](partition-data.md).
* 99 yüzdebirlikte tek basamaklı milisaniyelik gecikme süresi.
* Garantili yüksek kullanılabilirlik.
* [Otomatik ikincil dizin oluşturma](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Azure Tablo depolama için yazılmış uygulamalar herhangi bir kod değişikliği olmadan Tablo API'sini kullanarak Azure Cosmos DB'ye geçirilebilir ve üst düzey özelliklerden yararlanabilir. Tablo API’si, .NET için kullanılabilen bir istemci SDK'sına sahiptir.

Aravind Ramachandran'ın Azure Cosmos DB Tablo API'si ile çalışmaya başlama konulu aşağıdaki videosunu izlemenizi öneririz:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="table-offerings"></a>Tablo teklifleri
Şu anda Azure Tablo Depolama hizmetini kullanıyorsanız, Azure Cosmos DB Tablo API’sine geçerek aşağıdaki avantajlara sahip olabilirsiniz:

| | Azure Tablo depolama | Azure Cosmos DB Tablo API’si |
| --- | --- | --- |
| Gecikme süresi | Hızlıdır, ancak gecikme süresi için üst sınır yoktur. | Herhangi bir ölçekte, dünyanın her yerinde okuma ve yazma işlemleri için tek haneli milisaniyelik gecikme (99. yüzdebirlik dilimde okumalar için 10 ms'den az, yazma için 15 ms'den az gecikme süresiyle desteklenir). |
| Aktarım hızı | Değişken aktarım hızı modeli. Tabloların 20.000 işlem/sn'lik bir ölçeklenebilirlik sınırı vardır. | SLA'lar ile desteklenen [tablo başına adanmış, ayrılmış aktarım hızı](request-units.md) ile yüksek düzeyde ölçeklenebilir. Hesapların aktarım hızı açısından üst sınırı yoktur ve tablo başına saniyede 10 milyondan fazla işlem desteklenir. |
| Genel dağıtım | Yüksek kullanılabilirlik için isteğe bağlı okunabilir bir ikincil okuma bölgesi olan tek bölge. Yük devretme başlatamazsınız. | 30'dan fazla bölgenin birinden [anahtar teslimi genel dağıtım](distribute-data-globally.md). Her zaman, dünyanın her yerinde [otomatik ve el ile yük devretme](regional-failover.md) desteği. |
| Dizinleme | Yalnızca PartitionKey ve RowKey’de birincil dizin. İkincil dizin yok. | Tüm özelliklerde otomatik ve eksiksiz dizin oluşturma, dizin yönetimi yok. |
| Sorgu | Sorgu yürütme birincil anahtar için dizini kullanır, aksi durumda tarar. | Sorgular, hızlı sorgu süreleri için özelliklerde otomatik dizin oluşturma avantajından yararlanabilir. Azure Cosmos DB'nin veritabanı altyapısı, toplamaları, jeo-uzamsal aramayı ve sıralamayı destekleme özelliğine sahiptir. |
| Tutarlılık | Birincil bölge içinde güçlü. İkincil bölge içinde nihai. | Uygulama gereksinimlerinize bağlı olarak kullanılabilirlik, gecikme süresi, aktarım hızı ve tutarlılık arasında denge sağlamak için [iyi tanımlanmış beş tutarlılık düzeyi](consistency-levels.md). |
| Fiyatlandırma | Depolama açısından iyileştirilmiş. | Aktarım hızı açısından iyileştirilmiş. |
| SLA’lar | %99,99 kullanılabilirlik. | Rahat bir tutarlılıkla tek tek tüm bölge hesapları ve çok bölgeli tüm hesaplar için %99,99 kullanılabilirlik SLA'sı ve çok bölgeli tüm veritabanı hesaplarında %99,999 okunabilirlik Genel kullanıma sunulma aşamasında [endüstri lideri kapsamlı SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/). |

## <a name="get-started"></a>başlarken

[Azure portalındaki](https://portal.azure.com) yeni Azure Cosmos DB hesabı. Ardından [.NET kullanarak Tablo API'sı için hızlı başlangıç](create-table-dotnet.md) makalemizi inceleyin. 

## <a name="next-steps"></a>Sonraki adımlar

İşte başlamanıza yardımcı olacak birkaç ipucu:
* [Tablo API'sini kullanarak bir .NET uygulaması derleme](create-table-dotnet.md)
* [.NET’te Tablo API’siyle geliştirme](tutorial-develop-table-dotnet.md)
* [Tablo API’sini kullanarak tablo verilerini sorgulama](tutorial-query-table.md)
* [Tablo API'sini kullanarak Azure Cosmos DB genel dağıtımını ayarlamayı öğrenin](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Tablosu .NET API'si](table-sdk-dotnet.md)
* [Azure Cosmos DB Tablosu Java API'si](table-sdk-java.md)
* [Azure Cosmos DB Tablosu Node.js API'si](table-sdk-nodejs.md)
* [Python için Azure Cosmos DB Tablosu SDK'sı](table-sdk-python.md)

