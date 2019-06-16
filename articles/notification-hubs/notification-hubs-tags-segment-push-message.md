---
title: Yönlendirme ve etiket ifadeleri
description: Bu konuda Azure bildirim hub'ları için Yönlendirme ve etiket ifadeler açıklanmaktadır.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2019
ms.author: jowargo
ms.openlocfilehash: 31a22aabc7b0f1d51a673ef8642037103badcc02
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61457830"
---
# <a name="routing-and-tag-expressions"></a>Yönlendirme ve etiket ifadeleri

## <a name="overview"></a>Genel Bakış

Etiket ifadeleri için hedef belirli ayarlar veya kayıtları daha açık belirtmek gerekirse, cihaz, Notification Hubs ile anında iletme bildirimi gönderirken etkinleştirin.

## <a name="targeting-specific-registrations"></a>Belirli hedefleme

Hedef için tek yolu kayıtları olduğunu etiketleri bunları ile ilişkilendirmek için belirli bildirim sonra bu etiketleri hedefleyin. Bölümünde açıklandığı gibi [kayıt yönetimi](notification-hubs-push-notification-registration-management.md), anında iletme bildirimleri bir uygulamanın sahip bir cihaz kaydetmek bir bildirim hub'ındaki ele almak için. Bir kaydı bir bildirim hub'ındaki oluşturulduktan sonra uygulama arka ucu için anında iletme bildirimleri gönderebilirsiniz. Uygulama arka ucu belirli bir bildirim ile hedeflenecek kayıtları aşağıdaki yollarla seçebilirsiniz:

1. **Yayın**: bildirim hub'ında tüm kayıtları bir bildirim alır.
2. **Etiket**: belirtilen etiketi içeren tüm kayıtları bir bildirim alır.
3. **Etiket ifadesi**: bildirim, etiket kümesinin belirtilen ifade eşleşen tüm kayıtlar alır.

## <a name="tags"></a>Tags

Bir etiketi alfasayısal içeren en fazla 120 karakter ve şu alfasayısal olmayan karakterleri bir dize olabilir: '_', ' @', '#', '. ',':', '-'. Aşağıdaki örnek, belirli müzik grupları hakkında kutlama bildirimleri almak için bir uygulama gösterir. Bu senaryoda, aşağıdaki resimde olduğu gibi farklı bantları temsil eden etiketlere sahip etiket kayıtları bildirimlerinin yönlendirileceği basit bir yolu şudur:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Bu resimde, ileti etiketli **Beatles** etiket ile kayıtlı tablet ulaştığında **Beatles**.

Etiketleri için kayıtları oluşturma hakkında daha fazla bilgi için bkz. [kayıt yönetimi](notification-hubs-push-notification-registration-management.md).

Etiketleri Gönder bildirimleri yöntemlerini kullanarak bildirim gönderebilirsiniz `Microsoft.Azure.NotificationHubs.NotificationHubClient` sınıfını [Microsoft Azure Notification hubs'ı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Ayrıca, Node.js veya anında iletme bildirimleri REST API de kullanabilirsiniz.  SDK'sını kullanarak bir örnek aşağıda verilmiştir.

```csharp
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
"You requested a Beatles notification</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

// Windows 10
toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
"You requested a Wailers notification</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");
```

Etiketler, önceden sağlanmış olması gerekmez ve birden çok uygulamaya özgü kavramlara başvurabilir. Örneğin, bu örnek uygulama kullanıcılarının bant üzerinde yorum ve toasts, yalnızca kendi sık kullanılan bantları açıklamaları için aynı zamanda tüm açıklamaları için bunlar yorum bant bağımsız olarak kullanıcıların arkadaşlarını almak istiyorsunuz. Aşağıdaki resimde, bu senaryoya örnek olarak gösterilmiştir:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Bu resimdeki Alice Beatles güncelleştirmeleri ilginizi ve Bob Wailers güncelleştirmeleri ilginizi. Bob ayrıca Cahit'ın açıklamalarda ilgi ve Cahit içinde Wailers ilgileniyor. Bir bildirim Beatles Cahit'ın ederken açıklama gönderildiğinde, hem Alice ve Bob alırsınız.

Etiket (örneğin, "band_Beatles" veya "follows_Charlie") içinde birden çok kaygıları kodlayabilir, ancak etiketleri basit dizeler ve değerlerle Özellikleri ' dir. Bir kayıt yalnızca varlığı veya yokluğu belirli bir etiketi üzerinde eşleştirilir.

İlgi alanı gruplarına göndermek için etiketleri kullanma hakkında tam adım adım bir öğretici için bkz: [bozucu haber](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

> [!NOTE]
> Azure Notification Hubs kayıt başına 60 etiket en fazla destekler.

## <a name="using-tags-to-target-users"></a>Hedef kullanıcı için etiketleri kullanma

Etiketleri kullanmak için başka bir yolu, belirli bir kullanıcının tüm cihazları belirlemektir. Kayıtları, aşağıdaki resimde olduğu gibi bir kullanıcı Kimliğini içeren bir etiketi ile etiketlenebilir:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Bu resimde, ileti uid etiketli: Alice ulaştığında tüm kayıtları etiketli "uid:Alice"; Bu nedenle, tüm Alice'in cihazları.

## <a name="tag-expressions"></a>Etiket ifadeleri

Tek bir etiket tarafından değil, ancak etiketleri hakkında bir Boole ifadesi tarafından tanımlanan bir dizi kayıtları hedeflemek bir bildirim sahip olduğu durumlar vardır.

Kırmızı Sox Cardinals arasındaki bir oyun hakkında Boston herkese bir anımsatıcı gönderir bir spor uygulamayı düşünün. İstemci uygulaması etiketler takımlar ve konumu hakkında kaydederse, ardından bildirim kırmızı Sox veya Cardinals ilgileniyor Boston herkes için hedeflenmesi. Bu durum aşağıdaki Boole ifadesi ile belirtilebilir:

```csharp
(follows_RedSox || follows_Cardinals) && location_Boston
```

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Etiket ifadeleri içerebilir tüm Boole işleçleri gibi AND (& &), veya (|) ve NOT (!). Bunlar ayrıca parantez içerebilir. Bunlar yalnızca ORs içeriyorsa, etiket ifadeleri 20 etiket sınırlıdır; Aksi halde bunlar 6 etiketleri sınırlı olursunuz.

Etiket ifadeleri SDK'sını kullanarak bildirim göndermek için bir örnek aşağıda verilmiştir.

```csharp
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

String userTag = "(location_Boston && !follows_Cardinals)";

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
"You want info on the Red Sox</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

// Windows 10
toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
"You want info on the Red Sox</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
```
