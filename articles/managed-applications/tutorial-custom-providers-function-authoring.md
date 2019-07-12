---
title: Bir RESTful uç noktası için özel sağlayıcılar yazma
description: Bu öğreticide, bir RESTful uç noktası için özel sağlayıcılar yazmak nasıl üzerinden geçer. Bu istekleri ve yanıtları için desteklenen bir RESTful HTTP yöntemleri işlemek nasıl ayrıntıya gider.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 176e3b02cbda7577e306d86363cfe5b41335fb6e
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800039"
---
# <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Bir RESTful uç noktası için özel sağlayıcılar yazma

Özel sağlayıcılar azure'da iş akışlarını özelleştirmenizi sağlar. Özel bir sağlayıcı Azure arasında bir sözleşmedir ve `endpoint`. Bu öğreticide, özel bir sağlayıcı RESTful geliştirme sürecinde geçer `endpoint`. Azure özel sağlayıcıları ile alışkın değilseniz bkz [genel bakış'ta özel kaynak sağlayıcıları](./custom-providers-overview.md).

Bu öğreticide aşağıdaki adımlar ayrılır:

- Özel Eylemler ve özel kaynaklar ile çalışma
- Nasıl ayıracağınız depolamadaki özel kaynaklar
- Özel sağlayıcı RESTful yöntemleri destekler
- RESTful operations tümleştirin

Bu öğreticide, aşağıdaki öğreticiler oluşturacaksınız:

- [Azure işlevleri için Azure özel sağlayıcılar Kurulumu](./tutorial-custom-providers-function-setup.md)

> [!NOTE]
> Bu öğreticide, önceki öğreticide oluşturur. Bu öğreticideki adımlardan bazıları, yalnızca bir Azure işlevi özel sağlayıcıları ile çalışmak için Kurulum olmaması durumunda çalışır.

## <a name="working-with-custom-actions-and-custom-resources"></a>Özel Eylemler ve özel kaynaklar ile çalışma

Bu öğreticide, bizim Özel sağlayıcı için bir RESTful uç noktası olarak çalışmak için işlev güncelleştireceğiz. Azure'da temel bir RESTful belirtimi sonra kaynaklara ve işlemlere modellenir: PUT - yeni bir kaynak oluşturan GET - alır, mevcut bir kaynağı silme - (örnek), mevcut bir kaynağı kaldırır, POST - tetikleyici eylem ve GET (koleksiyon) - var olan tüm kaynakları listeler. Bu öğretici için biz Azure tablo depolama kullanacak ancak herhangi bir veritabanı veya depolama hizmeti çalışabilir.

## <a name="how-to-partition-custom-resources-in-storage"></a>Nasıl ayıracağınız depolamadaki özel kaynaklar

Bir RESTful hizmeti oluşturuyoruz depolamada oluşturulan kaynakları depolamak gerekir. Azure tablo depolaması için verilerimizi bölüm ve satır anahtarlarını oluşturmak ihtiyacımız var. Özel sağlayıcılar için sağlayıcıya özel veri bölümlendirilmelidir. Özel sağlayıcı için gelen bir istek gönderildiğinde, özel sağlayıcı ekler `x-ms-customproviders-requestpath` giden istek üst bilgisine `endpoint`.

örnek `x-ms-customproviders-requestpath` özel bir kaynak için üst bilgi:

```
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{myResourceType}/{myResourceName}
```

Yukarıdaki örneği temel alarak `x-ms-customproviders-requestpath` üst bilgi, biz oluşturabilirsiniz partitionKey ve rowKey aşağıdaki gibi depolama için:

Parametre | Şablon | Açıklama
---|---
partitionKey | '{subscriptionId}:{resourceGroupName}:{resourceProviderName}' | PartitionKey, verilerin nasıl bölümlendiğini ' dir. Çoğu durumda, özel sağlayıcı örneği tarafından verilerin bölümlere ayrılması gerekir.
RowKey | ' {myResourceType}: {myResourceName}' | RowKey verileri tek tek tanımlayıcısıdır. Çoğu zaman bu kaynak adıdır.

Ayrıca, biz de müşterilerimizin özel kaynak modellemek için yeni bir sınıf oluşturmanız gerekir. Bu öğreticide, ekleyeceğiz `CustomResource` tüm girilen verileri kabul eden bir genel sınıftır bizim işlev sınıfı:

```csharp
// Custom Resource Table Entity
public class CustomResource : TableEntity
{
    public string Data { get; set; }
}
```

Bu temel bir temel sınıf oluşturur `TableEntity`, verileri depolamak için kullanılır. `CustomResource` Sınıfından devralan iki özelliklerinden `TableEntity`: partitionKey ve rowKey.

