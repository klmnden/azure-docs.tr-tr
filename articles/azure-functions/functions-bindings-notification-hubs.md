---
title: Azure işlevleri için bildirim hub'ları bağlamaları
description: Azure bildirim hub'ı bağlama Azure işlevlerini kullanmak nasıl anlayın.
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
ms.openlocfilehash: 4208a7be3c002bcefed273015d002cb1aee0fecd
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724984"
---
# <a name="notification-hubs-output-binding-for-azure-functions"></a>Bildirim hub'ları Azure işlevleri için bağlama çıktı

Bu makalede kullanarak anında iletme bildirimleri göndermek nasıl açıklanmaktadır [Azure Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md) Azure işlevlerinde bağlar. Azure işlevleri destekler çıkış bağlamaları bildirim hub'ları için.

Azure bildirim hub'ları Platform bildirim hizmeti (kullanmak istediğiniz PNS için) yapılandırılması gerekir. Anında iletme bildirimleri istemci uygulamanızı bildirim hub'larından alma hakkında bilgi için bkz: [Notification Hubs ile çalışmaya başlama](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) ve sayfanın üstünde yanında aşağı açılan listeden, hedef istemci platformu seçin.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Bildirim hub'ları bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.NotificationHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs) NuGet paketi, sürüm 1.x. Paket için kaynak kodunu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.NotificationHubs) GitHub depo.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Bu bağlama işlevlerinde kullanılamaz 2.x.

## <a name="example---template"></a>Örnek - şablonu

