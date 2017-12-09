---
title: "Azure Cosmos DB giriş: API MongoDB için | Microsoft Docs"
description: "Depolamak için Azure Cosmos DB nasıl kullanabileceğiniz ve sorgu yoğun birimlerini JSON belgeleri, popüler OSS MongoDB API'lerini kullanarak düşük gecikme süresi ile bilgi edinin."
keywords: MongoDB nedir
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: anhoh
ms.openlocfilehash: eca720f365a00070afd2a657829f5b108ab91fb9
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a>Azure Cosmos DB giriş: API MongoDB için

[Azure Cosmos DB](../cosmos-db/introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB tarafından [kullanıma hazır genel dağıtım](distribute-data-globally.md), dünya çapında [aktarım hızı ve depolama için elastik ölçeklendirme](partition-data.md), 99. yüzdebirlik dilimde tek haneli milisaniyelik gecikme süreleri, [beş iyi tanımlanmış tutarlılık düzeyi](consistency-levels.md) ve garantili yüksek kullanılabilirlik olanakları sağlanır ve bunların tamamı [sektör lideri SLA’lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) ile desteklenir. Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler. 

![Azure Cosmos DB: MongoDB API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Azure Cosmos DB veritabanları için yazılmış uygulamalar için veri deposu olarak kullanılabilir [MongoDB](https://docs.mongodb.com/manual/introduction/). Bu işlev, varolan kullanarak anlamına gelir [sürücüleri](https://docs.mongodb.org/ecosystem/drivers/), uygulamanızın MongoDB, artık Azure Cosmos DB ile iletişim kurmak ve Azure Cosmos DB veritabanları MongoDB veritabanı yerine kullanmak için yazılmış. Çoğu durumda, sadece bir bağlantı dizesi değiştirilerek Azure Cosmos DB'de MongoDB kullanarak geçiş yapabilirsiniz. Bu işlevini kullanarak, kolayca oluşturabilir ve çalışma MongoDB veritabanı uygulamaları Azure bulut ile Azure Cosmos veritabanı genel dağıtım ve [kapsamlı endüstri lideri SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), tanıdık becerileri kullanmaya devam ederken ve MongoDB için araçları.

**MongoDB Uyumluluk**: Azure Cosmos DB MongoDB 3.4 (sürüm 5) kablo protokolü uygulayan ve destekleyen gibi varolan MongoDB uzmanlık, uygulama kodu ve araçları kullanabilirsiniz [MongoDB toplama ardışık düzen](mongodb-feature-support.md#aggregation-pipeline). MongoDB kullanarak uygulamaları geliştirmek ve bunları tam olarak yönetilen kullanan üretim ve genel olarak dağıtılmış Azure Cosmos DB hizmeti dağıtabilirsiniz.

## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>MongoDB uygulamaları için Azure Cosmos DB kullanmanın avantajı nedir?

**Esnek bir şekilde ölçeklenebilir işleme ve Depolama:** uygulamalarınızı gerekiyor kolayca ölçeklendirme tarafından yukarı veya aşağı MongoDB veritabanınızın karşılayan. Verileriniz düşük tahmin edilebilir gecikme süreleri için katı hal diskleri (SSD) depolanır. Azure Cosmos DB neredeyse sınırsız depolama boyutlarına ve sağlanan işlemeye ölçeklenebilir MongoDB koleksiyonları destekler. Uygulamanız büyüdükçe, Azure Cosmos DB'yi tahmin edilebilir performansla sorunsuz ve esnek bir şekilde ölçeklendirebilirsiniz. 

**Bölgeli çoğaltma:** Azure Cosmos DB saydam çoğaltır, verilerinizi tüm bölgelere ilgili MongoDB hesabınızla arasında bileşim sağlarken verilere genel erişim gerektiren uygulamalar geliştirmek etkinleştirme tutarlılık, kullanılabilirlik ve performans, tüm ilgili garanti ile. Azure Cosmos DB çok girişli API’ler ile şeffaf bölgesel yük devretme olanağının yanı sıra aktarım hızını ve depolamayı dünya çapında elastik bir şekilde ölçeklendirme imkanı sağlar. Daha fazla bilgi edinin [verilerini genel dağıtmak](distribute-data-globally.md).

**Hiçbir Sunucu Yönetimi**: yönetmek ve MongoDB veritabanlarınızı ölçeklendirme gerekmez. Azure Cosmos DB herhangi bir altyapı ya da sanal makineleri kendiniz yönetmek zorunda değilsiniz demektir tam olarak yönetilen bir hizmettir. Azure Cosmos DB kullanılabilir 30 + [Azure bölgeleri](https://azure.microsoft.com/regions/services/).

**İnce ayarlanabilir tutarlılık düzeyleri:** tutarlılık ve performans arasında en iyi dengeyi elde etmek için beş iyi tanımlanmış tutarlılık düzeylerini seçin. Azure Cosmos DB sorgular ve okuma işlemleri için beş farklı tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmanıza olanak sağlar. Daha fazla bilgi için bkz. [Kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md).

**Otomatik dizin oluşturma**: varsayılan olarak, Azure Cosmos DB otomatik olarak tüm özellikleri, MongoDB belgelerde içinde veritabanı ve beklediğiniz veya herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını gerektirir dizinler. Ayrıca, bir Benzersizlik kısıtlamasını zaten Azure Cosmos DB'de otomatik dizinli tüm belge alanlarını üzerinde benzersiz dizin yeteneği sağlar.

**Kurumsal düzeyde**: Azure Cosmos DB yerel ve bölgesel hataları karşısında % 99,99 kullanılabilirlik ve veri koruması sağlamak üzere birden çok yerel çoğaltma destekler. Azure Cosmos DB sahip kurumsal düzeyde [uyumluluk sertifikalarından](https://www.microsoft.com/trustcenter) ve güvenlik özellikleri. 

Bu Azure Cuma Scott Hanselman ve Azure Cosmos DB sorumlu mühendislik Yöneticisi, Kirill Gavrylyuk ile video edinin.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Bir Azure Cosmos DB hesabı oluşturmanız ve varolan MongoDB uygulamanızı Azure Cosmos DB kullanacak şekilde geçirmek için MongoDB quickstarts izleyin veya yeni bir tane oluşturabilirsiniz:

* [Var olan bir Node.js MongoDB web uygulamaya geçirmek](create-mongodb-nodejs.md).
* [.NET ve Azure portal ile bir MongoDB API web uygulaması oluşturma](create-mongodb-dotnet.md)
* [Java ile Azure portalı ile MongoDB API konsol uygulaması oluşturma](create-mongodb-java.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos veritabanı MongoDB API'si hakkında bilgi Azure Cosmos DB belgeler genel tümleştirilmiştir, ancak başlamanıza yardımcı olmak için birkaç işaretçi şunlardır:

* İzleyin [MongoDB hesabına Bağlan](connect-mongodb-account.md) hesabı bağlantı dizesi bilgilerinizi alma hakkında bilgi için öğretici.
* İzleyin [Azure Cosmos DB ile kullanım MongoChef](mongodb-mongochef.md) öğretici Azure Cosmos DB veritabanı MongoDB uygulama arasında bir bağlantı MongoChef oluşturmayı öğrenin.
* İzleyin [protokolü ile Azure Cosmos DB'de geçirme verilerini destekleyen MongoDB için](mongodb-migrate.md) verilerinizi bir API MongoDB veritabanı için içeri aktarmak için öğretici.
* Bir API MongoDB hesabı kullanarak bağlanmak [Robomongo](mongodb-robomongo.md).
* Kaç tane RUs işletim ile kullandığınız öğrenin [GetLastRequestStatistics komut ve Azure portal ölçümleri](request-units.md#GetLastRequestStatistics).
* Bilgi edinmek için nasıl [Genel dağıtılmış uygulamalar için okuma tercihleri yapılandırmak](../cosmos-db/tutorial-global-distribution-mongodb.md).
