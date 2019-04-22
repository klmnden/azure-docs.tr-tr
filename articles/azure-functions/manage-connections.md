---
title: Azure işlevleri'nde bağlantıları yönetme
description: Statik bağlantı istemcileri kullanarak Azure işlevleri'nde performans sorunlarını önlemek öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 02/25/2018
ms.author: glenga
ms.openlocfilehash: 4e9bd4e9ea467446c2814cdb8956a40b1503b027
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59784934"
---
# <a name="manage-connections-in-azure-functions"></a>Azure işlevleri'nde bağlantıları yönetme

Bir işlev uygulaması işlevlerde kaynakları paylaşır. Bu paylaşılan kaynaklar arasında bağlantılar şunlardır: HTTP bağlantıları, veritabanı bağlantıları ve Azure depolama gibi hizmetleri. Birçok işlev eşzamanlı olarak çalışırken, kullanılabilir bağlantılar dışında çalıştırmak mümkündür. Bu makalede, gerekenden daha fazla bağlantı kullanmaktan kaçınmak için İşlevler kodu açıklanmaktadır.

## <a name="connection-limit"></a>Bağlantı sınırı

Bir işlev uygulaması kısmen çalıştığı için kullanılabilir bağlantı sayısı sınırlı bir [korumalı ortamda](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Korumalı alan kodunuza uygular kısıtlamaları biri bir [(şu anda 600 etkin bağlantıları ve 1.200 toplam bağlantıları) bağlantı sayısı için üst sınır](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits) örnek başına. Bu sınıra ulaştığınızda, İşlevler çalışma zamanı şu iletiyle bir günlük oluşturur: `Host thresholds exceeded: Connections`.

Örnek başına sınırdır.  Zaman [ölçek denetleyicisi ekler işlevi uygulama örneği](functions-scale.md#how-the-consumption-and-premium-plans-work) daha fazla isteklerini işlemek için her örnek, bir bağımsız bağlantı sınırı vardır. Genel bağlantı sınırı yoktur ve tüm etkin örnekler arasında 600'den çok fazla etkin bağlantılar olabilir anlamına gelir.

Sorunlarını giderirken, işlev uygulamanız için Application Insights etkinleştirdiğinizden emin olun. Application Insights, işlev uygulamalarınızı yürütmeleri gibi ölçümlerini görüntülemenizi sağlar. Daha fazla bilgi için [Application Insights'da telemetriyi görüntüleyin](functions-monitoring.md#view-telemetry-in-application-insights).  

## <a name="static-clients"></a>Statik istemciler

Daha fazla gerekli bağlantı bulunduran önlemek için her işlev Çağırma ile yenilerini oluşturmak yerine istemci örnekleri yeniden kullanın. İstemci bağlantıları için işlevinizi yazabilir herhangi bir dilde yeniden öneririz. Örneğin, .NET istemcileri gibi [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx), [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
), ve statik, tek bir istemci kullanıyorsanız, Azure depolama istemci bağlantıları yönetebilir.

Azure işlevleri uygulamada bir hizmete özgü istemci kullandığınız izlenmesi gereken bazı Kılavuzlar şunlardır:

- *Sağlamadığı* yeni bir istemci her işlev Çağırma ile oluşturun.
- *Yapmak* her işlev çağrısını kullanabilirsiniz statik, tek bir istemci oluşturun.
- *Göz önünde bulundurun* aynı hizmetin farklı işlevlere kullanırsanız, paylaşılan bir yardımcı sınıfta statik, tek bir istemci oluşturma.

## <a name="client-code-examples"></a>İstemci kod örnekleri

Bu bölüm, oluşturma ve işlev kodunuzu istemcilerden kullanmak için en iyi uygulamaları gösterir.

### <a name="httpclient-example-c"></a>HttpClient örneği (C#)

İşte bir örnek C# işlevini statik oluşturan kodu [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) örneği:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("https://example.com");
    // Rest of function
}
```

Karşılaşılan bir soru hakkında [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) "miyim dispose istemcim?",. NET'te olduğu Genel olarak, size uygulayan nesneleri atmak `IDisposable` bitirdiğinizde bunları kullanarak. Ancak bitti değildir çünkü statik istemcisini dispose yoksa işlev sona erdiğinde, kullanarak. Uygulamanızın süresi boyunca Canlı statik istemci kullanmanız gerekir.

### <a name="http-agent-examples-javascript"></a>HTTP Aracısı örnekleri (JavaScript)

Yerel yönetim seçenekleri, daha iyi bağlantıyı sağladığından, kullanmanız gereken [ `http.agent` ](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) yerel olmayan yöntemler yerine gibi sınıf `node-fetch` modülü. Bağlantı parametrelerini yapılandırılır seçeneklerde `http.agent` sınıfı. Ayrıntılı seçenekleri için kullanılabilir HTTP aracısıyla bkz [yeni aracı (\[seçenekleri\])](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options).

Genel `http.globalAgent` sınıfı tarafından kullanılan `http.request()` tüm ilgili değerlerinde bu değerleri vardır. Bağlantı sınırları işlevleri yapılandırmak için önerilen yöntem en fazla küresel olarak ayarlamaktır. Aşağıdaki örnek, işlev uygulaması için yuva sayısı ayarlar:

```js
http.globalAgent.maxSockets = 200;
```

 Aşağıdaki örnek, yalnızca bu istek için özel bir HTTP Aracısı ile yeni bir HTTP isteği oluşturur:

```js
var http = require('http');
var httpAgent = new http.Agent();
httpAgent.maxSockets = 200;
options.agent = httpAgent;
http.request(options, onResponseCallback);
```

### <a name="documentclient-code-example-c"></a>DocumentClient kod örneği (C#)

[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) Azure Cosmos DB örneğine bağlanır. Azure Cosmos DB belgeleri önerir, [tekil Azure Cosmos DB istemci uygulama ömrü boyunca kullanma](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage). Aşağıdaki örnek, bir işlevde yapmak için bir desen gösterir:

```cs
#r "Microsoft.Azure.Documents.Client"
using Microsoft.Azure.Documents.Client;

private static Lazy<DocumentClient> lazyClient = new Lazy<DocumentClient>(InitializeDocumentClient);
private static DocumentClient documentClient => lazyClient.Value;

private static DocumentClient InitializeDocumentClient()
{
    // Perform any initialization here
    var uri = new Uri("example");
    var authKey = "authKey";
    
    return new DocumentClient(uri, authKey);
}

public static async Task Run(string input)
{
    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("database", "collection");
    object document = new { Data = "example" };
    await documentClient.UpsertDocumentAsync(collectionUri, document);
    
    // Rest of function
}
```

### <a name="cosmosclient-code-example-javascript"></a>CosmosClient kod örneği (JavaScript)
[CosmosClient](/javascript/api/@azure/cosmos/cosmosclient) Azure Cosmos DB örneğine bağlanır. Azure Cosmos DB belgeleri önerir, [tekil Azure Cosmos DB istemci uygulama ömrü boyunca kullanma](../cosmos-db/performance-tips.md#sdk-usage). Aşağıdaki örnek, bir işlevde yapmak için bir desen gösterir:

```javascript
const cosmos = require('@azure/cosmos');
const endpoint = process.env.COSMOS_API_URL;
const masterKey = process.env.COSMOS_API_KEY;
const { CosmosClient } = cosmos;

const client = new CosmosClient({ endpoint, auth: { masterKey } });
// All function invocations also reference the same database and container.
const container = client.database("MyDatabaseName").container("MyContainerName");

module.exports = async function (context) {
    const { result: itemArray } = await container.items.readAll().toArray();
    context.log(itemArray);
}
```

## <a name="sqlclient-connections"></a>SqlClient bağlantıları

İşlev kodunuzu .NET Framework veri sağlayıcısı, SQL Server için kullanabilirsiniz ([SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)) SQL ilişkisel bir veritabanı bağlantısı yapma. Alttaki sağlayıcı ADO.NET üzerinde gibi kullanan veri çerçeveler için de budur [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Farklı [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) ve [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) bağlantıları, ADO.NET uygulayan varsayılan olarak bağlantı havuzu. Ancak yine de bağlantılar dışında çalışabileceğinden, veritabanı bağlantılarını iyileştirmeniz gerekir. Daha fazla bilgi için [SQL Server Connection Pooling (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling).

> [!TIP]
> Entity Framework gibi bazı veri çerçeveleri genellikle bağlantı dizeleri alma **ConnectionStrings** yapılandırma dosyasının. SQL veritabanı bağlantı dizeleri için bu durumda, açıkça eklemelidir **bağlantı dizeleri** koleksiyonu, işlev uygulaması ayarları hem de [local.settings.json dosyasında](functions-run-local.md#local-settings-file) yerel projenizdeki. Örneği oluşturuyorsanız [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) işlev kodunuzu bağlantı dizesi değerindeki saklamalısınız **uygulama ayarları** diğer bağlantı.

## <a name="next-steps"></a>Sonraki adımlar

Statik istemciler neden öneririz hakkında daha fazla bilgi için bkz. [yanlış örnek oluşturma kötü modeli](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Daha fazla Azure işlevleri performans ipuçları için bkz: [Azure işlevleri'nin güvenilirliği ve performansı en iyi duruma getirme](functions-best-practices.md).
