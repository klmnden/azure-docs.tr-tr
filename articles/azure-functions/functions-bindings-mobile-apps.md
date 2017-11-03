---
title: "Azure işlevleri Mobile Apps bağlamaları | Microsoft Docs"
description: "Azure Mobile Apps bağlamaları Azure işlevlerini kullanmak nasıl anlayın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.openlocfilehash: d2c0e4e233761584bad2df05a8e702e4fc77e84f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a>Azure işlevleri Mobile Apps bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede nasıl yapılandırılacağı ve kod açıklanmaktadır [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) Azure işlevlerinde bağlar. Giriş ve Mobile Apps için bağlamaları çıktı Azure işlevleri destekler.

Mobile Apps giriş ve çıkış bağlamaları izin [okuma ve yazma veri tablolarına](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) mobil uygulamanızda.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a>Mobile Apps giriş bağlama
Mobile Apps giriş bağlaması bir kaydı bir mobil tablo uç noktasından yükler ve işlevinizin geçirir. İşlev başarıyla çıktığında, C# ve F # işlevleri kayda yapılan değişiklikler otomatik olarak tabloya geri gönderilir.

Bir işlev için Mobile Apps giriş aşağıdaki JSON nesnesinde kullanır `bindings` function.json dizisi:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of the record to retrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

Şunlara dikkat edin:

* `id`statik olabilir veya işlevi çağırır Tetikle dayalı olabilir. Kullanırsanız, örneğin, bir [sıra tetikleyici]() , işlev, ardından `"id": "{queueTrigger}"` almak için kuyruk iletisini dize değerini kayıt kimliği kullanır.
* `connection`bir uygulama ayarı sırayla mobil uygulamanızı URL'sini içeren işlevi uygulamanızda adını içermelidir. Gerekli REST işlemlerini karşı mobil uygulamanızı oluşturmak için bu URL'yi işlevini kullanır. [İşlevi uygulamanıza bir uygulama ayarı oluşturmak]() , mobil uygulamanızın URL'si içeren (gibi görünen `http://<appname>.azurewebsites.net`), uygulama ayarı adı belirtin `connection` giriş bağlaması özelliği. 
* Belirtmek zorunda `apiKey` varsa, [bir API anahtarı, Node.js mobil uygulama arka ucuna uygulamak](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), veya [bir API anahtarı, .NET Mobil uygulama arka ucuna uygulamak](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Bunu yapmak için [işlevi uygulamanıza bir uygulama ayarı oluşturmak]() API anahtarını içeren, ardından ekleyin `apiKey` , giriş bağlama uygulama ayarı adı ile bir özellik. 
  
  > [!IMPORTANT]
  > Bu API anahtarı ile mobil uygulama istemcilerinizi paylaşılmayan gerekir. Bunu yalnızca güvenli bir şekilde Azure işlevleri gibi hizmet tarafı istemcilerine dağıtılmalıdır. 
  > 
  > [!NOTE]
  > Azure işlevleri depolar bağlantı bilgilerini ve API anahtarlarını uygulama ayarlarda böylece kaynak denetimi deponuzun denetlenmez. Bu, hassas bilgilerinizi koruma sağlar.
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a>Giriş kullanımı
Bu bölümde işlevi kodunuzda, Mobile Apps giriş bağlaması kullanmayı gösterir. 

Belirtilen tablo ve kayıt kimliği kaydıyla bulunduğunda, geçirilen adlandırılmış içine [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametresi (veya içine geçirilen Node.js içinde `context.bindings.<name>` nesnesi). Kayıt bulunamadı, parametredir `null`. 

C# ve F # işlevleri, giriş değişiklik işlevi başarıyla çıktığında kayıt (giriş parametresi) Mobile Apps tablosuna otomatik olarak gönderilir. Node.js işlevlerini kullanmak `context.bindings.<name>` girdi kaydının erişmek için. Node.js kaydında değiştiremezsiniz.

<a name="inputsample"></a>

## <a name="input-sample"></a>Giriş örneği
Bir mobil uygulama tablosu kaydı sıra tetikleyici ileti kimliği alan aşağıdaki function.json olduğunu varsayalım:

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

Bağlama girdi kaydından kullanan dile özgü örneğe bakın. C# ve F # örnekleri ayrıca kaydın değişiklik `text` özelliği.

* [C#](#inputcsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>C# giriş örneği #

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

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Node.js giriş örneği

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a>Mobile Apps bağlama çıktı
Mobile Apps tablo uç noktası için yeni bir kayıt yazmak için bağlama Mobile Apps çıkış kullanın.  

Bir işlev aşağıdaki JSON nesnesinde kullanan çıktısı Mobile Apps `bindings` function.json dizisi:

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

Şunlara dikkat edin:

* `connection`bir uygulama ayarı sırayla mobil uygulamanızı URL'sini içeren işlevi uygulamanızda adını içermelidir. Gerekli REST işlemlerini karşı mobil uygulamanızı oluşturmak için bu URL'yi işlevini kullanır. [İşlevi uygulamanıza bir uygulama ayarı oluşturmak]() , mobil uygulamanızın URL'si içeren (gibi görünen `http://<appname>.azurewebsites.net`), uygulama ayarı adı belirtin `connection` giriş bağlaması özelliği. 
* Belirtmek zorunda `apiKey` varsa, [bir API anahtarı, Node.js mobil uygulama arka ucuna uygulamak](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), veya [bir API anahtarı, .NET Mobil uygulama arka ucuna uygulamak](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). Bunu yapmak için [işlevi uygulamanıza bir uygulama ayarı oluşturmak]() API anahtarını içeren, ardından ekleyin `apiKey` , giriş bağlama uygulama ayarı adı ile bir özellik. 
  
  > [!IMPORTANT]
  > Bu API anahtarı ile mobil uygulama istemcilerinizi paylaşılmayan gerekir. Bunu yalnızca güvenli bir şekilde Azure işlevleri gibi hizmet tarafı istemcilerine dağıtılmalıdır. 
  > 
  > [!NOTE]
  > Azure işlevleri depolar bağlantı bilgilerini ve API anahtarlarını uygulama ayarlarda böylece kaynak denetimi deponuzun denetlenmez. Bu, hassas bilgilerinizi koruma sağlar.
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a>Çıktı kullanımı
Bu bölümde işlevi kodunuzda bağlama, Mobile Apps çıkış kullanmayı gösterir. 

C# işlevlerde, türünde bir adlandırılmış çıktı parametresi kullanın `out object` çıkış kaydı erişmek için. Node.js işlevlerini kullanmak `context.bindings.<name>` çıkış kaydı erişmek için.

<a name="outputsample"></a>

## <a name="output-sample"></a>Çıkış örneği
Bir kuyruk tetikleyici ve Mobile Apps çıkış tanımlayan aşağıdaki function.json olduğunu varsayalım:

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

Mobile Apps tablo uç noktası kuyruk iletisini içeriğe sahip bir kayıt oluşturur dile özgü örneğe bakın.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>C# çıktı örneği #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Node.js çıktı örneği

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

