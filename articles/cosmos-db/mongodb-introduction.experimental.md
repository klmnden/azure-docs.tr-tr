---
title: 'Azure Cosmos DB giriş: API MongoDB | Microsoft Docs'
description: Depolamak için Azure Cosmos DB nasıl kullanabileceğiniz ve sorgu yoğun birimlerini JSON belgeleri, popüler OSS MongoDB API'lerini kullanarak düşük gecikme süresi ile bilgi edinin.
keywords: MongoDB nedir
services: cosmos-db
author: AndrewHoh
manager: kfile
editor: ''
documentationcenter: ''
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: anhoh
ms.openlocfilehash: d58933e510a56bf21bcd714dc93d4c7f0b57340b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-azure-cosmos-db-mongodb-api"></a>Azure Cosmos DB giriş: API MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB sağlar [anahtar teslim genel dağıtım](distribute-data-globally.md), [üretilen iş ve depolama esnek ölçeklendirme](partition-data.md) 99 ve garantili yüksek dünya, tek basamaklı milisaniyelik gecikme kullanılabilirlik, tarafından yedeklenen tüm [endüstri lideri SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler. 

![Azure Cosmos DB: MongoDB API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Azure Cosmos DB veritabanları için yazılmış uygulamalar için veri deposu olarak kullanılabilir [MongoDB](https://docs.mongodb.com/manual/introduction/). Bu işlev, varolan kullanarak anlamına gelir [sürücüleri](https://docs.mongodb.org/ecosystem/drivers/), uygulamanızın MongoDB, artık Azure Cosmos DB ile iletişim kurmak ve Azure Cosmos DB veritabanları MongoDB veritabanı yerine kullanmak için yazılmış. Çoğu durumda, sadece bir bağlantı dizesi değiştirilerek Azure Cosmos DB'de MongoDB kullanarak geçiş yapabilirsiniz. Bu işlevini kullanarak, kolayca oluşturabilir ve çalışma Genel dağıtılmış MongoDB veritabanı uygulamaları Azure bulut Azure Cosmos DB ve buna ait [kapsamlı endüstri lideri SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), bilinen kullanmaya devam ederken becerileri ve araçları MongoDB için.

**MongoDB Uyumluluk**: Azure Cosmos DB MongoDB 3.4 (sürüm 5) kablo protokolü uygulayan ve destekleyen gibi varolan MongoDB uzmanlık, uygulama kodu ve araçları kullanabilirsiniz [MongoDB toplama ardışık düzen](mongodb-feature-support.md#aggregation-pipeline). MongoDB kullanarak uygulamaları geliştirmek ve bunları tam olarak yönetilen kullanan üretim ve genel olarak dağıtılmış Azure Cosmos DB hizmeti dağıtabilirsiniz.

## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>MongoDB uygulamaları için Azure Cosmos DB kullanmanın avantajı nedir?

**Esnek bir şekilde ölçeklenebilir işleme ve Depolama:** uygulamalarınızı gerekiyor kolayca ölçeklendirme tarafından yukarı veya aşağı MongoDB veritabanınızın karşılayan. Verileriniz düşük tahmin edilebilirliğe sahip gecikme süreleri sağlamak için katı hal disklerinde (SSD) depolanır. Azure Cosmos DB neredeyse sınırsız depolama boyutlarına ve sağlanan işlemeye ölçeklenebilir MongoDB koleksiyonları destekler. Uygulamanız büyüdükçe, Azure Cosmos DB'yi tahmin edilebilir performansla sorunsuz ve esnek bir şekilde ölçeklendirebilirsiniz. 

**Bölgeli çoğaltma:** Azure Cosmos DB saydam çoğaltır, verilerinizi tüm bölgelere ilgili MongoDB hesabınızla arasında bileşim sağlarken verilere genel erişim gerektiren uygulamalar geliştirmek etkinleştirme tutarlılık, kullanılabilirlik ve performans, tüm ilgili garanti ile. Azure Cosmos DB çok girişli API’ler ile şeffaf bölgesel yük devretme olanağının yanı sıra aktarım hızını ve depolamayı dünya çapında elastik bir şekilde ölçeklendirme imkanı sağlar. Daha fazla bilgi edinin [verilerini genel dağıtmak](distribute-data-globally.md).

**Hiçbir Sunucu Yönetimi**: yönetmek ve MongoDB veritabanlarınızı ölçeklendirme gerekmez. Azure Cosmos DB herhangi bir altyapı ya da sanal makineleri kendiniz yönetmek zorunda değilsiniz demektir tam olarak yönetilen bir hizmettir. Azure Cosmos DB kullanılabilir 30 + [Azure bölgeleri](https://azure.microsoft.com/regions/services/).

**İnce ayarlanabilir tutarlılık düzeyleri:** Azure Cosmos DB MongoDB iki tutarlılık ayarları, güçlü ve nihai olan sürüm 3.4, şu anda uygular. Azure Cosmos DB multi-API olduğundan, tutarlılık ayarlar hesap düzeyinde uygulanabilir ve tutarlılık zorlama her API tarafından denetlenir. MongoDB 3.6 kadar oturum tutarlılığı kavramına oluştu oturum tutarlılığı kullanmak için MongoDB API hesabını ayarlarsanız, tutarlılık için son MongoDB API'leri kullanırken bir alt sürüme şekilde. Bilgisayarınızı-kendi-salt okunur garantisi MongoDB API hesabı için gerekiyorsa, hesap için varsayılan tutarlılık düzeyi strong ayarlanmalı veya sınırlanmış eskime durumu. Daha fazla bilgi için bkz. [Kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md).

| Azure Cosmos DB varsayılan tutarlılık düzeyi |   Mongo API (3.4) |
|---|---|
|Nihai| Nihai |
|Tutarlı Ön Ek| Son tutarlı sırası |
|Oturum| Son tutarlı sırası |
|Sınırlanmış Eskime Durumu| Güçlü |
| Güçlü | Güçlü |

**Otomatik dizin oluşturma**: varsayılan olarak, Azure Cosmos DB otomatik olarak tüm özellikleri, MongoDB belgelerde içinde veritabanı ve beklediğiniz veya herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını gerektirir dizinler. Ayrıca, bir Benzersizlik kısıtlamasını zaten Azure Cosmos DB'de otomatik dizinli tüm belge alanlarını üzerinde benzersiz dizin yeteneği sağlar.

**Kurumsal düzeyde**: Azure Cosmos DB yerel ve bölgesel hataları karşısında % 99,99 kullanılabilirlik ve veri koruması sağlamak üzere birden çok yerel çoğaltma destekler. Azure Cosmos DB sahip kurumsal düzeyde [uyumluluk sertifikalarından](https://www.microsoft.com/trustcenter) ve güvenlik özellikleri. 

Bu videoda ile Azure Cosmos DB üst düzey Program Yöneticisi, Aleksey Savateyev daha fazla bilgi edinin.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T136/player]
> 

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Bir Azure Cosmos DB hesabı oluşturmanız ve varolan MongoDB uygulamanızı Azure Cosmos DB kullanacak şekilde geçirmek için MongoDB quickstarts izleyin veya yeni bir tane oluşturabilirsiniz:

* [Var olan bir Node.js MongoDB web uygulamaya geçirmek](create-mongodb-nodejs.md).
* [.NET ve Azure portal ile bir MongoDB API web uygulaması oluşturma](create-mongodb-dotnet.md)
* [Java ile Azure portalı ile MongoDB API konsol uygulaması oluşturma](create-mongodb-java.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB MongoDB API'si hakkında bilgi Azure Cosmos DB belgeler genel tümleştirilmiştir, ancak başlamanıza yardımcı olmak için birkaç işaretçi şunlardır:

* İzleyin [MongoDB hesabına Bağlan](connect-mongodb-account.md) hesabı bağlantı dizesi bilgilerinizi alma hakkında bilgi için öğretici.
* İzleyin [Studio'yu kullanın 3T (MongoChef) Azure Cosmos DB ile](mongodb-mongochef.md) öğretici 3 T. Studio'da Azure Cosmos DB veritabanı MongoDB uygulama arasında bir bağlantı oluşturma hakkında bilgi edinin
* İzleyin [protokolü ile Azure Cosmos DB'de geçirme verilerini destekleyen MongoDB için](mongodb-migrate.md) verilerinizi bir API MongoDB veritabanı için içeri aktarmak için öğretici.
* Bir API MongoDB hesabı kullanarak bağlanmak [Robomongo](mongodb-robomongo.md).
* Kaç tane RUs işletim ile kullandığınız öğrenin [GetLastRequestStatistics komut ve Azure portal ölçümleri](request-units.md#GetLastRequestStatistics).
* Bilgi edinmek için nasıl [Genel dağıtılmış uygulamalar için okuma tercihleri yapılandırmak](../cosmos-db/tutorial-global-distribution-mongodb.md).
