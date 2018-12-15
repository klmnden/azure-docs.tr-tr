---
title: "Azure Cosmos DB'ye giriş: MongoDB API’si"
description: Azure Cosmos DB ile popüler OSS MongoDB API'lerini kullanarak çok büyük hacimlerdeki JSON belgelerini düşük gecikme süreleriyle nasıl depolayabileceğinizi ve sorgulayabileceğinizi öğrenin.
keywords: MongoDB nedir
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: overview
ms.date: 02/12/2018
ms.author: sclyon
ms.openlocfilehash: 0d36d4c18860f6448d98d9d67b854d91b07ea9d2
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409097"
---
# <a name="introduction-to-azure-cosmos-db-mongodb-api"></a>Azure Cosmos DB'ye giriş: MongoDB API’si

[Azure Cosmos DB](../cosmos-db/introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB, [kullanıma hazır genel dağıtım](distribute-data-globally.md), dünya çapında [aktarım hızı ve depolama için esnek ölçeklendirme](partition-data.md), 99. yüzdebirlik dilimde tek haneli milisaniyelik gecikme süreleri ve garantili yüksek kullanılabilirlik olanakları sunar ve bunların tümü [sektör lideri SLA’lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) ile desteklenir. Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler. 

![Azure Cosmos DB: MongoDB API’si](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Azure Cosmos DB veritabanları, [MongoDB](https://docs.mongodb.com/manual/introduction/) için yazılmış uygulamaların veri deposu olarak kullanılabilir. Bu işlevsellik, MongoDB için yazılmış uygulamanızın mevcut[sürücüleri](https://docs.mongodb.org/ecosystem/drivers/) kullanarak artık Azure Cosmos DB ile iletişim kurabileceği ve MongoDB veritabanları yerine Azure Cosmos DB veritabanlarını kullanabileceği anlamına gelir. Çoğu durumda, sadece bir bağlantı dizesini değiştirerek MongoDB'den Azure Cosmos DB'ye geçiş yapabilirsiniz. Bu işlevselliği kullanarak, MongoDB'nin tanıdık yöntem ve araçlarını kullanmaya devam ederken, Azure Cosmos DB ve [kapsamlı sektör lideri SLA'ları](https://azure.microsoft.com/support/legal/sla/cosmos-db) ile Azure bulutunda kolayca küresel olarak dağıtılmış MongoDB veritabanı uygulamaları geliştirebilir ve çalıştırabilirsiniz.

**MongoDB Uyumluluk**: Azure Cosmos DB MongoDB kablo protokolüne uyguladığı, var olan MongoDB uzmanlığı, uygulama kodu ve araçları kullanabilirsiniz. MongoDB kullanarak uygulamalar geliştirebilir ve bunları tamamen yönetilen, küresel olarak dağıtılmış Azure Cosmos DB hizmetini kullanarak üretime dağıtabilirsiniz. Desteklenen sürümler hakkında daha fazla bilgi için bkz. [MongoDB Protokol Desteği](mongodb-feature-support.md#mongodb-protocol-support).

Aynı MongoDB API'sini kullanan Azure Cosmos DB MongoDB API'SİNDE doğrudan uç noktası olarak Azure Stream Analytics gibi hizmetler için kullanılamaz çünkü [istemci sürücüleri](https://docs.mongodb.org/ecosystem/drivers/) yerel MongoDB olarak. Azure Stream Analytics ile tümleştirmek için kullanmayı [Azure App Service](../app-service/app-service-web-overview.md) veya [Azure işlevleri hizmet](../azure-functions/functions-overview.md) verileri Azure Cosmos DB MongoDB API'SİNDE yazmak bir ara yazılım hizmeti olarak.

## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>MongoDB uygulamaları için Azure Cosmos DB kullanmanın avantajı nedir?

**Esnek bir şekilde ölçeklenebilir aktarım hızı ve Depolama:** Uygulamaları kolayca ölçeği artırılabilen veya azaltılabilen MongoDB veritabanınızı tarafından ihtiyaçlarınıza. Verileriniz düşük tahmin edilebilirliğe sahip gecikme süreleri sağlamak için katı hal disklerinde (SSD) depolanır. Azure Cosmos DB, neredeyse sınırsız depolama boyutlarına ve sağlanmış aktarım hızına ölçeklenebilen MongoDB koleksiyonlarını destekler. Uygulamanız büyüdükçe, Azure Cosmos DB'yi tahmin edilebilir performansla sorunsuz ve esnek bir şekilde ölçeklendirebilirsiniz. 

**Çok bölgeli çoğaltma:** Azure Cosmos DB, verilerinizi tutarlılık, kullanılabilirlik ve performansın bir denge sağlarken verilere genel erişim gerektiren uygulamalar geliştirmenize imkan tanır, MongoDB hesabınızla ilişkilendirdiğiniz tüm bölgelere şeffaf biçimde çoğaltır , tüm garantili ile. Azure Cosmos DB çok girişli API’ler ile şeffaf bölgesel yük devretme olanağının yanı sıra aktarım hızını ve depolamayı dünya çapında elastik bir şekilde ölçeklendirme imkanı sağlar. [Verileri küresel olarak dağıtma](distribute-data-globally.md) bölümünde daha fazlasını öğrenin.

**Hiçbir Sunucu Yönetimi**: Yönetin ve ölçeklendirin, MongoDB veritabanları gerekmez. Azure Cosmos DB tamamen yönetilen bir hizmet olduğundan herhangi bir altyapıyı veya Sanal Makineyi yönetmeniz gerekmez. Azure Cosmos DB, 30+ [Azure Bölgesinde](https://azure.microsoft.com/regions/services/) kullanılabilir.

**İnce ayarlanabilir tutarlılık düzeyleri:** Azure Cosmos DB çok modelli API'leri desteklediğinden hesap düzeyinde tutarlılık ayarların geçerli olduğundan ve tutarlılığını zorlama her API'sı tarafından denetlenir. MongoDB'nin 3.6 sürümüne kadar oturum tutarlılığı kavramı yoktu, bu nedenle MongoDB API'si hesabını oturum tutarlılığı kullanacak şekilde ayarlarsanız, tutarlılık MongoDB API'leri kullanılırken son düzeyine düşürülür. Bir MongoDB API'si hesabı için "kendi yazmanı okuma" garantisine ihtiyacınız varsa, hesabın varsayılan tutarlılık düzeyi güçlü veya sınırlanmış eskime durumu olarak ayarlanmalıdır. Daha fazla bilgi için bkz. [Kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md).

| Azure Cosmos DB Varsayılan Tutarlılık Düzeyi |   Mongo API (3.4) |
|---|---|
|Nihai| Nihai |
|Tutarlı Ön Ek| Tutarlı sırayla son |
|Oturum| Tutarlı sırayla son |
|Sınırlanmış Eskime Durumu| Güçlü |
| Güçlü | Güçlü |

**Otomatik dizin oluşturma**: Varsayılan olarak, Azure Cosmos DB otomatik olarak dizinler MongoDB veritabanı belgelerde tüm özellikleri ve beklemez veya herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını gerektirir. Ayrıca benzersiz dizin özelliği, Azure Cosmos DB'de zaten otomatik olarak dizini oluşturulmuş belge alanlarında bir benzersizlik kısıtlamasını olanaklı kılar.

**Kurumsal sınıf**: Azure Cosmos DB yerel ve bölgesel hatalarla karşılaşıldığında % 99,99 kullanılabilirlik ve veri koruması sağlamak üzere birden çok yerel kopya destekler. Azure Cosmos DB'nin kurumsal düzeyde [uyumluluk sertifikaları](https://www.microsoft.com/trustcenter) ve güvenlik özellikleri vardır. 

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Bir Azure Cosmos DB hesabı oluşturmak ve mevcut MongoDB uygulamanızı Azure Cosmos DB kullanacak şekilde taşımak veya yeni bir tane geliştirmek için MongoDB hızlı başlangıçlarını uygulayın:

* [Mevcut Node.js MongoDB Web uygulamasını taşıma](create-mongodb-nodejs.md).
* [.NET ve Azure portalı ile bir MongoDB API'si Web uygulaması geliştirme](create-mongodb-dotnet.md)
* [Java ve Azure portalıyla bir MongoDB API'si konsol uygulaması geliştirme](create-mongodb-java.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB MongoDB API'si hakkında bilgiler, genel Azure Cosmos DB belgeleriyle tümleştirilmiştir ancak başlamanıza yardımcı olacak birkaç ipucu verebiliriz:

* Hesap bağlantı dizesi bilgilerinizi nasıl alacağınızı öğrenmek için [Bir MongoDB hesabına bağlanma](connect-mongodb-account.md) öğreticisini izleyin.
* Studio 3T'de Azure Cosmos DB veritabanınız ve MongoDB uygulamanız arasında bağlantı oluşturmayı öğrenmek için [Azure Cosmos DB ile Studio 3T (MongoChef) kullanma](mongodb-mongochef.md) öğreticisini izleyin.
* Verilerinizi bir MongoDB için API veritabanına içeri aktarmak için [MongoDB için protokol destekli Azure Cosmos DB'ye veri taşıma](mongodb-migrate.md) öğreticisini izleyin.
* [Robomongo](mongodb-robomongo.md) kullanarak bir MongoDB için API hesabına bağlanın.
* [Küresel olarak dağıtılmış uygulamalar için okuma tercihlerini yapılandırmayı](../cosmos-db/tutorial-global-distribution-mongodb.md) öğrenin.
