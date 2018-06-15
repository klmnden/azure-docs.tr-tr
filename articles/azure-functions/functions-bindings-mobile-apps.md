---
title: Azure işlevleri için Mobile Apps bağlamaları
description: Azure Mobile Apps bağlamaları Azure işlevlerini kullanmak nasıl anlayın.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 67adec7f30c8e4b24d0726ebdefa613fcefa7d3e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725267"
---
# <a name="mobile-apps-bindings-for-azure-functions"></a>Azure işlevleri için Mobile Apps bağlamaları 

Bu makale ile nasıl çalışılacağını açıklar [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) Azure işlevlerinde bağlar. Giriş ve Mobile Apps için bağlamaları çıktı Azure işlevleri destekler.

Let Mobile Apps bağlamaları okuma ve güncelleştirme mobil uygulamalarda veri tabloları.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Mobile Apps bağlamaları verilmiştir [Microsoft.Azure.WebJobs.Extensions.MobileApps](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps) NuGet paketi, sürüm 1.x. Paket için kaynak kodunu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.MobileApps/) GitHub depo.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Mobile Apps bağlamaları verilmiştir [Microsoft.Azure.WebJobs.Extensions.MobileApps](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps) NuGet paketi, sürüm 3.x. Paket için kaynak kodunu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/) GitHub depo.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Girdi

Mobile Apps giriş bağlaması bir kaydı bir mobil tablo uç noktasından yükler ve işlevinizin geçirir. İşlev başarıyla çıktığında, C# ve F # işlevleri kayda yapılan değişiklikler otomatik olarak tabloya geri gönderilir.

## <a name="input---example"></a>Girişi - örnek

Dile özgü örneğe bakın:

