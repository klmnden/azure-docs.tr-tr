---
title: Azure işlevleri için Notification Hubs bağlamaları
description: Azure işlevleri'nde Azure bildirim hub'ı bağlama kullanma hakkında bilgi edinin.
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
ms.openlocfilehash: 79ea9455fec7d31f800b2b5d36df6a2a53f502c3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61438196"
---
# <a name="notification-hubs-output-binding-for-azure-functions"></a>Notification hubs'ı çıkışı bağlama için Azure işlevleri

Bu makalede kullanarak anında iletme bildirimleri göndermek açıklanmaktadır [Azure Notification hubs'ı](../notification-hubs/notification-hubs-push-notification-overview.md) Azure işlevleri'nde bağlar. Azure işlevleri destekler çıkış bağlamaları bildirim hub'ları için.

Azure Notification hubs'ı, Platform bildirim sistemi (kullanmak istediğiniz PNS için) yapılandırılması gerekir. Anında iletme bildirimleri istemci uygulamanızı Notification Hubs'a ait alma hakkında bilgi için bkz: [Notification Hubs ile çalışmaya başlama](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) ve hedef istemci platformunuzu sayfanın üst kısmındaki açılan listeden seçin.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!IMPORTANT]
> Google da aynı şekilde [Google Cloud Messaging (GCM) Firebase Cloud Messaging (FCM) ile değiştiriliyor kullanım dışı](https://developers.google.com/cloud-messaging/faq). Bu çıkış bağlaması FCM desteklemiyor. FCM kullanarak bildirim göndermek için [Firebase API](https://firebase.google.com/docs/cloud-messaging/server#choosing-a-server-option) işlevi ya da kullanım, doğrudan [şablon bildirimleri](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Notification Hubs bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs) NuGet paketi sürüm 1.x. Paket için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.NotificationHubs) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Bu bağlama işlevlerinde kullanılamaz 2.x.

## <a name="example---template"></a>Örnek - şablon

Gönderdiğiniz bildirimleri yerel bildirimleri olabilir veya [şablon bildirimleri](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Yerel bildirimleri yapılandırılan belirli istemci platformu hedefleyen `platform` çıkış bağlaması özelliğidir. Şablon bir bildirim, birden çok platformu hedefleyecek şekilde kullanılabilir.   

Dile özgü örneğe bakın:

* [C# betiği - out parametresi](#c-script-template-example---out-parameter)
* [C# betiği - zaman uyumsuz](#c-script-template-example---asynchronous)
* [C# betiği - JSON](#c-script-template-example---json)
* [C# betiği - kitaplık türleri](#c-script-template-example---library-types)
* [F#](#f-template-example)
* [JavaScript](#javascript-template-example)

### <a name="c-script-template-example---out-parameter"></a>C# betik şablon örneği - out parametresi

Bu örnek, bir bildirim gönderir bir [şablon kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren bir `message` şablonunda yer tutucu.

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = GetTemplateProperties(myQueueItem);
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return templateProperties;
}
```

### <a name="c-script-template-example---asynchronous"></a>C# betik şablon örneği - zaman uyumsuz

Zaman uyumsuz kod kullanıyorsanız, parametrelerine izin verilmez. Bu durumda `IAsyncCollector` , şablon bildirim dönün. Aşağıdaki kod, yukarıdaki kod, zaman uyumsuz bir örnektir. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification to Notification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants to be added : " + message;
    return templateProperties;
}
```

### <a name="c-script-template-example---json"></a>C# betik şablon örneği - JSON

Bu örnek, bir bildirim gönderir bir [şablon kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren bir `message` geçerli bir JSON dizesi kullanarak şablonu yer tutucusu.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

### <a name="c-script-template-example---library-types"></a>C# betik şablon örneği - kitaplık türleri

Bu örnek, tanımlanan türleri kullanmayı gösterir. [Microsoft Azure Notification Hubs Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

```cs
#r "Microsoft.Azure.NotificationHubs"

using System;
using System.Threading.Tasks;
using Microsoft.Azure.NotificationHubs;

public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
{
   log.Info($"C# Queue trigger function processed: {myQueueItem}");
   notification = GetTemplateNotification(myQueueItem);
}

private static TemplateNotification GetTemplateNotification(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return new TemplateNotification(templateProperties);
}
```

### <a name="f-template-example"></a>F#Şablon örneği

Bu örnek, bir bildirim gönderir bir [şablon kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren `location` ve `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

### <a name="javascript-template-example"></a>JavaScript şablon örneği

Bu örnek, bir bildirim gönderir bir [şablon kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren `location` ve `message`.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if (myTimer.IsPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);  
    context.bindings.notification = {
        location: "Redmond",
        message: "Hello from Node!"
    };
    context.done();
};
```

## <a name="example---apns-native"></a>Örnek - APNS yerel

Bu C# betiği örneği, yerel APNS bildirim göndermek nasıl gösterir. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.LogInformation($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.LogInformation($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="example---wns-native"></a>Örnek - WNS yerel

Bu C# betiği örneği, türleri içinde tanımlanan işlemi gösterilir [Microsoft Azure Notification Hubs Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) yerel WNS bildirim göndermek için. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The XML format for a native WNS toast notification is ...
    // <?xml version="1.0" encoding="utf-8"?>
    // <toast>
    //      <visual>
    //     <binding template="ToastText01">
    //       <text id="1">notification message</text>
    //     </binding>
    //   </visual>
    // </toast>

    log.Info($"Sending WNS toast notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                    "<toast><visual><binding template=\"ToastText01\">" +
                                        "<text id=\"1\">" + 
                                            "A new user wants to be added (" + user.name + ")" + 
                                        "</text>" +
                                    "</binding></visual></toast>";

    log.Info($"{wnsNotificationPayload}");
    await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));        
}
```

## <a name="attributes"></a>Öznitelikler

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [NotificationHub](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) özniteliği.

Öznitelik oluşturucu parametresi ve özellikleri açıklanmıştır [yapılandırma](#configuration) bölümü.

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `NotificationHub` özniteliği:

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** |yok| "NotificationHub için" olarak ayarlanmalıdır. |
|**direction** |yok| "Out" ayarlanmalıdır. | 
|**Adı** |yok| İşlev kodu bildirim hub'ı iletide kullanılan değişken adı. |
|**tagExpression** |**TagExpression** | Etiket ifadeleri bir etiket ifadeyle eşleşecek bildirimleri almak için kayıtlı cihazlar kümesine bildirimleri teslim edilmesini belirtmenizi sağlar.  Daha fazla bilgi için [Yönlendirme ve etiket ifadeleri](../notification-hubs/notification-hubs-tags-segment-push-message.md). |
|**HubName** | **HubName** | Azure Portalı'ndaki bildirim hub'ı kaynağının adı. |
|**bağlantı** | **connectionStringSetting** | Notification hubs'ı bağlantı dizesi içeren bir uygulama ayarı adı.  Bağlantı dizesini ayarlamak *DefaultFullSharedAccessSignature* bildirim hub'ınız için bir değer. Bkz: [bağlantı dizesi kurulumu](#connection-string-setup) bu makalenin ilerleyen bölümlerinde.|
|**Platform** | **Platform** | Platform özelliği bildirim hedeflerinizi istemci platformları gösterir. Platform özelliği çıkış bağlamanın dışında belirtilmezse varsayılan olarak, Azure bildirim Hub'ındaki yapılandırılmış herhangi bir platformu hedefleyecek şekilde şablon bildirimleri kullanılabilir. Genel çapraz platform bildirimleri bir Azure bildirim hub'ına göndermek için şablonları kullanma hakkında daha fazla bilgi için bkz. [şablonları](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Ayarlandığında, **platform** aşağıdaki değerlerden biri olmalıdır: <ul><li><code>apns</code>&mdash;Apple anında iletilen bildirim servisi. APNS için bildirim hub'ı yapılandırma ve bir istemci uygulamasında bildirim alma hakkında daha fazla bilgi için bkz. [iOS için Azure Notification Hubs ile Android'e anında iletme bildirimleri](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md).</li><li><code>adm</code>&mdash;[Amazon cihaz Mesajlaşma](https://developer.amazon.com/device-messaging). ADM için bildirim hub'ı yapılandırma ve bir Kindle uygulaması bildirim alma hakkında daha fazla bilgi için bkz. [Kindle uygulamaları için Notification Hubs ile çalışmaya başlama](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md).</li><li><code>wns</code>&mdash;[Windows anında bildirim Hizmetleri](/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview) Windows platformlarını hedefleyen. Windows Phone 8.1 ve üzeri, WNS tarafından da desteklenir. Daha fazla bilgi için [için Windows Evrensel Platform uygulamaları Notification Hubs ile çalışmaya başlama](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</li><li><code>mpns</code>&mdash;[Microsoft anında bildirim hizmeti](/previous-versions/windows/apps/ff402558(v=vs.105)). Bu platform, Windows Phone 8 ve önceki Windows Phone platformları destekler. Daha fazla bilgi için [Windows Phone üzerinde Azure Notification Hubs ile Android'e anında iletme bildirimleri](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md).</li></ul> |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="functionjson-file-example"></a>Function.JSON dosyası örneği

İşte bir bildirim hub'ları bağlamanın ilişkin bir örnek bir *function.json* dosya.

```json
{
  "bindings": [
    {
      "type": "notificationHub",
      "direction": "out",
      "name": "notification",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "apns"
    }
  ],
  "disabled": false
}
```

### <a name="connection-string-setup"></a>Bağlantı dizesi kurulumu

Bir bildirim hub'ı çıkış bağlaması kullanmak için hub için bağlantı dizesini de yapılandırmanız gerekir. Mevcut bir bildirim hub'ı seçin veya yeni bir sağ taraftan oluşturma *tümleştir* Azure portalında sekmesi. Bağlantı dizesi el ile yapılandırabilirsiniz. 

Mevcut bir bildirim hub'ına bağlantı dizesini yapılandırmak için:

1. Bildirim hub'ınıza gidin [Azure portalında](https://portal.azure.com), seçin **erişim ilkeleri**ve ardından Kopyala düğmesini işaretleyin **DefaultFullSharedAccessSignature** ilkesi. Bu bağlantı dizesini kopyalar *DefaultFullSharedAccessSignature* bildirim hub'ınıza ilkesi. Bu bağlantı dizesi, bildirim hub'ına göndermek, işlevinizi olanak tanır.
    ![Bildirim hub'ı bağlantı dizesini kopyalayın](./media/functions-bindings-notification-hubs/get-notification-hub-connection.png)
1. Azure portalında işlev uygulamanıza gidin, **uygulama ayarları**, bir anahtar ekleyin **MyHubConnectionString**, kopyalanan yapıştırın *DefaultFullSharedAccessSignature* değeri ve ardından olarak bildirim hub'ınız için **Kaydet**.

Çıkış bağlaması bağlantı ayarında unsurları bu uygulama ayarının adı olan *function.json* veya .NET özniteliği. Bkz: [yapılandırma bölümü](#configuration) bu makalenin üst kısmındaki.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Notification Hub'ı | [İşlemler Kılavuzu](https://docs.microsoft.com/rest/api/notificationhubs/) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

