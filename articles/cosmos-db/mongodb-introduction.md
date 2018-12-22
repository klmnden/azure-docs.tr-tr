---
title: Azure Cosmos DB MongoDB API'si için giriş
description: Sorgu çok büyük hacimlerdeki JSON belgeleri depolamak için Azure Cosmos DB'yi nasıl kullanabileceğinizi ve düşük gecikme ile popüler OSS MongoDB API'sini kullanarak öğrenin.
keywords: MongoDB API için Azure Cosmos DB
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: overview
ms.date: 12/19/2018
ms.author: sclyon
ms.openlocfilehash: c2e009b81fa072fef5249f5dc697fcad547adb3e
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53744182"
---
# <a name="azure-cosmos-db-for-mongodb-api-clients"></a>Azure Cosmos DB MongoDB API istemciler için

[Azure Cosmos DB](introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB, [kullanıma hazır genel dağıtım](distribute-data-globally.md), dünya çapında [aktarım hızı ve depolama için esnek ölçeklendirme](partition-data.md), 99. yüzdebirlik dilimde tek haneli milisaniyelik gecikme süreleri ve garantili yüksek kullanılabilirlik olanakları sunar ve bunların tümü [sektör lideri SLA’lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) ile desteklenir. Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler. Varsayılan olarak, Cosmos DB SQL API'sini kullanarak ile etkileşim kurabilir. Ayrıca, Cosmos DB hizmetinin, Cassandra, MongoDB, Gremlin ve Azure tablo depolama gibi sık kullanılan NoSQL API'leri için kablo protokollerini kullanır. Bu, Cosmos veritabanıyla etkileşim kurmanıza imkan tanıdık NoSQL istemci sürücüleri ve araçlarını kullanmanıza olanak sağlar.

## <a name="wire-protocol-compatibility"></a>Kablo protokolü uyumluluğu

Cosmos DB Cassandra, MongoDB, Gremlin ve Azure tablo depolama da dahil olmak üzere sık kullanılan NoSQL veritabanları kablo protokolleri uygular. Yerel bir uygulama kablo protokolünü verimli bir şekilde ve doğrudan Cosmos DB içinde vererek, mevcut istemci SDK'ları, sürücüler ve NoSQL veritabanları araçlarının Cosmos DB ile saydam bir şekilde etkileşim kurmak sağlar. Cosmos DB, kablo ile uyumlu API'leri herhangi bir NoSQL veritabanı sağlamak için herhangi bir kaynak kod veritabanı kullanmaz.

Varsayılan olarak, Azure Cosmos DB MongoDB API'si için kablo protokolünü 3.2 sürümü ile uyumludur. Bir önizleme özelliği olarak şu anda özellikleri veya kablo protokolünü 3.4 sürümünde eklenen sorgu işleçleri kullanılabilir. Bu protokol sürümleri anlayan herhangi bir MongoDB istemcisi sürücüsünü yerel olarak Azure Cosmos DB'ye bağlanmak görebilmeniz gerekir.

![MongoDB API için Azure Cosmos DB](./media/mongodb-introduction/cosmosdb-mongodb.png) 

## <a name="key-benefits"></a>Önemli avantajlar

Cosmos DB, hizmet olarak tam olarak yönetilen, küresel olarak dağıtılan bir veritabanı olarak öne çıkan avantajları açıklanmıştır [burada](introduction.md). Ayrıca, popüler NoSQL API kablo protokolleri yerel olarak uygulayarak, Cosmos DB aşağıdaki avantajları sağlar:

* Uygulama mantığınızın önemli kısımları korurken Cosmos DB uygulamanıza kolayca geçirin.
* Uygulamanızı taşınabilir tutun ve bulut satıcısı belirsiz durumda kalır.
* Sektör ortak NoSQL Cosmos DB tarafından desteklenen API'ler için önde gelen, mali olarak desteklenen SLA'ları alın.
* Sağlanan aktarım hızı elastik olarak ölçeklendirmenize ve Cosmos veritabanlarınız için depolama ihtiyaçlarınıza göre ve yalnızca aktarım hızını ve ihtiyacınız olan depolama için ödeme yaparsınız. Bu, önemli maliyet tasarrufları yol açar.
* Çok yöneticili çoğaltma ile kullanıma hazır, küresel dağıtım.

## <a name="cosmos-db-for-mongodb-api-clients"></a>Cosmos DB MongoDB API istemciler için

Bir Azure Cosmos DB hesabı oluşturmak ve mevcut MongoDB uygulamanızı Azure Cosmos DB kullanacak şekilde taşımak veya yeni bir tane geliştirmek için MongoDB hızlı başlangıçlarını uygulayın:

* [Mevcut Node.js MongoDB Web uygulamasını taşıma](create-mongodb-nodejs.md).
* [.NET ve Azure Cosmos DB için MongoDB API'sini kullanarak Azure portalı ile bir web uygulaması derleme](create-mongodb-dotnet.md)
* [Java ve Azure Cosmos DB için MongoDB API'sini kullanarak Azure portalında bir konsol uygulaması oluşturma](create-mongodb-java.md)

## <a name="next-steps"></a>Sonraki adımlar

İşte başlamanıza yardımcı olacak birkaç ipucu:

* İzleyin [bir MongoDB uygulamasını Azure Cosmos DB'ye bağlanmak](connect-mongodb-account.md) hesabı bağlantı dizesi bilgilerinizi alın hakkında bilgi edinmek için öğretici.
* İzleyin [Azure Cosmos DB ile kullanım Studio 3T](mongodb-mongochef.md) 3 t Studio'da Azure Cosmos DB veritabanı ve MongoDB uygulaması arasında bir bağlantı oluşturma hakkında bilgi edinmek için öğretici
* İzleyin [protokolü ile verilerini Azure Cosmos DB'ye geçirme için MongoDB API'si desteği](mongodb-migrate.md) verilerinizi Cosmos veritabanına içeri aktarmak için öğretici.
* Bir Cosmos hesap kullanarak bağlanmak [Robo 3T](mongodb-robomongo.md).
* [Küresel olarak dağıtılmış uygulamalar için okuma tercihlerini yapılandırmayı](../cosmos-db/tutorial-global-distribution-mongodb.md) öğrenin.

<sup>Not: Bu makalede, Azure Cosmos DB'nin MongoDB veritabanları kablo protokolü uyum sağlayan bir özellik açıklanmaktadır. Microsoft bu hizmeti sağlamak için MongoDB veritabanları çalıştırmaz. Azure Cosmos DB ile MongoDB, Inc.'e bağlı değil</sup>
