---
title: Değişiklik akışı işlemci Kitaplığı'nda Azure Cosmos DB ile çalışma
description: Azure Cosmos DB değişiklik işlemci kitaplığı akışa.
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rafats
ms.openlocfilehash: 9d427a8001112e4994597b86579d85156f94a870
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629383"
---
# <a name="using-the-azure-cosmos-db-change-feed-processor-library"></a>Kullanarak Azure Cosmos DB değişiklik akışı işlemci kitaplığı

[Azure Cosmos DB değişiklik akışı işlemci Kitaplığı](sql-api-sdk-dotnet-changefeed.md) , olay işleme çeşitli tüketicilere dağıtmak yardımcı olur. Bu kitaplık, bölümler ve paralel olarak çalışan birden çok iş parçacığı üzerinde okuma değişiklikleri basitleştirir.

Değişiklik akışı işlemci kitaplığı ana avantajı, her bölüm yönetmek zorunda olmadığınız ve devamlılık belirteci ve her kapsayıcı el ile yoklamaya yoksa ' dir.

Değişiklik akışı işlemci kitaplığı, bölümler ve paralel olarak çalışan birden çok iş parçacığı üzerinde okuma değişiklikleri basitleştirir. Kiralama mekanizması kullanılarak bölümler arasında okuma değişiklikleri otomatik olarak yönetir. Değişiklik akışı işlemci kitaplığı kullanan iki istemciler başlatırsanız, aşağıdaki görüntüde görebileceğiniz gibi bunlar kendi aralarında iş bölün. İstemci sayısı artmaya devam ederken, kendi aralarında iş bölme tutun.

![Kullanarak Azure Cosmos DB değişiklik akışı işlemci kitaplığı](./media/change-feed-processor/change-feed-output.png)

Sol istemci ilk olarak başlatıldığından ve tüm bölümleri ve ardından ikinci bir istemci başlatıldı ve ardından ilk izin bazı ikinci istemci kiraları Git izleme başlatıldı. Bu, farklı makineler ve istemcilerin aralarında iş dağıtmak için etkili bir yoludur.

Aynı kapsayıcı izleme ve aynı kira kullanarak iki sunucusuz Azure işlevleri varsa, iki işlev nasıl işlemci kitaplığı bölümlerini işlemek karar verdikten sonra bağlı olarak farklı belgeler alabilirsiniz.

## <a name="implementing-the-change-feed-processor-library"></a>Değişiklik uygulama akışı işlemci kitaplığı

Değişiklik akışı işlemci kitaplığı uygulama dört ana bileşenleri şunlardır: 

1. **İzlenen kapsayıcı:** değişiklik akışı oluşturulan veriler izlenen kapsayıcı vardır. Kapsayıcı içinde değişiklik akışı, tüm eklemeleri ve değişiklikleri izlenen kapsayıcıya yansıtılır.

1. **Kira kapsayıcı:** değişiklik arasında birden fazla çalışana akışı işleme kira kapsayıcı koordinatlar. Ayrı bir kapsayıcı, bölüm başına bir kira ile kiraları depolamak için kullanılır. Değişiklik akışı işlemci çalıştığı için yazma bölgesi yakın farklı bir hesapla bu kira kapsayıcı depolamak için avantajlıdır. Bir kira nesnesi aşağıdaki öznitelikleri içerir:

   * Sahibi: kira sahibi olan konak belirtir.

   * Devamlılık: belirli bir bölüm için akış değiştirme (devamlılık belirteci) konumu belirtir.

   * Zaman damgası: kira güncelleştirildi; son saat zaman damgası, kiralama süresi olarak kabul edilip edilmediğini kontrol etmek için kullanılabilir.

1. **İşleyicisi ana bilgisayarı:** her ana bilgisayar işlemi konakların kaç tane Etkin kiralar olmadığına göre kaç bölümlerini belirler.

   * Bir ana bilgisayar başlatıldığında tüm konaklar arasında iş yükünü dengelemek için kiraları alır. Kira etkin şekilde bir konak kiralama, düzenli aralıklarla yeniler.

   * Bir ana bilgisayar denetim noktaları son kirasını her devamlılık belirteci okuyun. Eşzamanlılık güvenliği sağlamak için her bir kira güncelleştirme ETag bir konak denetler. Diğer denetim noktası stratejileri de desteklenir.

   * Kapatma, bağlı bir konak tüm kira serbest bırakır, ancak depolanan denetim noktası daha sonra okumaya devam edebilmek için devamlılık bilgileri tutar.

   Şu anda, konak sayısını bölüm (kiraları) sayısından büyük olamaz.

1. **Tüketiciler:** tüketiciler veya çalışanlar, olan değişiklik akışı, her konak tarafından başlatılan işlemleri gerçekleştiren iş parçacıkları. Her işleyicisi ana bilgisayarı, birden fazla tüketici olabilir. Her tüketici, değişiklik atandığı bölümünden akışı ve değişiklikleri ana bilgisayar bildirir ve kiralama süresi okur.

Daha fazla değişiklik akışı nasıl bu dört öğeden anlamak için işlemci iş birlikte bakalım, aşağıdaki diyagramda bir örnek. İzlenen koleksiyonu, belgeleri depolayan ve "City" Bölüm anahtarı olarak kullanır. Mavi bölüm "A-E" "City" alanından belgelerle vb. içeren görüyoruz. Her iki tüketici paralel dört bölümden okuma içeren iki ana vardır. Oklar, değişiklik akışı belirli bir noktada okuma tüketiciler gösterir. Açık mavi değişiklik akışı zaten okuma değişiklikleri temsil ederken, ilk bölümü, koyu mavi okunmamış değişiklikleri temsil eder. Ana bilgisayarlar, her bir tüketicinin geçerli okuma konumunu izlemek için bir "Devam" değerini depolamak için kira koleksiyonu kullanın.

![Değişiklik akışı işlemci örneği](./media/change-feed-processor/changefeedprocessor.png)

### <a name="change-feed-and-provisioned-throughput"></a>Değişiklik akışı ve sağlanan aktarım hızı

Cosmos kapsayıcılar ve dışındaki veri hareketleri her zaman RU tüketir beri tüketilen RU için ücretlendirilir. Kira kapsayıcı tarafından tüketilen RU'ları için ücretlendirilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB değişiklik akışı işlemci kitaplığı](sql-api-sdk-dotnet-changefeed.md)
* [NugGet paket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [GitHub üzerinde ek örnekleri](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de akış değiştirme hakkında daha fazla bilgi edinmek için şimdi geçebilirsiniz:

* [Değişiklik akışı genel bakış](change-feed.md)
* [Değişiklik akışını okumak için yollar](read-change-feed.md)
* [Azure işlevleri ile akış Değiştir](change-feed-functions.md)