---
title: Azure Cosmos DB Azure Cosmos DB'de değişiklik erişim akışı
description: Bu makalede, okumak ve değişiklik akışı Azure Cosmos DB Azure Cosmos DB'de erişmek kullanılabilir farklı seçenekler açıklanır.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: e008b44ee2859f319d0250658d7c2beb190af1c2
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967164"
---
# <a name="reading-azure-cosmos-db-change-feed"></a>Okuma Azure Cosmos DB değişiklik akışı

Aşağıdaki seçeneklerden herhangi birini kullanarak Azure Cosmos DB değişiklik akışı ile çalışabilirsiniz:

* Azure işlevleri'ni kullanarak
* Kullanarak değişiklik akışı işlemci kitaplığı
* Azure Cosmos DB SQL API SDK'sını kullanma

## <a name="using-azure-functions"></a>Azure işlevleri'ni kullanarak

Azure işlevleri, basit ve önerilen seçenektir. Azure işlevleri uygulamayı bir Azure Cosmos DB tetikleyicisi oluşturduğunuzda, bağlanmak için kapsayıcı seçebilir ve Azure işlevi, kapsayıcıya bir değişiklik olduğunda tetiklenen. Tetikleyiciler, Azure işlevleri portalı, Azure Cosmos DB portal kullanarak veya SDK'lar ile program aracılığıyla oluşturulabilir. Visual Studio ve VS Code Azure işlevleri yazmak için destek sağlar ve platformlar arası geliştirme için Azure işlevleri CLI'yı da kullanabilirsiniz. Yazma ve masaüstünüzde kodda hata ayıklama ve ardından işlev tek bir tıklamayla dağıtın. Bkz: [Azure işlevleri ile sunucusuz veritabanı bilgi işlem](serverless-computing-database.md) ve [değişiklik kullanarak, Azure işlevleri ile akış](change-feed-functions.md)) makaleleri daha fazla bilgi için.

## <a name="using-the-change-feed-processor-library"></a>Kullanarak değişiklik akışı işlemci kitaplığı

Değişiklik akışı işlemci kitaplığı karmaşıklığını gizler ve yine de bir değişiklik akışı denetimini sağlar. Kitaplığı, kitaplık için işleme işlevinizi burada çağrılır gözlemci deseni izler. Bir yüksek aktarım hızı değişiklik akışı varsa, değişiklik akışını okumak için birden çok istemci örneği oluşturabilir. Değişiklik akışı işlemci kitaplığı kullandığımızdan, otomatik olarak yükü farklı istemciler arasında bu mantığı uygulamasına gerek kalmadan böler. Tüm karmaşıklığı kitaplığı tarafından işlenir. Yük dengeleyicinizi olmasını istediğiniz sonra uygulayabileceğiniz `IPartitionLoadBalancingStrategy` stratejisi değişiklik işlemek için özel bir bölümü için akış. Daha fazla bilgi için bkz. [kullanarak değişiklik akışı işlemci Kitaplığı](change-feed-processor.md).

## <a name="using-the-azure-cosmos-db-sql-api-sdk"></a>Azure Cosmos DB SQL API SDK'sını kullanma

SDK ile değişiklik akışı düşük düzeyli denetimin alın. Kontrol noktası yönetme, erişim belirli bir mantıksal bölüm anahtarı, vs. Birden fazla okuyucuyu kapsayacak varsa, kullanabileceğiniz `ChangeFeedOptions` farklı iş parçacıkları veya farklı istemcilerin okuma yükünü dağıtmak için. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de akış değiştirme hakkında daha fazla bilgi edinmek için şimdi geçebilirsiniz:

* [Değişiklik akışı genel bakış](change-feed.md)
* [Azure işlevleri ile akış Değiştir](change-feed-functions.md)
* [Kullanarak değişiklik akışı işlemci kitaplığı](change-feed-processor.md)
