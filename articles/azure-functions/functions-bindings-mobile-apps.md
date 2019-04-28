---
title: Azure işlevleri için Mobile Apps bağlamaları
description: Azure işlevleri'nde Azure Mobile Apps bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 5fd220f15f363c1987f1576009519e4b2feae6b9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61438134"
---
# <a name="mobile-apps-bindings-for-azure-functions"></a>Azure işlevleri için Mobile Apps bağlamaları 

> [!NOTE]
> Azure Mobile Apps bağlamaları bulunan ve yalnızca Azure işlevleri'ne 1.x. Azure işlevleri'nde desteklenmez 2.x.

Bu makalede ile nasıl çalışılacağı açıklanmaktadır [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) Azure işlevleri'nde bağlar. Giriş ve çıkış bağlamaları Mobile Apps için Azure işlevleri destekler.

Mobile Apps bağlamaları izin okumasına ve güncelleştirmesine mobil uygulamalarda veri tabloları.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Mobile Apps bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.MobileApps](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps) NuGet paketi sürüm 1.x. Paket için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.MobileApps/) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="input"></a>Girdi

Mobile Apps giriş bağlamasına mobil tablo uç noktasından bir kaydı yükler ve, işleve geçirir. İçinde C# ve F# İşlevler, kayıtta yapılan tüm değişiklikler otomatik olarak gönderilir tabloya işlevi başarıyla çıktığında.

## <a name="input---example"></a>Giriş - örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#input---c-script-example)
* JavaScript

### <a name="input---c-script-example"></a>Giriş - C# betiği örneği

