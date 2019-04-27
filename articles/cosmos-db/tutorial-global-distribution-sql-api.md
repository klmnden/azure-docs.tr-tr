---
title: SQL API'si için Azure Cosmos DB genel dağıtım Öğreticisi
description: SQL API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlamayı öğrenin.
author: rimman
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 19e0dd8a60155272f2e338c760db409d20a9f755
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60747634"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-sql-api"></a>SQL API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlama

Bu makalede, Azure portalını kullanarak Azure Cosmos DB genel dağıtımını ayarlama ve sonra SQL API’sini kullanarak bağlanma işlemlerinin nasıl yapılacağı gösterilmektedir.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * [SQL API’lerini](sql-api-introduction.md) kullanarak genel dağıtımı yapılandırma

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-sql-api"></a>SQL API’sini kullanarak tercih edilen bir bölgeye bağlanma

[Genel dağıtımdan](distribute-data-globally.md) yararlanmak için istemci uygulamaları, belge işlemlerini gerçekleştirmek için kullanılacak bölgelerin tercihe göre sıralanmış listesini belirtebilir. Bağlantı ilkesi ayarlanarak bu yapılabilir. Azure Cosmos DB hesap yapılandırmasına, geçerli bölgesel kullanılabilirliğe ve belirtilen tercih listesine göre yazma ve okuma işlemlerini gerçekleştirmek için SQL SDK tarafından en iyi uç nokta seçilir.

SQL SDK’ları kullanılarak bağlantı başlatırken bu tercih listesi belirtilir. SDK’lar, Azure bölgelerinin sıralı listesinde isteğe bağlı "PreferredLocations" parametresini kabul eder.

SDK, tüm yazma işlemlerini otomatik olarak geçerli yazma bölgesine gönderir.

Tüm yazma işlemleri, PreferredLocations listesindeki ilk kullanılabilir bölgeye gönderilir. İstek başarısız olursa, istemci listedeki sonraki bölgeyi dener ve bu şekilde devam eder.

SDK’lar yalnızca PreferredLocations listesinde belirtilen bölgelerden okuma işlemi yapmaya çalışır. Bu nedenle, örneğin, Veritabanı Hesabı dört bölgede kullanılabiliyorsa ancak istemci, PreferredLocations için yalnızca iki okuma (yazma olmayan) bölgesi belirtiyorsa, PreferredLocations listesinde belirtilmeyen okuma bölgesinden okuma işlemi yapılmaz. PreferredLocations listesinde belirtilen okuma bölgeleri kullanılamıyorsa, okuma işlemleri yazma bölgesinden yapılır.

Uygulama, SDK 1.8 ve üzeri sürümlerde kullanılabilir olan iki WriteEndpoint ve ReadEndpoint özelliğini denetleyerek SDK tarafından seçilen geçerli yazma uç noktasını ve okuma uç noktasını doğrulayabilir.

PreferredLocations özelliği ayarlanmazsa, tüm istekler geçerli yazma bölgesinden işlenir.

## <a name="net-sdk"></a>.NET SDK
SDK herhangi bir kod değişikliği olmadan kullanılabilir. Bu durumda SDK hem okuma hem de yazma işlemlerini otomatik olarak geçerli yazma bölgesine yönlendirir.

.NET SDK’nın 1.8 ve sonraki sürümlerinde, DocumentClient oluşturucusu için ConnectionPolicy parametresi, Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations adlı bir özelliğe sahiptir. Bu özellik `<string>` Koleksiyonu türünde olup bölge adlarının listesini içermelidir. Dize değerleri, [Azure Bölgeleri][regions] sayfasındaki Bölge Adı sütununa göre biçimlendirilir ve ilk karakterden önce veya son karakterden sonra bir boşluk bulunmaz.

Geçerli yazma ve okuma uç noktaları sırasıyla DocumentClient.WriteEndpoint ve DocumentClient.ReadEndpoint içinde kullanılabilir.

> [!NOTE]
> Uç noktaların URL’leri, uzun ömürlü sabitler olarak değerlendirilmemelidir. Bu noktada hizmet bunları güncelleştirebilir. SDK bu değişikliği otomatik olarak işler.
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript ve Python SDK’ları
SDK herhangi bir kod değişikliği olmadan kullanılabilir. Bu durumda SDK hem okuma hem de yazma işlemlerini otomatik olarak geçerli yazma bölgesine yönlendirir.

Her bir SDK’nın 1.8 ve sonraki sürümlerinde, DocumentClient oluşturucusu için ConnectionPolicy parametresi, DocumentClient.ConnectionPolicy.PreferredLocations adlı yeni bir özelliğe sahiptir. Bu, bölge adlarının listesini alan bir dize dizisidir. Adlar, [Azure Bölgeleri][regions] sayfasındaki Bölge Adı sütununa göre biçimlendirilir. AzureDocuments.Regions kolaylık nesnesindeki önceden tanımlanmış sabitleri de kullanabilirsiniz

Geçerli yazma ve okuma uç noktaları sırasıyla DocumentClient.getWriteEndpoint ve DocumentClient.getReadEndpoint içinde kullanılabilir.

> [!NOTE]
> Uç noktaların URL’leri, uzun ömürlü sabitler olarak değerlendirilmemelidir. Bu noktada hizmet bunları güncelleştirebilir. SDK bu değişikliği otomatik olarak işler.
>
>

Aşağıda, NodeJS/Javascript için bir kod örneği verilmiştir. Python ve Java aynı düzeni izler.

```JavaScript
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a>REST
Birden çok bölgede bir veritabanı hesabı kullanılabilir duruma getirildiğinde istemciler aşağıdaki URI’de bir GET isteği gerçekleştirerek bu hesabın kullanılabilirliğini sorgulayabilir.

    https://{databaseaccount}.documents.azure.com/

Hizmet, bölgelerin listesini ve çoğaltmalar için karşılık gelen Azure Cosmos DB uç nokta URI’lerini döndürür. Yanıtta geçerli yazma bölgesi belirtilir. Daha sonra istemci aşağıdaki gibi diğer tüm REST API istekleri için uygun uç noktayı seçebilir.

Örnek yanıt

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* Tüm PUT, POST ve DELETE istekleri, belirtilen yazma URI’sine gitmelidir
* Tüm GET istekleri ve diğer salt okunur istekler (örneğin, sorgular), istemcinin tercih ettiği herhangi bir uç noktaya gidebilir

Salt okunur bölgelere yönelik yazma istekleri, HTTP hata kodu 403 (“Yasak”) ile başarısız olur.

İstemcinin ilk bulma aşamasından sonra yazma bölgesi değişirse, önceki yazma bölgesine yönelik sonraki yazma işlemleri, HTTP hata kodu 403 (“Yasak”) ile başarısız olur. İstemci daha sonra güncelleştirilmiş yazma bölgesini almak için bölgelerin listesini ALIR (GET).

Hepsi bu kadar. Böylece bu öğretici tamamlanmış olur. [Azure Cosmos DB’deki tutarlılık düzeyleri](consistency-levels.md) bölümünü okuyarak genel olarak çoğaltılan hesabınızın tutarlılığının nasıl yönetileceğini öğrenebilirsiniz. Ayrıca genel veritabanı çoğaltmasının Azure Cosmos DB’de nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Cosmos DB ile verileri genel olarak dağıtma](distribute-data-globally.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * SQL API’lerini kullanarak genel dağıtımı yapılandırma

Artık Azure Cosmos DB yerel öykünücüsünü kullanarak yerel olarak geliştirme konusunda bilgi almak için sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [Öykünücü ile yerel olarak geliştirme](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

