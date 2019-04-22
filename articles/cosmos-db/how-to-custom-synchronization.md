---
title: Daha yüksek kullanılabilirlik ve Azure Cosmos DB'de performans için en iyi duruma getirmek için özel eşitlemeyi uygulama
description: Daha yüksek kullanılabilirlik ve Azure Cosmos DB'de performans için en iyi duruma getirmek için özel eşitlemesi uygulama hakkında bilgi edinin
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: d948798f161eb36578cb679b6d96409917424fd4
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678471"
---
# <a name="how-to-implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Daha yüksek kullanılabilirlik ve performans için en iyi duruma getirmek için özel eşitlemeyi uygulama

Azure Cosmos DB sunar [beş iyi tanımlanmış tutarlılık düzeyi](consistency-levels.md) , tutarlılık, performans ve kullanılabilirlik etmekten dengelemek, içinden seçim yapabileceğiniz için. Veri eşzamanlı olarak kopyalandığı ve Azure Cosmos hesabı kullanılabilir olduğu her bölgede arızaya kalıcı güçlü tutarlılık sağlar. En üst düzeyde dayanıklılık, sağlarken bu yapılandırma, performans ve kullanılabilirlik karşılığında sunulur. Uygulama denetimi / olasılığına karşı veri dayanıklılığını hafifletin isterse gereksinimlerine kullanılabilirlik ödün vermeden uygulama uyacak şekilde, tercih edebilirsiniz *özel eşitleme* istenen düzeyini elde etmek için uygulama katmanı dayanıklılık.

Aşağıdaki diyagramda görsel olarak özel bir eşitleme modeli gösterilmektedir.

![Özel eşitleme](./media/how-to-custom-synchronization/custom-synchronization.png)

Bu senaryoda, bir Azure Cosmos kapsayıcı birden çok kıtada birçok bölgede küresel olarak çoğaltılır. Bu senaryoda tüm bölgeler için güçlü tutarlılık kullanarak performansı etkiler. Yazma gecikme süresi ödün vermeden yüksek veri dayanıklılık düzeyi sağlamak için uygulamanın aynı paylaşan iki istemcileri kullanabilirsiniz [Oturum belirteci](how-to-manage-consistency.md#utilize-session-tokens).

İlk istemci, yerel bölge (örneğin, ABD Batı) veri yazabilirsiniz. İkinci bir istemci (örneğin, ABD Doğu'daki), eşitleme sağlamak için kullanılan salt okunur bir istemcidir. Thread.CurrentPrincipal z tokenu relace aşağıdaki okuma yazma yanıtını akan tarafından okunur ABD Doğu yazma işlemlerini eşitlemesini garanti eder. Azure Cosmos DB yazma en az bir bölgeye göre görüldüğünde ve gitmek için özgün yazma bölgesi varsa bölgesel bir kesinti varlığını sürdürmesi için garantili sağlayacaktır. Bu senaryoda, ABD Doğu, güçlü tutarlılık kullanan tüm bölgeler arasında gecikme süresini azaltmak için her yazma eşitlenir. Yazma her bölgede nerede oluştuğunu gösteriyorsa, çok yöneticili bir senaryoda, bu model, birden çok bölgede paralel eşitlemek için genişletilebilir.

## <a name="configure-the-clients"></a>İstemcilerini yapılandırma

Aşağıdaki örnekte, iki istemci bu amaç için örnekleme veri erişim katmanı gösterir.

```csharp
class MyDataAccessLayer
{
    private DocumentClient writeClient;
    private DocumentClient readClient;

    public async Task InitializeAsync(Uri accountEndpoint, string key)
    {
        ConnectionPolicy writeConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp,
            UseMultipleWriteLocations = true
        };
        writeConnectionPolicy.SetCurrentLocation(LocationNames.WestUS);

        ConnectionPolicy readConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp
        };
        readConnectionPolicy.SetCurrentLocation(LocationNames.EastUS);

        writeClient = new DocumentClient(accountEndpoint, key, writeConnectionPolicy);
        readClient = new DocumentClient(accountEndpoint, key, readConnectionPolicy, ConsistencyLevel.Session);

        await Task.WhenAll(new Task[]
        {
            writeClient.OpenAsync(),
            readClient.OpenAsync()
        });
    }
}
```

## <a name="implement-custom-synchronization"></a>Özel eşitlemeyi uygulama

İstemcilerin başlatıldıktan sonra uygulama yerel bölge (ABD Batı) yazma işlemlerini gerçekleştirebilir ve zorla Yazar ABD Doğu gibi eşitleyebilirsiniz.

```csharp
class MyDataAccessLayer
{
    public async Task CreateItem(string containerLink, Document document)
    {
        ResourceResponse<Document> response = await writeClient.CreateDocumentAsync(
            containerLink, document);

        await readClient.ReadDocumentAsync(response.Resource.SelfLink,
            new RequestOptions { SessionToken = response.SessionToken });
    }
}
```

Bu model, birden çok bölgede paralel eşitlemek için genişletilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Genel dağıtım ve Azure Cosmos DB tutarlılık hakkında daha fazla bilgi edinmek için bu makaleleri okuyun:

* [Azure Cosmos DB'de doğru tutarlılık düzeyi seçme](consistency-levels-choosing.md)
* [Azure Cosmos DB'deki tutarlılık, kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Azure Cosmos DB'deki tutarlılık yönetme](how-to-manage-consistency.md)
* [Azure Cosmos DB'de bölümleme ve veri dağıtım](partition-data.md)