Gönderdiğiniz bildirimleri yerel bildirimler olabilir veya [şablon bildirimleri](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Yerel bildirimler yapılandırılmış gibi belirli istemci platformu hedefleyen `platform` çıkış bağlama özelliği. Bir şablon bildirim hedef birden çok platform için kullanılabilir.   

Dile özgü örneğe bakın:

* [C# betik - çıkış parametresi](#c-script-template-example---out-parameter)
* [C# betik - zaman uyumsuz](#c-script-template-example---asynchronous)
* [C# betik - JSON](#c-script-template-example---json)
* [C# betik - kitaplık türleri](#c-script-template-example---library-types)
* [F#](#f-template-example)
* [JavaScript](#javascript-template-example)

### <a name="c-script-template-example---out-parameter"></a>C# betik şablon örnek - çıkış parametresi

Bu örnek, bir bildirim gönderir bir [şablonu kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren bir `message` şablonunda yer tutucu.

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

### <a name="c-script-template-example---asynchronous"></a>C# betik şablon örnek - zaman uyumsuz

Zaman uyumsuz kod kullanıyorsanız, out Parametreleri izin verilmiyor. Bu durumda kullanarak `IAsyncCollector` , şablon bildirim dönün. Aşağıdaki kod, yukarıdaki kod zaman uyumsuz bir örnektir. 

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

### <a name="c-script-template-example---json"></a>C# betik şablon örnek - JSON

Bu örnek, bir bildirim gönderir bir [şablonu kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren bir `message` kullanarak geçerli bir JSON dizesi şablonunda yer tutucu.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

### <a name="c-script-template-example---library-types"></a>C# betik şablon örnek - kitaplık türleri

Bu örnek tanımlanan türleri kullanmayı gösterir [Microsoft Azure bildirim hub'ları Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

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

### <a name="f-template-example"></a>F # şablon örneği

Bu örnek, bir bildirim gönderir bir [şablonu kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren `location` ve `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

### <a name="javascript-template-example"></a>JavaScript şablon örneği

Bu örnek, bir bildirim gönderir bir [şablonu kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren `location` ve `message`.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
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

Bu C# betik örnek yerel APNS bildirim göndermek nasıl gösterir. 

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

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="example---gcm-native"></a>Örnek - GCM yerel

Bu C# betik örnek yerel GCM bildirim göndermek nasıl gösterir. 

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
    // The JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="example---wns-native"></a>Örnek - WNS yerel

Bu C# kod örneğinde tanımlanan türleri kullanmayı gösterir [Microsoft Azure bildirim hub'ları Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) yerel WNS bildirim göndermek için. 

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

Özniteliğin Oluşturucusu parametreleri ve özellikleri açıklanan [yapılandırma](#configuration) bölümü.

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `NotificationHub` özniteliği:

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** |yok| "NotificationHub" olarak ayarlanmalıdır. |
|**direction** |yok| Out"için" olarak ayarlanmalıdır. | 
|**Adı** |yok| Bildirim hub'ı ileti için işlevi kod içinde kullanılan değişken adı. |
|**tagExpression** |**TagExpression** | Etiket ifadeleri bildirimleri etiketi ifadeyle eşleşecek bildirimleri almak için kayıtlı cihazlar kümesine teslim edilmesi belirtmenizi sağlar.  Daha fazla bilgi için bkz: [Yönlendirme ve etiket ifadeleri](../notification-hubs/notification-hubs-tags-segment-push-message.md). |
|**hubName** | **hubName** | Azure Portalı'ndaki bildirim hub'ı kaynağının adı. |
|**Bağlantı** | **ConnectionStringSetting** | Bildirim hub'ları bağlantı dizesi içeren bir uygulama ayarı adı.  Bağlantı dizesi ayarlanmalıdır *DefaultFullSharedAccessSignature* bildirim hub'ınız için değer. Bkz: [bağlantı dizesi kurulumu](#connection-string-setup) bu makalenin ilerisinde yer.|
|**Platform** | **Platform** | Platform özelliği istemci platformu bildirim hedeflerinizi gösterir. Platform özellik çıkış bağlamanın dışında atlanırsa varsayılan olarak, şablon bildirim Azure bildirim hub'ına yapılandırılmış herhangi bir platform hedeflemek için kullanılabilir. Çapraz bir Azure bildirim hub'ı ile platform bildirimleri göndermek için genel şablonları kullanma hakkında daha fazla bilgi için bkz: [şablonları](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Ayarlandığında, **platform** aşağıdaki değerlerden biri olmalıdır: <ul><li><code>apns</code>&mdash;Apple anında bildirim hizmeti. APNS için bildirim hub'ı yapılandırma ve bir istemci uygulamasında bildirim alma hakkında daha fazla bilgi için bkz: [Azure Notification Hubs ile ios'a gönderme anında iletme bildirimleri](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md).</li><li><code>adm</code>&mdash;[Amazon Device Messaging](https://developer.amazon.com/device-messaging). ADM için bildirim hub'ı yapılandırma ve bir Kindle uygulaması bildirim alma hakkında daha fazla bilgi için bkz: [Kindle uygulamaları için Notification Hubs ile çalışmaya başlama](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md).</li><li><code>gcm</code>&mdash;[Google Cloud Messaging'i](https://developers.google.com/cloud-messaging/). Firebase bulut GCM yeni sürümü olan ileti, da desteklenir. Daha fazla bilgi için bkz: [Azure Notification Hubs ile android'e gönderme anında iletme bildirimleri](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md).</li><li><code>wns</code>&mdash;[Windows anında bildirim Hizmetleri](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) Windows platformları hedefleme. Windows Phone 8.1 ve üzeri WNS tarafından da desteklenir. Daha fazla bilgi için bkz: [uygulamaları için Notification Hubs Windows Evrensel Platform ile çalışmaya başlama](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</li><li><code>mpns</code>&mdash;[Microsoft anında bildirim hizmeti](https://msdn.microsoft.com/library/windows/apps/ff402558.aspx). Bu platform, Windows Phone 8 ve daha önceki Windows Phone platformlarını destekler. Daha fazla bilgi için bkz: [Windows Phone üzerinde Azure Notification Hubs ile anında iletme bildirimleri gönderme](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md).</li></ul> |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="functionjson-file-example"></a>Function.JSON dosyası örneği

Örneği bir bildirim hub'ları bağlamanın bir *function.json* dosya.

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
      "platform": "gcm"
    }
  ],
  "disabled": false
}
```

### <a name="connection-string-setup"></a>Bağlantı dizesi kurulumu

Bağlama bildirim hub'ı çıkışı kullanmak için bağlantı dizesi hub için yapılandırmanız gerekir. Varolan bir bildirim hub'ı seçin veya yeni bir sağ üstten oluşturma *tümleştir* Azure portalında sekmesi. Bağlantı dizesi el ile de yapılandırabilirsiniz. 

Varolan bir bildirim hub'ına bağlantı dizesini yapılandırmak için:

1. Bildirim hub'ınıza gidin [Azure portal](https://portal.azure.com), seçin **erişim ilkeleri**ve ardından Kopyala düğmesini işaretleyin **DefaultFullSharedAccessSignature** ilkesi. Bu bağlantı dizesini kopyalar *DefaultFullSharedAccessSignature* bildirim hub'ınıza ilkesi. Bu bağlantı dizesi hub'ına bildirim iletilerini göndermek, işlev sağlar.
    ![Bildirim hub'ı bağlantı dizesini kopyalayın](./media/functions-bindings-notification-hubs/get-notification-hub-connection.png)
1. Azure portalında işlevi uygulamanıza gidin, seçin **uygulama ayarları**, bir anahtar ekleyin **MyHubConnectionString**, kopyalanan Yapıştır *DefaultFullSharedAccessSignature* değeri ve ardından olarak bildirim hub'ınız için **kaydetmek**.

Bu uygulama ayarını çıkış bağlama bağlantı ayarında unsurları adıdır *function.json* veya .NET özniteliği. Bkz: [yapılandırma bölümü](#configuration) bu makalenin önceki.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Notification Hub'ı | [İşlemler Kılavuzu](https://docs.microsoft.com/rest/api/notificationhubs/) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