## <a name="support-custom-provider-restful-methods"></a>Özel sağlayıcı RESTful yöntemleri destekler

> [!NOTE]
> Kodu doğrudan öğreticinin Kopyalamakta olduğunuz değil, yanıt içeriği geçerli JSON olmalıdır ve ayarlar `Content-Type` başlık olarak `application/json`.

Veri bölümlendirme sahibiz, özel kaynaklar ve özel eylemler için temel CRUD ve tetikleyici yöntemleri çıkış şimdi iskelesini. Özel sağlayıcılar bir proxy olarak davranmasına olduğundan, istek ve yanıt modellenir ve gereken RESTful tarafından işlenen `endpoint`. İzleyin temel RESTful işlemleri işlemek için kod parçacıklarını aşağıda:

### <a name="trigger-custom-action"></a>Özel eylemi tetikleme

Özel sağlayıcılar için özel bir eylem ile tetiklenen `POST` istekleri. Özel bir eylem, isteğe bağlı olarak bir dizi bir giriş parametrelerini içeren bir istek gövdesini kabul edebilir. Eylem, geri dönüş sonra bir yanıt signally eylem sonucu olarak başarılı veya başarısız olmadığını gerekir. Bu öğreticide, yöntemin ekleyeceğiz `TriggerCustomAction` bizim işlevi için:

```csharp
/// <summary>
/// Triggers a custom action with some side effect.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <returns>The http response result of the custom action.</returns>
public static async Task<HttpResponseMessage> TriggerCustomAction(HttpRequestMessage requestMessage)
{
    var myCustomActionRequest = await requestMessage.Content.ReadAsStringAsync();

    var actionResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    actionResponse.Content = myCustomActionRequest != string.Empty ? 
        new StringContent(JObject.Parse(myCustomActionRequest).ToString(), System.Text.Encoding.UTF8, "application/json") :
        null;
    return actionResponse;
}
```

`TriggerCustomAction` Yöntemi, bir gelen isteği kabul eder ve yalnızca konsola yansıtır geri bir başarı durum kodu ile yanıt. 

### <a name="create-custom-resource"></a>Özel kaynak oluşturma

Özel sağlayıcılar için özel bir kaynak aracılığıyla oluşturulan `PUT` istekleri. Özel bir sağlayıcı özel kaynak için özellikler kümesi içeren bir JSON istek gövdesini kabul eder. Azure'da bir RESTful modeli kaynakları izleyin. Bir kaynak oluşturmak için kullanılan aynı istek URL'si almak ve kaynağı silme olmalıdır. Bu öğreticide, yöntemin ekleyeceğiz `CreateCustomResource` yeni kaynaklar oluşturmak için:

```csharp
/// <summary>
/// Creates a custom resource and saves it to table storage.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="azureResourceId">The parsed Azure resource Id.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the created custom resource.</returns>
public static async Task<HttpResponseMessage> CreateCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, ResourceId azureResourceId, string partitionKey, string rowKey)
{
    // Adds the Azure top-level properties.
    var myCustomResource = JObject.Parse(await requestMessage.Content.ReadAsStringAsync());
    myCustomResource["name"] = azureResourceId.Name;
    myCustomResource["type"] = azureResourceId.FullResourceType;
    myCustomResource["id"] = azureResourceId.Id;

    // Save the resource into storage.
    var insertOperation = TableOperation.InsertOrReplace(
        new CustomResource
        {
            PartitionKey = partitionKey,
            RowKey = rowKey,
            Data = myCustomResource.ToString(),
        });
    await tableStorage.ExecuteAsync(insertOperation);

    var createResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    createResponse.Content = new StringContent(myCustomResource.ToString(), System.Text.Encoding.UTF8, "application/json");
    return createResponse;
}
```

`CreateCustomResource` Yöntemi güncelleştirmeleri Azure belirli alanları içerecek biçimde gelen istek: `id`, `name`, ve `type`. Bunlar arasında Azure Hizmetleri tarafından kullanılan en üst düzey özellikleridir. Bunlar, Azure İlkesi, Azure Resource Manager şablonları ve Azure etkinlik günlükleri gibi başka hizmetler ile tümleştirmek özel sağlayıcı olanak sağlar.

Özellik | Örnek | Açıklama
---|---|---
name | '{myCustomResourceName}' | Özel kaynak adı.
türü | }' | Kaynak türü ad alanı.
id | '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>{resourceTypeName} / {myCustomResourceName}' | Kaynak kimliği.