* [C# betik (.csx)](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Giriş - C# kod örneği

Aşağıdaki örnek, Mobile Apps giriş bağlamasında gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev kayıt tanımlayıcısı olan bir kuyruk iletisi tarafından tetiklenir. İşlev belirtilen kayıt okur ve değiştirir kendi `Text` özelliği.

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
],
"disabled": false
}
```
[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kod aşağıdaki gibidir:

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

Aşağıdaki örnek, Mobile Apps giriş bağlamasında gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev kayıt tanımlayıcısı olan bir kuyruk iletisi tarafından tetiklenir. İşlev belirtilen kayıt okur ve değiştirir kendi `Text` özelliği.

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
],
"disabled": false
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

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz: [yapılandırma bölümü aşağıdaki](#input---configuration).

## <a name="input---configuration"></a>Girişi - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `MobileTable` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **type**|| "MobileTable" olarak ayarlanmalıdır|
| **direction**||"İçin" ayarlanmalıdır|
| **Adı**|| İşlev imzası giriş parametresinin adı.|
|**TableName** |**TableName**|Mobil uygulamanızın veri tablosu adı|
| **Kimliği**| **Kimlik** | Alınacak kayıt tanımlayıcısı. Statik veya işlevi çağırır tetikleyici bağlı olabilir. Örneğin, bir sıra tetikleyici işlevinizi için sonra kullanırsanız `"id": "{queueTrigger}"` almak için kuyruk iletisini dize değerini kayıt kimliği kullanır.|
|**Bağlantı**|**Bağlantı**|Mobil uygulamanızın URL'sine sahip bir uygulama ayarı adı. Gerekli REST işlemlerini karşı mobil uygulamanızı oluşturmak için bu URL'yi işlevini kullanır. İşlevi uygulamanızda mobil uygulamanın URL'si içeren bir uygulama ayarı oluşturmak ve ardından uygulama ayarı adı belirtin `connection` giriş bağlaması özelliği. URL benzer `http://<appname>.azurewebsites.net`.
|**apikey ile yapılan**|**apikey ile yapılan**|Mobil uygulamanızın API anahtara sahip bir uygulama ayarı adı. API anahtar IF sağlamak, [bir API anahtarı Node.js mobil uygulamanızda uygulamak](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), veya [bir API anahtarı .NET Mobil uygulamanızda uygulamak](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Anahtar sağlamak için işlevi uygulamanızı API anahtarını içeren bir uygulama ayarı oluşturmak, ardından ekleyin `apiKey` , giriş bağlama uygulama ayarı adı ile bir özellik. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> API anahtarı ile mobil uygulama istemcilerinizi paylaşmayın. Bunu yalnızca güvenli bir şekilde Azure işlevleri gibi hizmet tarafı istemcilerine dağıtılmalıdır. Azure işlevleri depolar bağlantı bilgilerini ve API anahtarlarını uygulama ayarlarda böylece kaynak denetimi deponuzun denetlenmez. Bu, hassas bilgilerinizi koruma sağlar.

## <a name="input---usage"></a>Giriş - kullanım

C# işlevlerde, belirtilen Kimliğe sahip kaydı bulunduğunda, geçirilen adlandırılmış içine [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametresi. Kayıt bulunamadı, parametre değeridir `null`. 

JavaScript işlevleri kaydı içine geçirilen `context.bindings.<name>` nesnesi. Kayıt bulunamadı, parametre değeridir `null`. 

C# ve F # işlevleri, giriş değişiklik kaydı (giriş parametresi) otomatik olarak gönderilir tabloya geri işlevi başarıyla çıktığında. JavaScript işlevleri kaydında değiştiremezsiniz.

## <a name="output"></a>Çıktı

Yeni bir kayıt Mobile Apps tabloya yazmak için bağlama Mobile Apps çıkış kullanın.  

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betik (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , bir kuyruk iletisi tarafından tetiklenir ve bir mobil uygulama tablosuna bir kayıt oluşturur.

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

### <a name="output---c-script-example"></a>Çıktı - C# kod örneği

Aşağıdaki örnek, bağlama Mobile Apps çıkış gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlevi bir kuyruk iletisi tarafından tetiklenir ve sabit kodlanmış değerini içeren yeni bir kayıt oluşturur `Text` özelliği.

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

C# betik kod aşağıdaki gibidir:

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

Aşağıdaki örnek, bağlama Mobile Apps çıkış gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlevi bir kuyruk iletisi tarafından tetiklenir ve sabit kodlanmış değerini içeren yeni bir kayıt oluşturur `Text` özelliği.

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

## <a name="output---attributes"></a>Çıktı - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) özniteliği.

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz: [çıktı - yapılandırma](#output---configuration). Burada bir `MobileTable` yöntemi imza özniteliği örnekte:

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

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `MobileTable` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **type**|| "MobileTable" olarak ayarlanmalıdır|
| **direction**||Out"için" olarak ayarlanmalıdır|
| **Adı**|| İşlev imzası çıkış parametresinin adı.|
|**TableName** |**TableName**|Mobil uygulamanızın veri tablosu adı|
|**Bağlantı**|**MobileAppUriSetting**|Mobil uygulamanızın URL'sine sahip bir uygulama ayarı adı. Gerekli REST işlemlerini karşı mobil uygulamanızı oluşturmak için bu URL'yi işlevini kullanır. İşlevi uygulamanızda mobil uygulamanın URL'si içeren bir uygulama ayarı oluşturmak ve ardından uygulama ayarı adı belirtin `connection` giriş bağlaması özelliği. URL benzer `http://<appname>.azurewebsites.net`.
|**apikey ile yapılan**|**ApiKeySetting**|Mobil uygulamanızın API anahtara sahip bir uygulama ayarı adı. API anahtar IF sağlamak, [bir API anahtarı, Node.js mobil uygulama arka ucuna uygulamak](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), veya [bir API anahtarı, .NET Mobil uygulama arka ucuna uygulamak](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Anahtar sağlamak için işlevi uygulamanızı API anahtarını içeren bir uygulama ayarı oluşturmak, ardından ekleyin `apiKey` , giriş bağlama uygulama ayarı adı ile bir özellik. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> API anahtarı ile mobil uygulama istemcilerinizi paylaşmayın. Bunu yalnızca güvenli bir şekilde Azure işlevleri gibi hizmet tarafı istemcilerine dağıtılmalıdır. Azure işlevleri depolar bağlantı bilgilerini ve API anahtarlarını uygulama ayarlarda böylece kaynak denetimi deponuzun denetlenmez. Bu, hassas bilgilerinizi koruma sağlar.

## <a name="output---usage"></a>Çıktı - kullanım

C# betik işlevlerde, türünde bir adlandırılmış çıktı parametresi kullanın `out object` çıkış kaydı erişmek için. C# sınıfı kitaplıklar içinde `MobileTable` özniteliği şu türlerden birini kullanılabilir:

* `ICollector<T>` veya `IAsyncCollector<T>`, burada `T` ya `JObject` veya herhangi türdeki bir `public string Id` özelliği.
* `out JObject`
* `out T` veya `out T[]`, burada `T` ile herhangi bir tür bir `public string Id` özelliği.

Node.js işlevlerini kullanmak `context.bindings.<name>` çıkış kaydı erişmek için.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
