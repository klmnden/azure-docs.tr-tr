---
title: Azure işlevleri bağlantıları yönetme
description: Statik bağlantı istemcileri kullanarak Azure işlevleri performans sorunlarını önlemek öğrenin.
services: functions
documentationcenter: ''
author: tdykstra
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: tdykstra
ms.openlocfilehash: 4ea2b033d8d67dd3c921fb833462605ba0aeb687
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655376"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Azure işlevleri bağlantıları yönetme

Bir işlev uygulaması işlevlerde paylaşmak kaynakları ve bağlantıları arasında paylaşılan kaynaklarla olan &mdash; HTTP bağlantıları, veritabanı bağlantılarını ve depolama alanı gibi Azure hizmetlerine bağlantıları. Birçok işlevini eşzamanlı olarak çalıştırırken dışında kullanılabilir bağlantıları çalıştırmak mümkündür. Bu makalede, aslında gerekenden daha fazla bağlantı kullanmaktan kaçının işlevlerinizi kod açıklanmaktadır.

## <a name="connections-limit"></a>Bağlantı sınırı

Bir işlev uygulaması kısmen çalıştığı için kullanılabilir bağlantı sayısı sınırlıdır [Azure App Service sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Korumalı alan kodunuz üzerinde uygular kısıtlamaları biri bir [cap bağlantıları, şu anda 300 sayısı](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Bu sınıra ulaştığınızda, işlevleri çalışma zamanı aşağıdaki iletiyle bir günlük oluşturur: `Host thresholds exceeded: Connections`.

Sınırı aşan olasılığını artırmak [ölçek denetleyicisi ekler işlevi app örnekleri](functions-scale.md#how-the-consumption-plan-works). Her işlev uygulama örneğini işlevleri birçok kez aynı anda çağırma ve tüm bu işlevlerin 300 sınırından sayılır bağlantılar kullanıyor olabilirsiniz.

## <a name="use-static-clients"></a>Statik istemciler kullanın

Gerekli'den daha fazla bağlantı bulunduran önlemek için her işlev çağrısını yenileriyle oluşturmak yerine istemci örnekleri yeniden kullanabilirsiniz. .NET istemcileri ister `HttpClient`, `DocumentClient`, ve tek, statik bir istemci kullanırsanız, Azure Storage istemci bağlantıları yönetebilir.

Azure işlevleri uygulamada bir hizmete özgü istemcisi kullanarak izlemeniz gereken bazı yönergeler şunlardır:

- **SAĞLAMADIĞI** yeni bir istemci her işlev çağrısını ile oluşturun.
- **YAPMAK** her işlev çağrısı tarafından kullanılan bir tek, statik istemci oluşturun.
- **Göz önünde bulundurun** aynı hizmetin farklı işlevler kullanıyorsanız, tek, statik bir istemci bir paylaşılan yardımcı sınıfı oluşturma.

## <a name="httpclient-code-example"></a>HttpClient kod örneği

Statik oluşturur işlevi kod örneği `HttpClient`:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

.NET hakkında bir soru `HttpClient` olan "ı olması atma my istemci?" Genel olarak, uygulayan nesneler dispose `IDisposable` bittiğinde bunları kullanarak. Ancak bitti değil çünkü bir statik istemci dispose yok işlevi sona erdiğinde kullanma. Uygulamanızı süresince Canlı statik istemci istiyor.

## <a name="documentclient-code-example"></a>DocumentClient kod örneği

`DocumentClient` Cosmos DB örneğine bağlanır. Cosmos DB belgelerine önerir, [bir singleton Azure Cosmos DB istemci uygulamanızın ömrü boyunca kullanın](https://docs.microsoft.com/en-us/azure/cosmos-db/performance-tips#sdk-usage). Aşağıdaki örnek, bir işlevde yapmak için bir desen gösterir.

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

## <a name="next-steps"></a>Sonraki adımlar

Statik istemciler neden hakkında daha fazla bilgi için önerilen, bkz: [hatalı örneklemesi antipattern](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Daha fazla Azure işlevleri performans ipuçları için bkz: [Azure işlevleri güvenilirliğini ve performansını en iyi duruma getirme](functions-best-practices.md).