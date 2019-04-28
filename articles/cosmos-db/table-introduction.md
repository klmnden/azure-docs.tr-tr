---
title: Azure Cosmos DB tablo API'sine giriş
description: Depolamak için Azure Cosmos DB'yi nasıl kullanabileceğinizi ve sorgu çok büyük hacimlerdeki anahtar-değer verilerini düşük gecikme süresi ile Azure tablo API'sini kullanarak öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 11/20/2017
ms.author: sngun
ms.openlocfilehash: 68190ad15ed70ac831c21582d60bc54da5d3c14b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60913489"
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Azure Cosmos DB’ye giriş: Tablo API’si

[Azure Cosmos DB](introduction.md), Azure Tablo depolaması için yazılmış olan ve aşağıdaki gibi üst düzey özelliklere ihtiyaç duyan uygulamalar için Tablo API'sini sunar:

* [Anahtar teslimi genel dağıtım](distribute-data-globally.md).
* Dünya genelinde [adanmış aktarım hızı](partition-data.md).
* 99 yüzdebirlikte tek basamaklı milisaniyelik gecikme süresi.
* Garantili yüksek kullanılabilirlik.
* [Otomatik ikincil dizin oluşturma](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Azure Tablo depolama için yazılmış uygulamalar herhangi bir kod değişikliği olmadan Tablo API'sini kullanarak Azure Cosmos DB'ye geçirilebilir ve üst düzey özelliklerden yararlanabilir. Tablo API’si, .NET, Java, Python ve Node.js ile kullanılabilecek istemci SDK’larına sahiptir.

## <a name="table-offerings"></a>Tablo teklifleri
Şu anda Azure Tablo Depolama hizmetini kullanıyorsanız, Azure Cosmos DB Tablo API’sine geçerek aşağıdaki avantajlara sahip olabilirsiniz:

| | Azure Tablo depolama | Azure Cosmos DB Tablo API’si |
| --- | --- | --- |
| Gecikme süresi | Hızlıdır, ancak gecikme süresi için üst sınır yoktur. | Herhangi bir ölçekte, dünyanın her yerinde okuma ve yazma işlemleri için tek haneli milisaniyelik gecikme (99. yüzdebirlik dilimde okumalar için 10 ms'den az, yazma için 15 ms'den az gecikme süresiyle desteklenir). |
| Aktarım hızı | Değişken aktarım hızı modeli. Tabloların 20.000 işlem/sn'lik bir ölçeklenebilirlik sınırı vardır. | SLA'lar ile desteklenen [tablo başına adanmış, ayrılmış aktarım hızı](request-units.md) ile yüksek düzeyde ölçeklenebilir. Hesapların aktarım hızı açısından üst sınırı yoktur ve tablo başına saniyede 10 milyondan fazla işlem desteklenir. |
| Genel dağıtım | Yüksek kullanılabilirlik için isteğe bağlı okunabilir bir ikincil okuma bölgesi olan tek bölge. Yük devretme başlatamazsınız. | 30'dan fazla bölgenin birinden [anahtar teslimi genel dağıtım](distribute-data-globally.md). Her zaman, dünyanın her yerinde [otomatik ve el ile yük devretme](high-availability.md) desteği. |
| Dizinleme | Yalnızca PartitionKey ve RowKey’de birincil dizin. İkincil dizin yok. | Tüm özelliklerde otomatik ve eksiksiz dizin oluşturma, dizin yönetimi yok. |
| Sorgu | Sorgu yürütme birincil anahtar için dizini kullanır, aksi durumda tarar. | Sorgular, hızlı sorgu süreleri için özelliklerde otomatik dizin oluşturma avantajından yararlanabilir. |
| Tutarlılık | Birincil bölge içinde güçlü. İkincil bölge içinde nihai. | Uygulama gereksinimlerinize bağlı olarak kullanılabilirlik, gecikme süresi, aktarım hızı ve tutarlılık arasında denge sağlamak için [iyi tanımlanmış beş tutarlılık düzeyi](consistency-levels.md). |
| Fiyatlandırma | Depolama açısından iyileştirilmiş. | Aktarım hızı açısından iyileştirilmiş. |
| SLA’lar | %99,99 kullanılabilirlik. | Rahat bir tutarlılıkla tek tek tüm bölge hesapları ve çok bölgeli tüm hesaplar için %99,99 kullanılabilirlik SLA'sı ve çok bölgeli tüm veritabanı hesaplarında %99,999 okunabilirlik Genel kullanıma sunulma aşamasında [endüstri lideri kapsamlı SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/). |

## <a name="get-started"></a>başlarken

[Azure portalındaki](https://portal.azure.com) yeni Azure Cosmos DB hesabı. Ardından [.NET kullanarak Tablo API'sı için hızlı başlangıç](create-table-dotnet.md) makalemizi inceleyin. 

> [!IMPORTANT]
> Önizleme sırasında bir Tablo API hesabı oluşturduysanız, genel kullanıma açık Tablo API SDK’ları ile çalışmak için lütfen [yeni Tablo API hesabı](create-table-dotnet.md#create-a-database-account) oluşturun.
>

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

