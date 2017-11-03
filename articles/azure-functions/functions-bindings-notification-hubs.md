---
title: "Azure işlevleri bildirim hub'ı bağlama | Microsoft Docs"
description: "Azure bildirim hub'ı bağlama Azure işlevlerini kullanmak nasıl anlayın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: 0ff0c949-20bf-430b-8dd5-d72b7b6ee6f7
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: glenga
ms.openlocfilehash: 02d01d0f6e945ed54dbe766aec2a0fd7c17c510f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Azure işlevleri bildirim hub'ı bağlama çıktı
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede nasıl yapılandırılacağı ve kod Azure bildirim hub'ı bağlamaları Azure işlevlerinde açıklanmaktadır. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

İşlevlerinizi birkaç kod satırıyla, bir yapılandırılmış Azure bildirim hub'ı kullanarak anında iletme bildirimleri gönderebilirsiniz. Ancak, Azure bildirim hub'ı Platform Bildirim Hizmetleri (kullanmak istediğiniz PNS için) yapılandırılması gerekir. Bir Azure bildirim hub'ı yapılandırma ve bildirimleri almak için kaydolun istemci uygulamaları geliştirme hakkında daha fazla bilgi için bkz: [Notification Hubs ile çalışmaya başlama](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) ve hedef istemci platformunuzu üst'ı tıklatın .

Gönderdiğiniz bildirimleri yerel bildirimleri veya şablon bildirimleri olabilir. Yerel bildirimler yapılandırılan belirli bildirim platform hedef `platform` çıkış bağlama özelliği. Bir şablon bildirim hedef birden çok platform için kullanılabilir.   

## <a name="notification-hub-output-binding-properties"></a>Bildirim hub'ı çıkış bağlama özellikleri
Function.json dosyası aşağıdaki özellikleri sağlar:


|Özellik  |Açıklama  |
|---------|---------|
|**adı** | Bildirim hub'ı ileti için işlevi kod içinde kullanılan değişken adı. |
|**türü** | ayarlanmalıdır `notificationHub`. |
|**tagExpression** | Etiket ifadeleri bildirimleri etiketi ifadeyle eşleşecek bildirimleri almak için kayıtlı cihazları kümesine teslim edilmesi belirtmenizi sağlar.  Daha fazla bilgi için bkz: [Yönlendirme ve etiket ifadeleri](../notification-hubs/notification-hubs-tags-segment-push-message.md). |
|**hubName** | Azure Portalı'ndaki bildirim hub'ı kaynağının adı. |
|**bağlantı** | Bu bağlantı dizesi olmalıdır bir **uygulama ayarı** bağlantı dizesini ayarlamak *DefaultFullSharedAccessSignature* bildirim hub'ınız için değer. |
|**yönü** | ayarlanmalıdır `out`. | 
|**Platform** | Platform özelliği bildirim platform bildirim hedeflerinizi gösterir. Platform özellik çıkış bağlamanın dışında atlanırsa varsayılan olarak, şablon bildirim Azure bildirim hub'ına yapılandırılmış herhangi bir platform hedeflemek için kullanılabilir. Çapraz bir Azure bildirim hub'ı ile platform bildirimleri göndermek için genel şablonları kullanma hakkında daha fazla bilgi için bkz: [şablonları](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md). Ayarlandığında, _platform_ aşağıdaki değerlerden biri olmalıdır: <ul><li><code>apns</code>&mdash;Apple anında bildirim hizmeti. APNS için bildirim hub'ı yapılandırma ve bir istemci uygulamasında bildirim alma hakkında daha fazla bilgi için bkz: [Azure Notification Hubs ile ios'a gönderme anında iletme bildirimleri](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md).</li><li><code>adm</code>&mdash;[Amazon Device Messaging](https://developer.amazon.com/device-messaging). ADM için bildirim hub'ı yapılandırma ve bir Kindle uygulaması bildirim alma hakkında daha fazla bilgi için bkz: [Kindle uygulamaları için Notification Hubs ile çalışmaya başlama](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md).</li><li><code>gcm</code>&mdash;[Google Cloud Messaging'i](https://developers.google.com/cloud-messaging/). Firebase bulut GCM yeni sürümü olan ileti, da desteklenir. Daha fazla bilgi için bkz: [Azure Notification Hubs ile android'e gönderme anında iletme bildirimleri](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md).</li><li><code>wns</code>&mdash;[Windows anında bildirim Hizmetleri](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) Windows platformları hedefleme. Windows Phone 8.1 ve üzeri WNS tarafından da desteklenir. Daha fazla bilgi için bkz: [uygulamaları için Notification Hubs Windows Evrensel Platform ile çalışmaya başlama](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</li><li><code>mpns</code>&mdash;[Microsoft anında bildirim hizmeti](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Bu platform, Windows Phone 8 ve daha önceki Windows Phone platformlarını destekler. Daha fazla bilgi için bkz: [Windows Phone üzerinde Azure Notification Hubs ile anında iletme bildirimleri gönderme](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md).</li></ul> |

Örnek function.json:

```json
{
  "bindings": [
    {
      "name": "notification",
      "type": "notificationHub",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "gcm",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="notification-hub-connection-string-setup"></a>Bildirim hub'ı bağlantı dizesi kurulumu
Bağlama bildirim hub'ı çıkışı kullanmak için bağlantı dizesi hub için yapılandırmanız gerekir. Varolan bir bildirim hub'ı seçin veya yeni bir sağ üstten oluşturmak *tümleştir* işlevinizi sekmesindedir. Bağlantı dizesi el ile de yapılandırabilirsiniz. 

Varolan bir bildirim hub'ına bağlantı dizesini yapılandırmak için:

1. Bildirim hub'ınıza gidin [Azure portal](https://portal.azure.com), seçin **erişim ilkeleri**ve ardından Kopyala düğmesini işaretleyin **DefaultFullSharedAccessSignature** ilkesi. Bu bağlantı dizesini kopyalar *DefaultFullSharedAccessSignature* bildirim hub'ınıza ilkesi. Bu bağlantı dizesi, bildirim iletilerini göndermek için işlevi erişim izniniz sağlar. 
    ![Bildirim hub'ı bağlantı dizesini kopyalayın](./media/functions-bindings-notification-hubs/get-notification-hub-connection.png)
1. Azure portalında işlevi uygulamanıza gidin, seçin **uygulama ayarları**, bir anahtar ekleyin `MyHubConnectionString`, kopyalanan Yapıştır *DefaultFullSharedAccessSignature* bildirim hub'ınız için değer ve ardından **kaydetmek**.

Artık bildirim hub bağlantısı çıkış bağlamasında tanımlar bu adlandırılmış uygulama ayarı kullanabilirsiniz.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>C# queue tetikleyicileri ile APNS yerel bildirimleri
Bu örnek tanımlanan türleri kullanmayı gösterir [Microsoft Azure bildirim hub'ları Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) yerel APNS bildirim göndermek için. 

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

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>C# queue tetikleyicileri ile GCM yerel bildirimleri
Bu örnek tanımlanan türleri kullanmayı gösterir [Microsoft Azure bildirim hub'ları Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) yerel GCM bildirim göndermek için. 

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

## <a name="wns-native-notifications-with-c-queue-triggers"></a>C# queue tetikleyicileri ile WNS yerel bildirimleri
Bu örnek tanımlanan türleri kullanmayı gösterir [Microsoft Azure bildirim hub'ları Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) yerel WNS bildirim göndermek için. 

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

## <a name="template-example-for-nodejs-timer-triggers"></a>Şablon örneği için Node.js Zamanlayıcı Tetikleyicileri
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

## <a name="template-example-for-f-timer-triggers"></a>F # Zamanlayıcı Tetikleyicileri şablon Örneğin
Bu örnek, bir bildirim gönderir bir [şablonu kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren `location` ve `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a>Out parametresi kullanılarak şablon örneği
Bu örnek, bir bildirim gönderir bir [şablonu kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren bir `message` şablonunda yer tutucusu.

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

## <a name="template-example-with-asynchronous-function"></a>Şablon örneği ile zaman uyumsuz işlevi
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

## <a name="template-example-using-json"></a>JSON kullanarak şablon örneği
Bu örnek, bir bildirim gönderir bir [şablonu kayıt](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) içeren bir `message` kullanarak geçerli bir JSON dizesi şablonunda yer tutucusu.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Şablon örneği bildirim hub'ları kitaplığı türleri kullanma
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

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

