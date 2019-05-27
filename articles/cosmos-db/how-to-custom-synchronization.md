---
title: Daha yüksek kullanılabilirlik ve Azure Cosmos DB'de performans için en iyi duruma getirmek için özel eşitlemeyi uygulama
description: Daha yüksek kullanılabilirlik ve Azure Cosmos DB'de performans için en iyi duruma getirmek için özel bir eşitleme uygulamayı öğrenin.
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: rimman
ms.openlocfilehash: cd89a145f5746696cc8fc163eb46896081d85a90
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240956"
---
# <a name="implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Daha yüksek kullanılabilirlik ve performansı iyileştirmek için özel eşitlemesi uygulama

Azure Cosmos DB sunar [beş iyi tanımlanmış tutarlılık düzeyi](consistency-levels.md) , tutarlılık, performans ve kullanılabilirlik etmekten dengelemek, içinden seçim yapabileceğiniz için. Güçlü tutarlılık, verileri zaman uyumlu olarak çoğaltılan ve Azure Cosmos hesabı kullanılabilir olduğu her bölgede arızaya kalıcı emin olun yardımcı olur. Bu yapılandırma, en yüksek dayanıklılık düzeyini sağlar, ancak performans ve kullanılabilirlik karşılığında sunulur. Uygulamanızın olasılığına karşı veri dayanıklılığını hafifletin veya denetim istiyorsanız gereksinimlerine kullanılabilirlik ödün vermeden uygulama uyacak şekilde, kullanabileceğiniz *özel eşitleme* dayanıklılık düzeyine ulaşmak için uygulama katmanında, istersiniz.

Aşağıdaki görüntüde, görsel olarak özel bir eşitleme modelini göstermektedir:

![Özel eşitleme](./media/how-to-custom-synchronization/custom-synchronization.png)

Bu senaryoda, bir Azure Cosmos kapsayıcı birden çok kıtadaki üzerinde birçok bölgede küresel olarak çoğaltılır. Bu senaryoda tüm bölgeler için güçlü tutarlılık kullanarak performansı etkiler. Ödün yazma olmadan gecikme süresi yüksek veri dayanıklılık düzeyi sağlamak için uygulamanın aynı paylaşan iki istemciler'i kullanabilir [Oturum belirteci](how-to-manage-consistency.md#utilize-session-tokens).

İlk istemci, yerel bölge (örneğin, ABD Batı) veri yazabilirsiniz. İkinci bir istemci (örneğin, ABD Doğu'daki) eşitleme sağlamak için kullanılan salt okunur bir istemcidir. Thread.CurrentPrincipal z tokenu relace aşağıdaki okuma yazma yanıtını akan tarafından salt okunur ABD Doğu Yazar eşitlenmesini sağlar. Azure Cosmos DB yazma en az bir bölgeye göre görüldüğünde sağlar. Özgün yazma bölgesi kalırsa bölgesel bir kesinti varlığını sürdürmesi için sağlanır. Bu senaryoda, ABD Doğu, güçlü tutarlılık kullanan tüm bölgeler arasında gecikme süresini azaltma her yazma eşitlenir. Burada her bölgede yazma işlemleri meydana, çok yöneticili bir senaryoda, bu model, birden çok bölgede paralel eşitlenecek genişletebilirsiniz.

## <a name="configure-the-clients"></a>İstemcilerini yapılandırma

Aşağıdaki örnek iki özel eşitleme istemcilerde örnekleyen bir veri erişim katmanı gösterir:

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

İstemcilerin başlatıldıktan sonra uygulama yazma işlemlerini ABD Doğu gibi yerel bölge (ABD Batı) ve zorla eşitleme yazma işlemlerini gerçekleştirebilirsiniz.

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

Birden çok bölgede paralel eşitlenecek model genişletebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Genel dağıtım ve Azure Cosmos DB tutarlılık hakkında daha fazla bilgi edinmek için bu makaleleri okuyun:

* [Azure Cosmos DB'de doğru tutarlılık düzeyi seçin](consistency-levels-choosing.md)
* [Azure Cosmos DB'deki tutarlılık, kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Azure Cosmos DB'deki tutarlılık yönetme](how-to-manage-consistency.md)
* [Azure Cosmos DB'de bölümleme ve veri dağıtım](partition-data.md)