Aşağıdaki örnek, bir Mobile Apps giriş bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevi, bir kayıt tanımlayıcısı olan bir kuyruk iletisi tarafından tetiklenir. İşlev belirtilen kayıt okur ve değiştiren kendi `Text` özelliği.

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
]
}
```
[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

### <a name="input---javascript"></a>Giriş - JavaScript

Aşağıdaki örnek, bir Mobile Apps giriş bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi, bir kayıt tanımlayıcısı olan bir kuyruk iletisi tarafından tetiklenir. İşlev belirtilen kayıt okur ve değiştiren kendi `Text` özelliği.

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
]
}
```
[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

## <a name="input---attributes"></a>Giriş - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) özniteliği.

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz. [aşağıdaki yapılandırma bölümüne](#input---configuration).

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `MobileTable` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **type**|| "MobileTable için" olarak ayarlanmalıdır|
| **direction**||"İçin" ayarlanmalıdır|
| **Adı**|| İşlev imzası giriş parametresi adı.|
|**TableName** |**TableName**|Mobil uygulamanın veri tablosunun adı|
| **id**| **Kimlik** | Alınacak kaydın tanıtıcısı. Bağlı işlevi çağıran bir tetikleyici olarak veya statik olabilir. Örneğin, bir kuyruk tetikleyicisi işleviniz için ardından kullanırsanız `"id": "{queueTrigger}"` kuyruk iletisini dize değerini almak için kayıt Kimliğini kullanır.|
|**bağlantı**|**bağlantı**|Mobil uygulamanızın URL'sine sahip bir uygulama ayarının adı. İşlev, gerekli REST işlemlerini karşı mobil uygulamanızı oluşturmak için bu URL'yi kullanır. İşlev uygulamanızda mobil uygulamanın URL'si içeren bir uygulama ayarı oluşturmak ve ardından uygulama ayarlarında adını `connection` , giriş bağlama özelliği. URL şuna `http://<appname>.azurewebsites.net`.
|**ApiKey**|**ApiKey**|Mobil uygulamanızın API anahtarı içeren bir uygulama ayarının adı. API anahtarı if sağlayın, [bir API anahtarı Node.js mobil uygulamanıza](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), veya [bir API anahtarı .NET Mobil uygulamanıza](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Anahtar sağlamak için işlev uygulamanızı API anahtarını içeren bir uygulama ayarı oluşturmak, sonra Ekle `apiKey` özelliği, giriş bağlama uygulama ayarının adı. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> API anahtarı ile mobil uygulama istemcilerinize paylaşmayın. Bunu yalnızca güvenli bir şekilde Azure işlevleri gibi hizmet tarafı istemcilerine dağıtılmalıdır. Azure işlevleri depolar API anahtarları ve bağlantı bilgilerini uygulama ayarları olarak böylece kaynak denetimi deponuzun denetlenmez. Bu, hassas bilgilerinizi korur.

## <a name="input---usage"></a>Giriş - kullanım

C# işlevleri'nde belirtilen Kimliğe sahip kaydı bulunduğunda, geçirilen adlandırılmış içine [JObject](https://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametresi. Kayıt bulunamadı, parametre değeri olduğu `null`. 

JavaScript işlevleri'nde kaydın içine geçirilen `context.bindings.<name>` nesne. Kayıt bulunamadı, parametre değeri olduğu `null`. 

İçinde C# ve F# İşlevler, giriş yaptığınız tüm değişiklikler işlevi başarıyla çıktığında kayıt (giriş parametresi) tablosuna otomatik olarak gönderilen. JavaScript işlevleri bir kayıtta değiştiremezsiniz.

## <a name="output"></a>Çıktı

Mobile Apps çıkış bir Mobile Apps tablosuna yeni bir kayıt yazmak için bağlaması kullanın.  

## <a name="output---example"></a>Çıkış - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıkış - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) bir kuyruk iletisi tarafından tetiklenir ve bir mobil uygulama tablosunda bir kaydını oluşturur.

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
    TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

### <a name="output---c-script-example"></a>Çıkış - C# betiği örneği

Aşağıdaki örnek, bağlama bir Mobile Apps çıkış gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevi bir kuyruk iletisi tarafından tetiklenir ve yeni bir kayıt için sabit kodlu değer oluşturur `Text` özelliği.

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
]
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

### <a name="output---javascript-example"></a>Çıkış - JavaScript örneği

Aşağıdaki örnek, bağlama bir Mobile Apps çıkış gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi bir kuyruk iletisi tarafından tetiklenir ve yeni bir kayıt için sabit kodlu değer oluşturur `Text` özelliği.

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="output---attributes"></a>Çıkış - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) özniteliği.

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz. [çıkışı - yapılandırma](#output---configuration). İşte bir `MobileTable` özniteliği örnek bir yöntem imzası:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
    TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output---c-example).

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `MobileTable` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **type**|| "MobileTable için" olarak ayarlanmalıdır|
| **direction**||"Out" olarak ayarlanmalıdır|
| **Adı**|| İşlev imzası çıkış parametresinin adı.|
|**TableName** |**TableName**|Mobil uygulamanın veri tablosunun adı|
|**bağlantı**|**MobileAppUriSetting**|Mobil uygulamanızın URL'sine sahip bir uygulama ayarının adı. İşlev, gerekli REST işlemlerini karşı mobil uygulamanızı oluşturmak için bu URL'yi kullanır. İşlev uygulamanızda mobil uygulamanın URL'si içeren bir uygulama ayarı oluşturmak ve ardından uygulama ayarlarında adını `connection` , giriş bağlama özelliği. URL şuna `http://<appname>.azurewebsites.net`.
|**ApiKey**|**ApiKeySetting**|Mobil uygulamanızın API anahtarı içeren bir uygulama ayarının adı. API anahtarı if sağlayın, [Node.js mobil uygulama arka ucunuza bir API anahtarı uygulama](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), veya [, .NET Mobil uygulama arka ucu bir API anahtarı uygulama](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Anahtar sağlamak için işlev uygulamanızı API anahtarını içeren bir uygulama ayarı oluşturmak, sonra Ekle `apiKey` özelliği, giriş bağlama uygulama ayarının adı. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> API anahtarı ile mobil uygulama istemcilerinize paylaşmayın. Bunu yalnızca güvenli bir şekilde Azure işlevleri gibi hizmet tarafı istemcilerine dağıtılmalıdır. Azure işlevleri depolar API anahtarları ve bağlantı bilgilerini uygulama ayarları olarak böylece kaynak denetimi deponuzun denetlenmez. Bu, hassas bilgilerinizi korur.

## <a name="output---usage"></a>Çıkış - kullanım

C# betik işlevleri'nde türünde bir adlandırılmış çıktı parametresi kullanın `out object` çıkışı kayda erişmek için. C# sınıf kitaplıkları içinde `MobileTable` öznitelik şu türlerden birini kullanılabilir:

* `ICollector<T>` veya `IAsyncCollector<T>`burada `T` ya da `JObject` veya any türüne sahip bir `public string Id` özelliği.
* `out JObject`
* `out T` veya `out T[]`burada `T` herhangi bir tür ile bir `public string Id` özelliği.

Node.js işlevleri'nde kullanmak `context.bindings.<name>` çıkışı kayda erişmek için.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
