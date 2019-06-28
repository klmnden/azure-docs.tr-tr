---
title: Yönergeler için daraltılmış istekler
description: Kısıtlanmış Azure kaynak grafiğe istekleri önlemek için daha iyi sorgular oluşturmayı öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: c644d230846d9c644c3845348431eef36c8279c8
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276905"
---
# <a name="guidance-for-throttled-requests-in-azure-resource-graph"></a>Azure kaynak Graph'te daraltılmış istekler için yönergeler

Azure kaynak graf verileri programlı ve sık kullanımını oluştururken, sorguların sonuçlarını için nasıl azaltma etkileri göz önünde bulundurarak yapılmalıdır. Yol verileri değiştirme yardımcı olabilir ve kuruluşunuzun aşarak önlemek ve Azure kaynaklarınızı hakkında güncel veri akışını korumak istenmektedir.

Bu makale, dört alanları ve Azure kaynak Graph sorguları oluşturulmasını ilgili desenleri kapsar:

- Azaltma üstbilgileri anlama
- Toplu işlem sorguları
- Kademelendirme sorguları
- Sayfalandırma etkisini

## <a name="understand-throttling-headers"></a>Azaltma üstbilgileri anlama

Azure Kaynak Grafiği, bir zaman penceresi süresine göre her bir kullanıcı için kota numarası ayırır. Örneğin, bir kullanıcı kısıtlanan olmadan en fazla 15 sorguları içinde her 5 saniyelik aralıklarda gönderebilirsiniz. Kota değeri faktörlerden tarafından belirlenir ve değişikliğe tabidir.

Her sorgu yanıt olarak, Azure kaynak Graph azaltma iki üstbilgi ekler:

- `x-ms-user-quota-remaining` (int): Kullanıcı için kalan kaynak kotası. Sorgu sayısı bu değeri eşler.
- `x-ms-user-quota-resets-after` (ss): Bir kullanıcının kota tüketim sıfırlanana kadar süre.

Üstbilgileri nasıl çalıştığını göstermek için üst bilgi ve değerlerini içeren bir sorgu yanıtında gözden geçirelim `x-ms-user-quota-remaining: 10` ve `x-ms-user-quota-resets-after: 00:00:03`.

- Sonraki 3 saniye içinde kısıtlanan olmadan en fazla 10 sorguları gönderilebilir.
- 3 saniye, değerlerini `x-ms-user-quota-remaining` ve `x-ms-user-quota-resets-after` sıfırlanır `15` ve `00:00:05` sırasıyla.

