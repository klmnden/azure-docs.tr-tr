---
title: "Azure Cosmos DB Tablo API’sine Giriş | Microsoft Docs"
description: "Azure Cosmos DB ile popüler OSS MongoDB API’lerini kullanarak çok büyük hacimlerdeki anahtar-değer verilerini düşük gecikme süreleriyle nasıl depolayabileceğinizi ve sorgulayabileceğinizi öğrenin."
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/09/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: ef57753aeeace0086c815d83600f92422996032a
ms.contentlocale: tr-tr
ms.lasthandoff: 07/08/2017


---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Azure Cosmos DB: Tablo API’sine Giriş

[Azure Cosmos DB](introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB tarafından [kullanıma hazır genel dağıtım](distribute-data-globally.md), dünya çapında [aktarım hızı ve depolama için elastik ölçeklendirme](partition-data.md), 99. yüzdebirlik dilimde tek haneli milisaniyelik gecikme süreleri, [beş iyi tanımlanmış tutarlılık düzeyi](consistency-levels.md) ve garantili yüksek kullanılabilirlik olanakları sağlanır ve bunların tamamı [sektör lideri SLA’lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) ile desteklenir. Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler. 

![Azure Tablo depolama API’si ve Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Azure Cosmos DB esnek şemaya, tahmin edilebilir performansa, genel dağıtıma ve yüksek aktarım hızına sahip bir anahtar değeri deposuna ihtiyacı olan uygulamalar için Tablo API'sini (önizleme) sağlar. Tablo API'si, Azure Tablo depolaması ile aynı işlevleri sağlar ancak Azure Cosmos DB altyapısından da yararlanır. 

Yüksek depolama, düşük aktarım hızı gereksinimleri olan tablolar için Azure Tablo depolamayı kullanmaya devam edebilirsiniz. Azure Cosmos DB, gelecekte yapılacak bir güncelleştirme ile depolama açısından iyileştirilmiş tablolar için destek sunmaya başlayacak ve hem mevcut hem de yeni Azure Tablo depolama hesapları Azure Cosmos DB’ye yükseltilecek.

## <a name="premium-and-standard-table-apis"></a>Premium ve standart Tablo API’leri
Şu anda Azure Tablo depolamayı kullanıyorsanız, Azure Cosmos DB’nin “premium tablo” önizlemesine geçerek aşağıdaki avantajlara sahip olabilirsiniz:

|  | Azure Table Storage | Azure Cosmos DB: Tablo depolama (önizleme) |
| --- | --- | --- |
| Gecikme süresi | Hızlıdır, ancak gecikme süresi için üst sınır yoktur | Herhangi bir ölçekte, dünyanın her yerinde okuma ve yazma işlemleri için tek haneli milisaniyelik gecikme (99. yüzdebirlik dilimde okumalar için 10 ms’den az, yazma için 15 ms’den az gecikme süresiyle desteklenir) |
| Aktarım hızı | Yüksek düzeyde ölçeklenebilir, ancak adanmış aktarım hızı modeli değildir. Tabloların 20.000 işlem/sn’lik bir ölçeklenebilirlik sınırı vardır | SLA’lar ile desteklenen [tablo başına adanmış, ayrılmış aktarım hızı](request-units.md) ile yüksek düzeyde ölçeklenebilir. Hesapların aktarım hızı açısından üst sınırı yoktur ve tablo başına saniyede 10 milyondan fazla işlem desteklenir |
| Genel Dağıtım | Yüksek kullanılabilirlik için isteğe bağlı okunabilir bir ikincil okuma bölgesi olan tek bölge. Yük devretme başlatamazsınız | 30’dan fazla bölgenin birinden [kullanıma hazır genel dağıtım](distribute-data-globally.md); her zaman, dünyanın her yerinde [otomatik ve el ile yük devretme](regional-failover.md) desteği |
| Dizin Oluşturma | Yalnızca PartitionKey ve RowKey’de birincil dizin. İkincil dizin yok | Tüm özelliklerde otomatik ve eksiksiz dizin oluşturma, dizin yönetimi yok |
| Sorgu | Sorgu yürütme birincil anahtar için dizini kullanır, aksi durumda tarar. | Sorgular, hızlı sorgu süreleri için özelliklerde otomatik dizin oluşturma avantajından yararlanabilir. Azure Cosmos DB’nin veritabanı altyapısı, toplamaları, jeo-uzamsal aramayı ve sıralamayı destekleme özelliğine sahiptir. |
| Tutarlılık | Birincil bölgede Güçlü, ikincil bölgede Nihai | Uygulama gereksinimlerinize bağlı olarak kullanılabilirlik, gecikme süresi, aktarım hızı ve tutarlılık arasında denge sağlamak için [iyi tanımlanmış beş tutarlılık düzeyi](consistency-levels.md) |
| Fiyatlandırma | Depolama açısından iyileştirilmiş  | Aktarım hızı açısından iyileştirilmiş |
| SLA’lar | %99,9 kullanılabilirlik | Tek bir bölge içinde %99,99 kullanılabilirlik ve daha yüksek kullanılabilirlik için daha fazla bölge ekleme olanağı. Genel kullanılabilirlik aşamasında [sektör lideri kapsamlı SLA’lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) |

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

[Azure portalında](https://portal.azure.com) bir Azure Cosmos DB hesabı oluşturun ve [.NET kullanarak Tablo API’sine hızlı başlangıç](create-table-dotnet.md) kılavuzumuz ile çalışmaya başlayın. 

## <a name="next-steps"></a>Sonraki adımlar

İşte başlamanıza yardımcı olacak birkaç ipucu:
* Mevcut NET Tablo SDK’sını kullanarak [Azure Cosmos DB'nin Tablo API’si](create-table-dotnet.md) ile çalışmaya başlayın.
* [Azure Cosmos DB ile genel dağıtım](distribute-data-globally.md) hakkında bilgi edinin.
* [Azure Cosmos DB’de sağlanan aktarım hızı](request-units.md) hakkında bilgi edinin.

