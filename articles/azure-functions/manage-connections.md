---
title: Azure işlevleri'nde bağlantılarını yönetme
description: Statik bağlantı istemcileri kullanarak Azure işlevleri'nde performans sorunlarını önlemek öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: glenga
ms.openlocfilehash: 9780f98be7d488f87e280a1f391f7cc536ce0f8c
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999576"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Azure işlevleri'nde bağlantılarını yönetme

Bir işlev uygulaması işlevlerde kaynakları paylaşma ve bu paylaşılan kaynakları arasında bağlantı, &mdash; HTTP bağlantıları, veritabanı bağlantıları ve depolama gibi Azure Hizmetleri. Birçok işlev eşzamanlı olarak çalışırken, kullanılabilir bağlantılar dışında çalıştırmak mümkündür. Bu makalede, işlevlerinizin gerçekten ihtiyacınız olandan daha fazla bağlantı kullanmaktan kaçınmak için kod açıklanmaktadır.

## <a name="connections-limit"></a>Bağlantı sayısı sınırı

Bir işlev uygulaması kısmen çalıştığı için kullanılabilir bağlantı sayısı sınırlıdır [Azure App Service korumalı](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Korumalı alan kodunuza uygular kısıtlamaları biri bir [bağlantısı, şu anda 300 sayısı için üst sınır](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Bu sınıra ulaştığınızda, İşlevler çalışma zamanı şu iletiyle bir günlük oluşturur: `Host thresholds exceeded: Connections`.

Sınırı aşan olasılığını zaman giden [ölçek denetleyicisi ekler işlevi uygulama örneği](functions-scale.md#how-the-consumption-plan-works) daha fazla isteklerini işlemek için. Her işlev uygulaması örneğinin, tümü 300 sınırından sayılır bağlantıları kullanarak birçok işlev aynı anda çalışabilir.

## <a name="use-static-clients"></a>Statik istemciler

Daha fazla gerekli bağlantı bulunduran önlemek için her işlev Çağırma ile yenilerini oluşturmak yerine istemci örnekleri yeniden kullanın. .NET istemcileri ister [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx), [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
), ve statik, tek bir istemci kullanıyorsanız, Azure depolama istemci bağlantıları yönetebilir.

Azure işlevleri uygulamada bir hizmete özgü istemcisi kullanarak izlenmesi gereken bazı Kılavuzlar şunlardır:

- **DO NOT** yeni bir istemci her işlev Çağırma ile oluşturun.
- **YAPMAK** her işlev çağrısı tarafından kullanılabilen statik, tek bir istemci oluşturabilir.
- **Göz önünde bulundurun** aynı hizmetin farklı işlevlere kullanırsanız, paylaşılan bir yardımcı sınıfta statik, tek bir istemci oluşturma.

## <a name="client-code-examples"></a>İstemci kod örnekleri

Bu bölüm, oluşturma ve işlev kodunuzu istemcilerden kullanmak için en iyi uygulamaları gösterir.

### <a name="httpclient-example-c"></a>HttpClient örneği (C#)

İşte bir örnek C# işlevini statik oluşturan kodu [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx):

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("https://example.com");
    // Rest of function
}
```

.NET ile ilgili karşılaşılan bir soru [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) olan "miyim olması disposing istemcim?" Genel olarak, size uygulayan nesneler dispose `IDisposable` bitirdiğinizde bunları kullanarak. Ancak bitti değildir çünkü statik istemci dispose yoksa işlev sona erdiğinde, kullanarak. Uygulamanızın süresi boyunca Canlı statik istemci kullanmanız gerekir.

### <a name="http-agent-examples-nodejs"></a>HTTP Aracısı örnekleri (Node.js)

Yerel yönetim seçenekleri, daha iyi bağlantıyı sağladığından, kullanmanız gereken [ `http.agent` ](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) yerel olmayan yöntemler yerine gibi sınıf `node-fetch` modülü. Bağlantı parametrelerini seçenekleri kullanarak yapılandırılmış `http.agent` sınıfı. Bkz: [yeni aracı (\[seçenekleri\])](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options) HTTP aracısıyla kullanılabilir ayrıntılı seçenekler.

Genel `http.globalAgent` tarafından kullanılan `http.request()` tüm ilgili değerlerinde bu değerleri vardır. Bağlantı sınırları işlevleri yapılandırmak için önerilen yöntem en fazla küresel olarak ayarlamaktır. Aşağıdaki örnek, işlev uygulaması için yuva sayısı ayarlar:

```js
http.globalAgent.maxSockets = 200;
```

 Aşağıdaki örnek, yalnızca bu istek için özel bir HTTP Aracısı ile yeni bir HTTP isteği oluşturur.

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

## <a name="sqlclient-connections"></a>SqlClient bağlantıları

İşlev kodunuzu SQL Server için .NET Framework veri sağlayıcısı kullanabilir ([SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)) SQL ilişkisel bir veritabanı bağlantısı yapma. Alttaki sağlayıcı Entity Framework gibi bir ADO.NET kullanan veri çerçeveler için de budur. Farklı [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) ve [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) bağlantıları, ADO.NET uygulayan varsayılan olarak bağlantı havuzu. Ancak, yine de bağlantılar dışında çalışabileceğinden, veritabanı bağlantılarını iyileştirmeniz gerekir. Daha fazla bilgi için [SQL Server Connection Pooling (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling).

> [!TIP]
> Bazı veri çerçeveleri gibi [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx), genellikle bağlantı dizeleri alma **ConnectionStrings** yapılandırma dosyasının. SQL veritabanı bağlantı dizeleri için bu durumda, açıkça eklemelidir **bağlantı dizeleri** koleksiyonu, işlev uygulaması ayarları hem de [local.settings.json dosyasında](functions-run-local.md#local-settings-file) yerel projenizdeki. Oluşturuyorsanız bir [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) işlev kodunuzu bağlantı dizesi değerindeki saklamalısınız **uygulama ayarları** diğer bağlantı.

## <a name="next-steps"></a>Sonraki adımlar

Statik istemciler önerilir nedeni hakkında daha fazla bilgi için bkz: [yanlış örnek oluşturma kötü modeli](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Daha fazla Azure işlevleri performans ipuçları için bkz: [Azure işlevleri'nin güvenilirliği ve performansı en iyi duruma getirme](functions-best-practices.md).