Özellikleri eklemenin yanı sıra, biz de belgeyi Azure tablo depolama alanına kaydedin. 

### <a name="retrieve-custom-resource"></a>Özel kaynak alma

Özel sağlayıcılar için özel bir kaynak üzerinden alınan `GET` istekleri. Özel sağlayıcı olacak *değil* JSON istek gövdesini kabul edin. Durumunda, `GET` istekleri **uç nokta** kullanması gereken `x-ms-customproviders-requestpath` zaten oluşturulan kaynak dönmek için üst bilgi. Bu öğreticide, yöntemin ekleyeceğiz `RetrieveCustomResource` mevcut kaynakları almak için:

```csharp
/// <summary>
/// Retrieves a custom resource.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the existing custom resource.</returns>
public static async Task<HttpResponseMessage> RetrieveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    var retrieveResponse = requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NotFound);

    retrieveResponse.Content = existingCustomResource != null ?
            new StringContent(existingCustomResource.Data, System.Text.Encoding.UTF8, "application/json"):
            null;
    return retrieveResponse;
}
```

Azure'da kaynakları bir RESTful modeli izlemelidir. Değilse kaynak oluşturulmuş istek URL'si de kaynak döndürmelidir bir `GET` istek gerçekleştirilir.

### <a name="remove-custom-resource"></a>Özel kaynak Kaldır

Özel sağlayıcılar için özel bir kaynak aracılığıyla kaldırılır `DELETE` istekleri. Özel sağlayıcı olacak *değil* JSON istek gövdesini kabul edin. Durumunda, `DELETE` istekleri **uç nokta** kullanması gereken `x-ms-customproviders-requestpath` zaten oluşturulan kaynak silmek için üst bilgi. Bu öğreticide, yöntemin ekleyeceğiz `RemoveCustomResource` mevcut kaynakları kaldırmak için:

```csharp
/// <summary>
/// Removes an existing custom resource.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the result of the delete.</returns>
public static async Task<HttpResponseMessage> RemoveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    if (existingCustomResource != null) {
        var deleteOperation = TableOperation.Delete(existingCustomResource);
        await tableStorage.ExecuteAsync(deleteOperation);
    }

    return requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NoContent);
}
```

Azure'da kaynakları bir RESTful modeli izlemelidir. Varsa kaynak oluşturulmuş istek URL'si de kaynak silmelisiniz bir `DELETE` istek gerçekleştirilir.

### <a name="list-all-custom-resources"></a>Tüm özel kaynaklar listesi

Özel sağlayıcılar için koleksiyonda mevcut özel kaynakların listesini numaralandırılabilir `GET` istekleri. Özel sağlayıcı olacak *değil* JSON istek gövdesini kabul edin. Koleksiyon söz konusu olduğunda `GET` istekleri `endpoint` kullanması gereken `x-ms-customproviders-requestpath` zaten oluşturulmuş olan kaynaklar numaralandırmak için üst bilgi. Bu öğreticide, yöntemin ekleyeceğiz `EnumerateAllCustomResources` mevcut kaynakları numaralandırılamadı.

```csharp
/// <summary>
/// Enumerates all the stored custom resources for a given type.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="resourceType">The resource type of the enumeration.</param>
/// <returns>The http response containing a list of resources stored under 'value'.</returns>
public static async Task<HttpResponseMessage> EnumerateAllCustomResources(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string resourceType)
{
    // Generate upper bound of the query.
    var rowKeyUpperBound = new StringBuilder(resourceType);
    rowKeyUpperBound[rowKeyUpperBound.Length - 1]++;

    // Create the enumeration query.
    var enumerationQuery = new TableQuery<CustomResource>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, partitionKey),
            TableOperators.And,
            TableQuery.CombineFilters(
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, resourceType),
                TableOperators.And,
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, rowKeyUpperBound.ToString()))));
    
    var customResources = (await tableStorage.ExecuteQuerySegmentedAsync(enumerationQuery, null))
        .ToList().Select(customResource => JToken.Parse(customResource.Data));

    var enumerationResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    enumerationResponse.Content = new StringContent(new JObject(new JProperty("value", customResources)).ToString(), System.Text.Encoding.UTF8, "application/json");
    return enumerationResponse;
}
```

> [!NOTE]
> Satır anahtarı "startswith" sorgu dizeleri için gerçekleştirmek için Azure tablo sözdizimi değerinden büyüktür ve küçüktür. 

Var olan tüm kaynakları listelemek için şu kaynakları bizim Özel sağlayıcı bölümü altında mevcut olmasını sağlar bir Azure Tablo sorgusu oluştur. Sorgu sonra satır anahtarı ile aynı başlatan denetimleri `{myResourceType}`.

