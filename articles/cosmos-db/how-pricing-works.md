---
title: Azure Cosmos DB'nin fiyatlandırma modeli
description: Bu makalede, Azure Cosmos DB ile maliyet yönetimi ve maliyet planlama nasıl kolaylaştırdığını fiyatlandırma modelini açıklar.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: c9e7afdb8927a2cdada8ae86ebf18a42327e640c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65968940"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Azure Cosmos DB'de fiyatlandırma modeli 

Azure Cosmos DB fiyatlandırma modelini maliyeti basitleştirir yönetimi ve planlama. Azure Cosmos DB'de sağlanan aktarım hızı ve tükettiğiniz depolama için ödeme yaparsınız.

* **Sağlanan aktarım hızı**: Sağlanan aktarım hızı (ayrılmış aktarım hızı olarak da bilinir) her ölçekte yüksek performanslı garanti eder. Gereksinim duyduğunuz ve Azure Cosmos DB, yapılandırılmış aktarım hızı sağlamak için gereken kaynakları ayıran işleme (RU/s) belirtirsiniz. Belirli bir saat için en fazla sağlanan aktarım hızı için faturalandırılır.

   > [!NOTE]
   > Sağlanan aktarım hızı modeli, kapsayıcı veya veritabanı kaynakları ayıran olduğundan, herhangi bir iş yükünü çalışmıyor olsa bile sağlanan aktarım hızı için ücretlendirilirsiniz.

* **Tüketilen depolama**: Faturalandırılırsınız veri ve dizinler için belirli bir saat için kullanılan depolama (GB) toplam miktarı için bir sabit ücretle.

Sağlanan aktarım hızı, belirtilen [istek birimi](request-units.md) saniye başına (RU/sn), kapsayıcıları veya veritabanlarına veri yazma veya Okuma olanak tanır. Yapabilecekleriniz [sağlama aktarım hızı bir veritabanı veya bir kapsayıcı](set-throughput.md). İş yükü gereksinimlerinize bağlı olarak, aktarım hızının yukarı/aşağı dilediğiniz zaman artırabilirsiniz. Azure Cosmos DB fiyatlandırma, elastik olan ve bir veritabanı veya bir kapsayıcı yapılandırmanızı aktarım hızı orantılıdır. En düşük aktarım hızı ve depolama değerleri ve ölçeği artırır, fiyat elastikliği spektrumun karşılaştırması çeşitli müşteriler, tüm parçalarını küçük ölçekli büyük ölçekli kapsayıcıları için gelen sağlar. Her bir veritabanı veya bir kapsayıcı, depolama miktarı GB ve 100 RU/sn, birimi en az 400 RU/sn, sağlanan aktarım hızı için saatlik olarak faturalandırılır. Sağlanan aktarım hızı depolama tüketim temelinde faturalandırılır. Diğer bir deyişle, herhangi bir depolama alanı önceden ayırmanıza gerek yoktur. Yalnızca tükettiğiniz depolama için faturalandırılırsınız.

Daha fazla bilgi için [Fiyatlandırma sayfasında Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) ve [Azure Cosmos DB faturanızı anlama](understand-your-bill.md).

Azure Cosmos DB fiyatlandırma modelinde tüm API'leri arasında tutarlıdır. Daha fazla bilgi için bkz. [nasıl Azure Cosmos fiyatlandırma modeli DB, müşterilerin uygun maliyetli](total-cost-ownership.md). Bir veritabanı veya bir kapsayıcı SLA'lar emin olmak için gereken en az bir aktarım hızı yoktur ve artırmak ya da sağlanan aktarım hızı, her 100 RU/sn için 6 ile azaltın.

