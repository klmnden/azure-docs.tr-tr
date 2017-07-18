---
title: "Azure İşlevleri ve Cosmos DB’yi kullanarak yapılandırılmamış verileri depolama"
description: "Azure İşlevleri ve Cosmos DB’yi kullanarak yapılandırılmamış verileri depolama"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, Cosmos DB, dinamik işlem, sunucusuz mimari"
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/08/2017
ms.author: rachelap
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 492c916a493bb8d5c5415fc517506e5c1ccffc56
ms.contentlocale: tr-tr
ms.lasthandoff: 07/10/2017

---
# <a name="store-unstructured-data-using-azure-functions-and-cosmos-db"></a>Azure İşlevleri ve Cosmos DB’yi kullanarak yapılandırılmamış verileri depolama

Azure Cosmos DB, yapılandırılmamış verileri ve JSON verilerini depolamanın harika bir yoludur. Cosmos DB, Azure İşlevleri ile birlikte kullanıldığında verilerin ilişkisel bir veritabanında depolanmasına göre çok daha az kodla verileri hızlı ve kolay bir şekilde depolar.

Bu öğreticide, Cosmos DB belgesinde yapılandırılmamış verileri depolayan bir Azure İşlevi oluşturmak için Azure Portalı’nın nasıl kullanılacağı gösterilmiştir. 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-a-function"></a>İşlev oluşturma

`MyTaskList` adında yeni bir C# genel Web Kancası oluşturun.

1. Mevcut işlevler listenizi genişletin ve yeni bir işlev oluşturmak için + işaretine tıklayın
1. GenericWebHook-CSharp’ı seçin ve `MyTaskList` olarak adlandırın

![Yeni C# Genel Web Kancası İşlev Uygulaması Ekleme](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-new-functionapp.png)

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

Bir Azure işlevinde, bir tetikleyici ve herhangi bir sayıda girdi veya çıktı bağlaması olabilir. Bu örnekte, bir HTTP isteği tetikleyicisini ve çıktı bağlaması olarak Cosmos DB belgesini kullanacağız.

1. İşlevin tetikleyicisini ve bağlamalarını görüntülemek veya değiştirmek için işlevin *Tümleştir* sekmesine tıklayın.
1. Sayfanın üst sağ tarafında bulunan *Yeni Çıktı* bağlantısını seçin.

Not: HTTP İsteği tetikleyicisi zaten yapılandırılmıştır, ancak Cosmos DB belge bağlamasını eklemeniz gerekir.

![Yeni Cosmos DB çıktı bağlamasını ekleme](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

1. Bağlamayı oluşturmak için gerekli bilgileri girin. Değerleri belirlemek için aşağıdaki tabloyu kullanın.

![Cosmos DB çıktı bağlamasını yapılandırma](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

|  Alan | Değer  |
|---|---|
| Belge parametre adı | Kodda Cosmos DB nesnesine başvuran ad |
| Veritabanı adı | Belgelerin kaydedileceği veritabanının adı |
| Koleksiyon adı | Cosmos DB veritabanları gruplandırmasının adı |
| Cosmos DB’nin ve koleksiyonun sizin için oluşturulmasını ister misiniz? | Evet veya Hayır |
| Cosmos DB hesabı bağlantısı | Cosmos DB veritabanını gösteren bağlantı dizesi |

Cosmos DB veritabanının bağlantısını da yapılandırmanız gerekir.

1. *Cosmos DB belge bağlantısı" etiketinin yanındaki “Yeni” bağlantısına tıklayın.
1. Alanları doldurun ve Cosmos DB belgesini oluşturmak için gereken uygun seçenekleri belirleyin.

![Cosmos DB bağlantısını yapılandırma](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

|  Alan | Değer  |
|---|---|
| Kimlik | Cosmos DB veritabanı için benzersiz kimlik  |
| NoSQL API | Cosmos DB veya MongoDB  |
| Abonelik | MSDN Aboneliği  |
| Kaynak Grubu  | Yeni bir grup oluşturun veya mevcut bir grubu seçin.  |
| Konum  | WestEurope  |

1. *Tamam* düğmesine tıklayın. Azure kaynakları oluştururken birkaç dakika beklemeniz gerekebilir.
1. *Kaydet* düğmesine tıklayın.

## <a name="update-the-function-code"></a>İşlev kodunu güncelleştirme

İşlevin şablon kodunu şununla değiştirin:

Bu örneğe yönelik kodun yalnızca C#'ta olduğunu unutmayın.

```csharp
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, out object taskDocument, TraceWriter log)
{
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    string task = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "task", true) == 0)
        .Value;

    string duedate = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "duedate", true) == 0)
        .Value;

    taskDocument = new {
        name = name,
        duedate = duedate.ToString(),
        task = task
    };

    if (name != "" && task != "") {
        return req.CreateResponse(HttpStatusCode.OK);
    }
    else {
        return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}

```

Bu kod örneği, HTTP İsteği sorgu dizelerini okur ve bunları `taskDocument` nesnesinin üyeleri olarak atar. `taskDocument` nesnesi, Cosmos DB veritabanındaki verileri otomatik olarak kaydeder ve hatta ilk kullanımda veritabanını oluşturur.

## <a name="test-the-function-and-database"></a>İşlevi ve veritabanını test etme

1. İşlev sekmesinde, portalın sağ tarafındaki *Test* bağlantısına tıklayın ve aşağıdaki HTTP sorgu dizelerini girin:

| Sorgu Dizesi | Değer |
|---|---|
| ad | Chris P. Bacon |
| görev | BLT sandviçi oluşturma |
| duedate | 12.05.2017 |

1. *Çalıştır* bağlantısına tıklayın.
1. İşlevin *HTTP 200 OK* Yanıt kodunu döndürdüğünü doğrulayın.

![Cosmos DB çıktı bağlamasını yapılandırma](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

Cosmos DB veritabanına bir giriş yapıldığını onaylayın.

1. Azure portalında veritabanınızı bulup seçin.
1. *Veri Gezgini* seçeneğini belirleyin.
1. Belgenin girişlerine ulaşana dek düğümleri genişletin.
1. Veritabanı girişini onaylayın. Veritabanınızda, verilerinizle birlikte ek meta veriler olacaktır.

![Cosmos DB girişini doğrulama](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

Veriler belgedeyse yapılandırılmamış verileri Cosmos DB veritabanında depolayan Azure işlevini başarıyla oluşturmuşsunuzdur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

Cosmos DB veritabanına bağlama hakkında daha fazla bilgi için bkz. [Azure İşlevleri Cosmos DB bağlamaları](functions-bindings-documentdb.md).