## <a name="integrate-restful-operations"></a>RESTful operations tümleştirin

RESTful tüm yöntemleri için işlev eklendikten sonra ana güncelleştirmek için `Run` farklı gerisini işlevleri çağırmak için yöntem ister:

```csharp
/// <summary>
/// Entry point for the Azure Function webhook and acts as the service behind a custom provider.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="log">The logger.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <returns>The http response for the custom Azure API.</returns>
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log, CloudTable tableStorage)
{
    // Get the unique Azure request path from request headers.
    var requestPath = req.Headers.GetValues("x-ms-customproviders-requestpath").FirstOrDefault();

    if (requestPath == null)
    {
        var missingHeaderResponse = req.CreateResponse(HttpStatusCode.BadRequest);
        missingHeaderResponse.Content = new StringContent(
            new JObject(new JProperty("error", "missing 'x-ms-customproviders-requestpath' header")).ToString(),
            System.Text.Encoding.UTF8, 
            "application/json");
    }

    log.LogInformation($"The Custom Provider Function received a request '{req.Method}' for resource '{requestPath}'.");

    // Determines if it is a collection level call or action.
    var isResourceRequest = requestPath.Split('/').Length % 2 == 1;
    var azureResourceId = isResourceRequest ? 
        ResourceId.FromString(requestPath) :
        ResourceId.FromString($"{requestPath}/");

    // Create the Partition Key and Row Key
    var partitionKey = $"{azureResourceId.SubscriptionId}:{azureResourceId.ResourceGroupName}:{azureResourceId.Parent.Name}";
    var rowKey = $"{azureResourceId.FullResourceType.Replace('/', ':')}:{azureResourceId.Name}";

    switch (req.Method)
    {
        // Action request for an custom action.
        case HttpMethod m when m == HttpMethod.Post && !isResourceRequest:
            return await TriggerCustomAction(
                requestMessage: req);

        // Enumerate request for all custom resources.
        case HttpMethod m when m == HttpMethod.Get && !isResourceRequest:
            return await EnumerateAllCustomResources(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                resourceType: rowKey);

        // Retrieve request for a custom resource.
        case HttpMethod m when m == HttpMethod.Get && isResourceRequest:
            return await RetrieveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Create request for a custom resource.
        case HttpMethod m when m == HttpMethod.Put && isResourceRequest:
            return await CreateCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                azureResourceId: azureResourceId,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Remove request for a custom resource.
        case HttpMethod m when m == HttpMethod.Delete && isResourceRequest:
            return await RemoveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Invalid request received.
        default:
            return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}
``` 

Güncelleştirilmiş `Run` yöntemi artık içerecektir `tableStorage` Azure tablo depolama için eklenen bağlama giriş. Yöntemin ilk kısmı artık okuyacaksa `x-ms-customproviders-requestpath` üstbilgi ve kullanım `Microsoft.Azure.Management.ResourceManager.Fluent` kitaplığı değeri olarak bir kaynak kimliği ayrıştırılamıyor `x-ms-customproviders-requestpath` Üstbilgi özel sağlayıcı tarafından gönderilen ve gelen istek yolunu belirtir. Ayrıştırılmış kaynak Kimliğini kullanarak, biz şimdi partitionKey ve rowKey özel kaynakları depolamak veya arama verileri için oluşturabilirsiniz.

Sınıflar ve yöntemler eklemenin yanı sıra kullanarak güncelleştirmek ihtiyacımız işlevi için yöntem. Dosyanın üst kısmına aşağıdakileri ekleyin:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.WindowsAzure.Storage"
#r "../bin/Microsoft.Azure.Management.ResourceManager.Fluent.dll"

using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Configuration;
using System.Text;
using System.Threading;
using System.Globalization;
using System.Collections.Generic;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Bu öğreticinin herhangi bir noktasında kayıp geldiyseniz, tamamlanan kodu örneği bulabilirsiniz [özel sağlayıcı C# RESTful uç nokta başvurusu](./reference-custom-providers-csharp-endpoint.md). Sonraki öğreticilerde kullanılacağından işlev tetiklemek için kullanılan işlev URL'sini işlevi tamamlandıktan sonra kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

Biz bu makalede, Azure özel sağlayıcı ile çalışmak için bir RESTful uç noktası yazılan `endpoint`. Özel bir sağlayıcı oluşturma hakkında bilgi edinmek için sonraki makaleye gidin.

- [Öğretici: Özel bir sağlayıcı oluşturma](./tutorial-custom-providers-create.md)