Şu anda hem veritabanı hem de kapsayıcı tabanlı işleme için en düşük fiyat aylık $24 olur (bkz [Fiyatlandırma sayfasında Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) en son bilgi. İş yükünüz birden çok kapsayıcı kullanıyorsa, bu maliyet için veritabanı düzeyi aktarım hızını kapsayıcılar arasında aktarım hızı paylaşımı veritabanındaki herhangi bir sayıda kapsayıcı olması izin verdiğinden, veritabanı düzeyinde aktarım hızını kullanarak iyileştirilebilir. Sağlanan aktarım hızı ve maliyetleri farklı varlıklar için aşağıdaki tabloda özetlenmiştir:

|**Varlık**  | **En düşük aktarım hızı ve maliyet** |**Ölçeği artırır ve maliyet** |**Kapsam sağlama** |
|---------|---------|---------|-------|
|Database    | 400 RU/sn ($24/ ay)    | 100 RU/sn (6 ABD Doları/ay)   |Aktarım hızı veritabanı için ayrılmıştır ve veritabanı kapsayıcılara tarafından paylaşılır |
|Kapsayıcı     | 400 RU/sn ($24/ ay)    | 100 RU/sn (6 ABD Doları/ay)  |Aktarım hızı belirli bir kapsayıcı için ayrılmıştır |

Önceki tabloda gösterildiği gibi Azure Cosmos DB'de en düşük aktarım hızı $24 aylık bir ücretten başlatır. En düşük aktarım hızı ve ölçek ile üretim iş yüklerini desteklemek için zaman içinde başlattığınızda, maliyetlerinizi düzgün 6 ABD Doları/ay artışlarla artış. Azure Cosmos DB'de fiyatlandırma modeli esnektir ve ölçek büyütme veya küçültme gibi bir kesintisiz artışa veya fiyata bulunur.

## <a name="try-azure-cosmos-db-for-free"></a>Azure Cosmos DB’yi ücretsiz deneyin 

Azure Cosmos DB, ücretsiz, geliştiricilere çeşitli seçenekler sunar. Bu seçenekler şunlardır:

* **Ücretsiz Azure hesabı**: Azure tekliflerini bir [ücretsiz katmanı](https://azure.microsoft.com/free/) , 200 ABD Doları değerinde Azure kredisine sahip olun, ilk 30 gün ve sınırlı bir miktar 12 ay boyunca Ücretsiz Hizmetler için sağlar. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](../billing/billing-avoid-charges-free-account.md). Azure Cosmos DB, Azure ücretsiz hesabı bir parçasıdır. Azure Cosmos DB için özellikle, bu ücretsiz hesaba yılın tamamı için 5 GB'lık depolama ve sağlanan aktarım hızı 400 Ru'ya sunar. 

* **Azure Cosmos DB'yi ücretsiz deneyin**: Azure Cosmos DB sunar zaman sınırlaması deneyimini kullanarak Azure Cosmos DB için ücretsiz hesaplar deneyin. Bir Azure Cosmos DB hesabı oluşturma, veritabanı ve koleksiyon oluşturma ve hızlı Başlangıçlar ve öğreticilerle kullanarak bir örnek uygulamayı çalıştırın. Bir Azure hesabına abone olma ya da kredi kartınızı kullanarak olmadan, örnek uygulamayı çalıştırabilirsiniz. [Azure Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) bir ay boyunca Azure Cosmos DB herhangi sayıda hesabınızı yenileme olanağı sunar.

* **Azure Cosmos DB öykünücüsü'nü**: Azure Cosmos DB öykünücüsü'nü geliştirme amacıyla Azure Cosmos DB hizmetine öykünür yerel bir ortam sağlar. Öykünücü, hiçbir ücret ödemeden ve bulut hizmeti yüksek doğrulukla sunulur. Azure Cosmos DB öykünücüsü'nü kullanarak geliştirme ve bir Azure aboneliği oluşturmadan veya masraf yapmadan uygulamalarınızı yerel olarak test etmek. Uygulamalarınızı öykünücüsü kullanarak yerel olarak üretime geçmeden önce geliştirebilirsiniz. Öykünücü karşı uygulamanın işlevselliğini memnun kaldığınızda, bulutta Azure Cosmos DB hesabı kullanmaya geçmek ve önemli ölçüde maliyet tasarrufu. Öykünücü hakkında daha fazla bilgi için bkz: [geliştirme ve test için Azure Cosmos DB kullanarak](local-emulator.md) makale daha fazla ayrıntı için.

## <a name="pricing-with-reserved-capacity"></a>İle ayrılmış kapasite fiyatlandırması

Azure Cosmos DB [ayrılan kapasite](cosmos-db-reserved-capacity.md) para'ı kaydetmek için Azure Cosmos DB kaynaklarını bir yıl veya üç yıl için önceden ödeme yaparak yardımcı olur. Önemli ölçüde, bir yıllık veya üç yıllık ön taahhüt ile maliyetlerinizi azaltın ve normal fiyatlandırma karşılaştırıldığında 20 %65 indirimleri arasında kaydedin. Azure Cosmos DB ayrılmış kapasite yardımcı sağlanan aktarım hızını (RU/s) için bir yıl veya üç yıllık bir dönem için önceden ödeme yaparak maliyetleri düşürmek ve sağlanan aktarım hızı bir indirim kazanın. 

Ayrılmış kapasite bir fatura oranında indirim sağlar ve Azure Cosmos DB kaynaklarınızın çalışma zamanı durumunu etkilemez. Ayrılmış kapasite MongoDB, Cassandra, SQL, Gremlin ve Azure tabloları ve dünya genelindeki tüm bölgelerde içeren tüm API'leri için sürekli olarak kullanılabilir. Ayrılmış kapasite hakkında daha fazla bilgi [ayrılmış kapasite ile Azure Cosmos DB kaynaklarını için ön ödeme](cosmos-db-reserved-capacity.md) makalesi ve ayrılmış kapasite satın [Azure portalında](https://portal.azure.com/).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde, Azure Cosmos DB kaynaklarını maliyetlerini en iyi duruma getirme hakkında daha fazla bilgi edinebilirsiniz:

* Hakkında bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [Azure Cosmos DB faturanızı anlama](understand-your-bill.md)
* Daha fazla bilgi edinin [aktarım hızı maliyeti en iyi duruma getirme](optimize-cost-throughput.md)
* Daha fazla bilgi edinin [depolama maliyetini en iyi duruma getirme](optimize-cost-storage.md)
* Daha fazla bilgi edinin [okuma ve yazma işlemleri maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md)
* Daha fazla bilgi edinin [sorguları maliyetini en iyi duruma getirme](optimize-cost-queries.md)
* Daha fazla bilgi edinin [çok bölgeli Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md)
* Hakkında bilgi edinin [Azure Cosmos DB ayrılan kapasite](cosmos-db-reserved-capacity.md)
* Hakkında bilgi edinin [Azure Cosmos DB öykünücüsü'nü](local-emulator.md)
