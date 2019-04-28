---
title: Okur ve Azure Cosmos DB'de Yazar maliyetini en iyi duruma getirme
description: Bu makalede açıklanır okuma yapılırken Azure Cosmos DB maliyetleri azaltmak ve yazma işlemleri verileri nasıl açıklar.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.openlocfilehash: b6c5722d5e096300f76f60dfaf8bab1e07d0c61c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61330212"
---
# <a name="optimize-reads-and-writes-cost-in-azure-cosmos-db"></a>Okuma ve yazma işlemleri Azure Cosmos DB'de maliyetini en iyi duruma getirme

Bu makalede, Azure Cosmos DB'den verileri yazmak ve okumak için gereken maliyeti nasıl hesaplanır açıklanır. Okuma işlemleri öğelerinde get işlemleri ve yazma işlemlerini ekleme, Değiştir, silme ve upsert öğeleri içerir.  

## <a name="cost-of-reads-and-writes"></a>Okuma ve yazma işlemleri maliyeti

Azure Cosmos DB, sağlanan aktarım hızı modeli kullanarak tahmin edilebilir performans açısından aktarım hızı ve gecikme süresi garanti eder. Sağlanan aktarım hızı açısından temsil edilen [istek birimi](request-units.md) ikinci veya RU/sn başına. İstek Birimi'ni (RU) CPU, GÇ, bellek gibi kaynakların mantıksal bir soyutlama bittikten bir isteği gerçekleştirmek için gerekli olan vb. Sağlanan aktarım hızına (RU) bir kenara bırakmanızı ve kapsayıcı ya da veritabanı, öngörülebilir üretilen iş hacmi ve gecikme süresi sağlamak için ayrılmış. Sağlanan aktarım hızı, düşük gecikme süresi ve yüksek kullanılabilirlik herhangi bir ölçekte garantili, öngörülebilir ve tutarlı performans sağlamak Azure Cosmos DB sağlar. İstek birimleri hakkında ne kadar kaynak uygulamanın gerek duyduğu mantık basitleştirir normalleştirilmiş bir para birimi temsil eder. 

İstek birimleri okuma ve yazma işlemleri arasında ayrım yapma hakkında düşünmek zorunda değilsiniz. İstek birimleri birleşik para birimi modelini birbirinin aynı işleme kapasitesi hem okuma hem de yazma işlemleri için kullanılacak verimliliği oluşturur. Aşağıdaki tabloda, okuma maliyetini gösterir ve bakımından RU/sn, 1 KB ve 100 KB boyutundaki öğeleri için yazar.

|**Öğe boyutu**  |**Bir okuma maliyeti** |**Bir yazma maliyeti**|
|---------|---------|---------|
|1 KB |1 RU |5 ru |
|100 KB |10 RU |50 ru |

Bir maliyetleri RU, boyutu 1 KB'a olan bir öğeyi okunuyor. 1 KB'lık bir öğenin yazma beş RU maliyeti. Okuma ve yazma maliyetleri varsayılan oturum kullanılırken geçerli olan [tutarlılık düzeyi](consistency-levels.md).  RUs etrafında konuları içerir: öğe boyutu, özellik sayısı, veri tutarlılığı, Dizinli Özellikler, dizin oluşturma ve sorgu desenleri.

## <a name="normalized-cost-for-1-million-reads-and-writes"></a>Normalleştirilmiş maliyetini 1 milyon okuma ve yazma

Sağlama 1.000 RU/sn, 3.6 milyon RU/saat çevirir ve $0.08 saat (ABD ve Avrupa içinde) maliyetlidir. Bir 1 KB'lik öğe için 3.6 milyon okuma ve 0,72 milyon yazma gerçekleştirebilirsiniz (Bu değer olarak hesaplandı: `3.6 million RU / 5`) bu sağlanan aktarım hızı ile saat başına. Bir milyon okuma ve yazma işlemleri için normalleştirilmiş, 1 milyon okuma için maliyeti $0.022 olur (Bu değer olarak hesaplanır: $0.08/3.6 milyon) ve 0.111 1 milyon yazma işlemleri için (Bu değer olarak hesaplandı: $0.08/0.72 milyon).

## <a name="number-of-regions-and-the-request-units-cost"></a>Bölgeler ve maliyet istek birimi sayısı

Azure Cosmos hesabıyla ilişkili bölge sayısı ne olursa olsun yazma maliyetine sabittir. Diğer bir deyişle, 1 KB yazma beş RU hesabıyla ilişkili bölge sayısı bağımsız maliyetlidir. Önemsiz olmayan bir çoğaltma, kabul etme ve her bölge çoğaltma trafiğini işlemek için harcanan kaynaklarının miktarını yoktur. Çok bölgeli maliyet iyileştirme hakkında daha fazla ayrıntı için bkz: [çok bölgeli Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md) makalesi.

## <a name="optimize-the-cost-of-writes-and-reads"></a>Yazma ve okuma maliyetini en iyi duruma getirme

Yazma işlemleri gerçekleştirirken, saniye başına gerekli yazma sayısı desteklemek üzere yeterli kapasitesi sağlamanız gerekir. SDK'yı kullanarak, sağlanan aktarım hızına artırabilirsiniz portal, yazma işlemleri gerçekleştirmeden önce CLI ve yazma tamamlandıktan sonra ardından aktarım azaltabilirsiniz. Yazma dönemi için aktarım hızınızı belirli verileri için gereken en düşük aktarım hızı yanı sıra, diğer iş yükleri çalıştıran iş yükü INSERT varsayılarak için gereken aktarım hızına ' dir. 

Örneğin, sorgu/okuma/güncelleştirme/silme, diğer iş yükleri eşzamanlı olarak çalıştırıyorsanız, bu işlemler için çok gerekli ek istek birimi eklemelisiniz. Yazma işlemleri oranı sınırlı ise, Azure Cosmos DB SDK'ları kullanarak yeniden deneme/geri alma İlkesi özelleştirebilirsiniz. Örneğin, küçük bir istekleri oranını oranı sınırlı alır kadar yük artırabilirsiniz. Hız sınırı meydana gelirse, istemci uygulaması hız sınırlama yedeklemelisiniz belirtilen yeniden deneme aralığı için istek. Yazma işlemlerini yeniden denemeden önce en düşük düzeyde yeniden denemeler arasındaki zaman aralığı olmalıdır. Yeniden deneme ilkesi desteği, SQL .NET, Java, Node.js ve Python SDK'ları ve .NET Core SDK'ları tüm desteklenen sürümleri dahildir. 

Ayrıca Azure Cosmos DB'ye toplu ekleme verileri veya veri kopyalama herhangi bir desteklenen kaynak veri deposundan Azure Cosmos DB'ye kullanarak [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md). Azure Data Factory, Azure Cosmos DB toplu veri yazdığınızda, en iyi performansı sağlamak için API'si ile yerel olarak tümleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Azure Cosmos DB'de maliyet iyileştirmesi hakkında daha fazla bilgi edinmek için sonraki geçebilirsiniz:

* Daha fazla bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [Azure Cosmos DB faturanızı anlama](understand-your-bill.md)
* Daha fazla bilgi edinin [aktarım hızı maliyeti en iyi duruma getirme](optimize-cost-throughput.md)
* Daha fazla bilgi edinin [depolama maliyetini en iyi duruma getirme](optimize-cost-storage.md)
* Daha fazla bilgi edinin [sorguları maliyetini en iyi duruma getirme](optimize-cost-queries.md)
* Daha fazla bilgi edinin [çok bölgeli Azure Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md)