Üstbilgileri kullanmaya bir örnek görmek için _geri alma_ sorgu isteklerinde örnekte bkz [paralel sorgu](#query-in-parallel).

## <a name="batching-queries"></a>Toplu işlem sorguları

Sorgular aboneliğe, kaynak grubu ya da tek başına bir kaynak tarafından toplu işlem sorguları paralelleştirmek değerinden daha verimli olur. Daha büyük bir sorgu kota maliyetini genellikle çok sayıda küçük ve hedeflenen sorgu kota maliyete daha azdır. Toplu iş boyutu olması önerilir küçüktür _300_.

- Hatalı bir şekilde iyileştirilmiş bir yaklaşım örneği

  ```csharp
  // NOT RECOMMENDED
  var header = /* your request header */
  var subscriptionIds = /* A big list of subscriptionIds */

  foreach (var subscriptionId in subscriptionIds)
  {
      var userQueryRequest = new QueryRequest(
          subscriptions: new[] { subscriptionId },
          query: "project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

  // ...
  }
  ```

- En iyi duruma getirilmiş bir toplu işlem yaklaşım #1 örneği

  ```csharp
  // RECOMMENDED
  var header = /* your request header */
  var subscriptionIds = /* A big list of subscriptionIds */

  const int batchSize = 100;
  for (var i = 0; i <= subscriptionIds.Count / batchSize; ++i)
  {
      var currSubscriptionBatch = subscriptionIds.Skip(i * batchSize).Take(batchSize).ToList();
      var userQueryRequest = new QueryRequest(
          subscriptions: currSubscriptionBatch,
          query: "project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

    // ...
  }
  ```

- En iyi duruma getirilmiş bir toplu işlem yaklaşım #2 örneği

  ```csharp
  // RECOMMENDED
  var header = /* your request header */
  var resourceIds = /* A big list of resourceIds */

  const int batchSize = 100;
  for (var i = 0; i <= resourceIds.Count / batchSize; ++i)
  {
      var resourceIdBatch = string.Join(",",
          resourceIds.Skip(i * batchSize).Take(batchSize).Select(id => string.Format("'{0}'", id)));
      var userQueryRequest = new QueryRequest(
          subscriptions: subscriptionList,
          query: $"where id in~ ({resourceIds}) | project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

    // ...
  }
  ```

## <a name="staggering-queries"></a>Kademelendirme sorguları

Neden azaltma zorlanır, sorguları düzenlenebilmesine öneririz. Diğer bir deyişle, aynı anda 60 sorguları göndermek yerine, sorguları dört 5 saniyelik pencerelere basamaklandırmak:

- Sorgu olmayan aşamalı zamanlama

  | Sorgu sayısı         | 60  | 0    | 0     | 0     |
  |---------------------|-----|------|-------|-------|
  | Zaman aralığı (sn) | 0-5 | 5-10 | 10-15 | 15-20 |

- Sorgu zamanlama aşamalı

  | Sorgu sayısı         | 15  | 15   | 15    | 15    |
  |---------------------|-----|------|-------|-------|
  | Zaman aralığı (sn) | 0-5 | 5-10 | 10-15 | 15-20 |

Azure kaynak Graph sorgulanırken azaltma üstbilgileri uyarak örneği aşağıdadır:

```csharp
while (/* Need to query more? */)
{
    var userQueryRequest = /* ... */
    // Send post request to Azure Resource Graph
    var azureOperationResponse = await this.resourceGraphClient
        .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
        .ConfigureAwait(false);

    var responseHeaders = azureOperationResponse.response.Headers;
    int remainingQuota = /* read and parse x-ms-user-quota-remaining from responseHeaders */
    TimeSpan resetAfter = /* read and parse x-ms-user-quota-resets-after from responseHeaders */
    if (remainingQuota == 0)
    {
        // Need to wait until new quota is allocated
        await Task.Delay(resetAfter).ConfigureAwait(false);
    }
}
```

### <a name="query-in-parallel"></a>Paralel sorgu

Toplu işleme paralelleştirme tavsiye edilir olsa da, zamanlar vardır burada sorguları kolayca toplu olarak işlenemeyen. Bu gibi durumlarda birden çok sorgu paralel biçimde göndererek Azure Kaynak Grafiği sorgulama isteyebilirsiniz. İçin nasıl bir örnek aşağıdadır _geri alma_ senaryolarda üstbilgileri daraltma üzerinde göre:

```csharp
IEnumerable<IEnumerable<string>> queryBatches = /* Batches of queries  */
// Run batches in parallel.
await Task.WhenAll(queryBatches.Select(ExecuteQueries)).ConfigureAwait(false);

async Task ExecuteQueries(IEnumerable<string> queries)
{
    foreach (var query in queries)
    {
        var userQueryRequest = new QueryRequest(
            subscriptions: subscriptionList,
            query: query);
        // Send post request to Azure Resource Graph.
        var azureOperationResponse = await this.resourceGraphClient
            .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
            .ConfigureAwait(false);
        
        var responseHeaders = azureOperationResponse.response.Headers;
        int remainingQuota = /* read and parse x-ms-user-quota-remaining from responseHeaders */
        TimeSpan resetAfter = /* read and parse x-ms-user-quota-resets-after from responseHeaders */
        if (remainingQuota == 0)
        {
            // Delay by a random period to avoid bursting when the quota is reset.
            var delay = (new Random()).Next(1, 5) * resetAfter;
            await Task.Delay(delay).ConfigureAwait(false);
        }
    }
}
```

## <a name="pagination"></a>Sayfalandırma

Azure Kaynak Grafiği tek sorgu yanıtına en fazla 1000 giriş döndürdüğünden, gerekebilir [sayfalandırma](./work-with-data.md#paging-results) sorgularınızı tam veri kümesinde aradığınız almak için. Ancak, bazı Azure kaynak Graph istemcileri sayfalandırma diğerlerinden farklı bir şekilde işleyin.

- C# SDK’sı

  ResourceGraph SDK'sını kullanarak, önceki sorgu yanıtı sonraki sayfalandırılmış sorgu iade edilen atlama belirteci ileterek sayfalandırma işlemek gerekir. Bu tasarım, tüm sayfalandırılmış çağrılarından sonuçlarını toplamak ve bunları sonunda birleştirmek için ihtiyaç duyduğunuz anlamına gelir. Bu durumda, gönderdiğiniz her sayfalandırılmış sorgu bir sorgu kota alır:

  ```csharp
  var results = new List<object>();
  var queryRequest = new QueryRequest(
      subscriptions: new[] { mySubscriptionId },
      query: "project id, name, type | top 5000");
  var azureOperationResponse = await this.resourceGraphClient
      .ResourcesWithHttpMessagesAsync(queryRequest, header)
      .ConfigureAwait(false);
  while (!string.Empty(azureOperationResponse.Body.SkipToken))
  {
      queryRequest.SkipToken = azureOperationResponse.Body.SkipToken;
      // Each post call to ResourceGraph consumes one query quota
      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(queryRequest, header)
          .ConfigureAwait(false);
      results.Add(azureOperationResponse.Body.Data.Rows);

      // Inspect throttling headers in query response and delay the next call if needed.
  }
  ```

- Azure CLI / Azure PowerShell

  Azure CLI veya Azure PowerShell kullanarak, Azure kaynak Graph sorguları otomatik olarak en fazla 5000 girişi getirilecek sayfalandırılmış. Sorgu sonuçları tüm sayfalandırılmış çağrılarından girdilerin birleşik bir listesini döndürür. Bu durumda, sorgu sonucu giriş sayısına bağlı olarak, tek bir sayfalandırılmış sorgu birden fazla sorgu kota tüketebilir. Örneğin, aşağıdaki örnekte sorgunun tek bir çalıştırma en fazla beş sorgu kota tüketebilir:

  ```azurecli-interactive
  az graph query -q 'project id, name, type' -top 5000
  ```

  ```azurepowershell-interactive
  Search-AzGraph -Query 'project id, name, type' -Top 5000
  ```

## <a name="still-get-throttled"></a>Hala kısıtlanmazsınız?

Ekibi, yukarıdaki önerileri uygulama sonra kaynaklarınızın azaltılıp, başvurun [ resourcegraphsupport@microsoft.com ](mailto:resourcegraphsupport@microsoft.com).

Bu ayrıntıları sağlayın:

- Belirli sürücü kullanım örneği ve iş gereksinimlerinize göre daha yüksek bir azaltma sınır.
- Kaç kaynak erişim hakkınız? Ne kadar olan tek bir sorgudan döndürülen?
- Kaynak türleri nelerdir, ilgileniyor musunuz?
- Sorgu desen nedir? Sorguları vb. Y saniyede x.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanımda dili bakın [başlangıç sorguları](../samples/starter.md).
- Bkz: Gelişmiş kullanır [Gelişmiş sorgular](../samples/advanced.md).
- Öğrenme [kaynakları keşfedin](explore-resources.md).