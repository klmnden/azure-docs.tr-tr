---
title: Daha yüksek kullanılabilirlik ve Azure Cosmos DB'de performans için en iyi duruma getirmek için özel eşitlemeyi uygulama
description: Daha yüksek kullanılabilirlik ve Azure Cosmos DB'de performans için en iyi duruma getirmek için özel eşitlemesi uygulama hakkında bilgi edinin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 2/12/2019
ms.author: mjbrown
ms.openlocfilehash: 9033a7502919c8dc05053048272f3fa62f81b31d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56119296"
---
# <a name="how-to-implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Daha yüksek kullanılabilirlik ve performans için en iyi duruma getirmek için özel eşitlemeyi uygulama

Azure Cosmos DB, tutarlılık, performans ve kullanılabilirlik etmekten dengelemek, içinden seçim yapabileceğiniz için beş iyi tanımlanmış tutarlılık düzeyi sunar. Veriler zaman uyumlu olarak çoğaltılır ve Cosmos hesabı ile yapılandırılmış her bölgede dayanıklılık kalıcı güçlü tutarlılık sağlar. Bu yapılandırma, ancak en yüksek dayanıklılık düzeyini sağlar performans ve kullanılabilirlik karşılığında sunulur. Uygulama denetimi/kullanılabilirlik ödün vermeden uygulama ihtiyaçlarını uyacak şekilde veri dayanıklılığını hafifletin isterse, istenen dayanıklılık düzeyi sağlamak için uygulama katmanında özel eşitleme tercih edebilirsiniz.

Aşağıdaki diyagramda görsel olarak özel bir eşitleme modeli gösterilmektedir.

![Özel eşitleme](./media/how-to-custom-synchronization/custom-synchronization.png)

Bu senaryoda Cosmos kapsayıcı birden çok kıtada birçok bölgede küresel olarak çoğaltılır. Bu senaryoda tüm bölgeler için güçlü tutarlılık kullanarak performansı etkiler. Uygulama yazma gecikmesi ödün vermeden yüksek veri dayanıklılık düzeyi sağlamak için aynı oturum belirteci paylaşan iki istemcileri kullanabilirsiniz.

İlk istemci, yerel bölge (örneğin, ABD Batı) veri yazabilirsiniz. İkinci bir istemci (örneğin, ABD Doğu'daki), eşitleme sağlamak için kullanılan salt okunur bir istemcidir. Akışı sağlama Thread.CurrentPrincipal z tokenu relace tarafından aşağıdaki okuma yazma yanıtını, okuma, ABD Doğu yazma işlemlerini eşitlemesini garanti eder. Azure Cosmos DB yazma en az bir bölgeye göre görüldüğünde ve gitmek için özgün yazma bölgesi varsa bölgesel bir kesinti varlığını sürdürmesi için garantili sağlayacaktır. Bu senaryoda, ABD Doğu, güçlü tutarlılık kullanan tüm bölgeler arasında gecikme süresini azaltmak için her yazma eşitlenir. Yazma her bölgede nerede oluştuğunu gösteriyorsa, çok yöneticili bir senaryoda, bu model, birden çok bölgede paralel eşitlemek için genişletilebilir.

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
