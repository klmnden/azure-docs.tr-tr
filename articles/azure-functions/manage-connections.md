---
title: Azure işlevleri'nde bağlantılarını yönetme
description: Statik bağlantı istemcileri kullanarak Azure işlevleri'nde performans sorunlarını önlemek öğrenin.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: glenga
ms.openlocfilehash: 86727355d36e16f5b3c7edef8ce666fb27805a80
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346309"
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

## <a name="httpclient-code-example"></a>HttpClient kod örneği

İşte bir örnek bir statik oluşturur ve işlev kodunu [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx):

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

.NET ile ilgili karşılaşılan bir soru [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) olan "miyim olması disposing istemcim?" Genel olarak, size uygulayan nesneler dispose `IDisposable` bitirdiğinizde bunları kullanarak. Ancak bitti değildir çünkü statik istemci dispose yoksa işlev sona erdiğinde, kullanarak. Uygulamanızın süresi boyunca Canlı statik istemci kullanmanız gerekir.

## <a name="documentclient-code-example"></a>DocumentClient kod örneği

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