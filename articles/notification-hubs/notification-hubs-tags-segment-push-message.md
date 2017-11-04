---
title: "Yönlendirme ve etiket ifadeleri"
description: "Bu konu Azure bildirim hub'ları için Yönlendirme ve etiket ifadeleri açıklar."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 18faa88641623e1248d6a33bc2d87099e1c9f624
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="routing-and-tag-expressions"></a>Yönlendirme ve etiket ifadeleri
## <a name="overview"></a>Genel Bakış
Etiket ifadeleri hedef belirli kümelerini cihazlar ya da daha açık belirtmek gerekirse kayıtlar için bildirim hub'ları üzerinden bir anında iletme bildirimi gönderirken etkinleştirin.

## <a name="targeting-specific-registrations"></a>Belirli kayıtları hedefleme
Hedef tek yolu kayıtlar etiketleri bunları ile ilişkilendirmek için belirli bildirimini sonra bu etiketlerin hedefleyin. ' Da anlatıldığı gibi [kayıt yönetimi](notification-hubs-push-notification-registration-management.md), anında iletme bildirimleri bir uygulama olan bir cihazı kaydetmek bir bildirim hub'ına ele almak için. Bir kaydı bir bildirim hub'ına oluşturulduktan sonra uygulama arka uç anında iletme bildirimleri gönderebilirsiniz.
Uygulama arka ucu belirli bir bildirim hedeflenecek kayıtlar aşağıdaki yollarla seçebilirsiniz:

1. **Yayın**: bildirim hub'ındaki tüm kayıtlar bir bildirim alır.
2. **Etiket**: Belirtilen etiket içeren tüm kayıtlar bir bildirim alır.
3. **Etiket ifade**: tüm kayıtlar, etiket kümesi, belirtilen ifade ile eşleşen bir bildirim alır.

## <a name="tags"></a>Etiketler
Bir etiketi herhangi bir dize, alfasayısal içeren en fazla 120 karakter ve alfasayısal olmayan karakterler olabilir: '_', ' @', '#', '. ',':', '-'. Aşağıdaki örnekte belirli müzik grupları hakkında bildirimleri alacak bir uygulamayı gösterir. Bu senaryoda, bir basit yol bildirimleri etiket kayıtları, aşağıdaki resimde olduğu gibi farklı bantları temsil eden etiketlere sahip yoludur.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Bu resim, ileti etiketli **Beatles** etiketiyle kayıtlı tablet ulaştığında **Beatles**.

Kayıtlar için etiketler oluşturma hakkında daha fazla bilgi için bkz: [kayıt yönetimi](notification-hubs-push-notification-registration-management.md).

Gönderme bildirimleri yöntemleri kullanılarak etiketleri bildirimleri gönderebilir `Microsoft.Azure.NotificationHubs.NotificationHubClient` sınıfını [Microsoft Azure bildirim hub'ları](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Node.js veya anında iletme bildirimleri REST API de kullanabilirsiniz.  Burada, SDK'yı kullanarak bir örnek verilmiştir.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Etiketleri önceden sağlanan olması gerekmez ve birden çok uygulamaya özgü kavramlara başvuruda bulunabilir. Örneğin, bu örnek uygulama kullanıcılarının bantlar üzerinde açıklama ve bunlar yorum bant bağımsız olarak kendi arkadaş bildirimleri, yalnızca kendi sık kullanılan bantlar üzerinde açıklamaları için aynı zamanda tüm yorumlar için almak istiyor. Aşağıdaki resimde bu senaryo örneği gösterilmektedir:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Bu resmi Alice Beatles güncelleştirmeleri ilgilendiği ve Bob Wailers güncelleştirmeleri ilgilendiği. Bob ayrıca Cahit'ın açıklamaları ilgilenen ve Cahit içinde Wailers ilgileniyor. Bir bildirim Beatles Cahit'ın açıklama gönderildiğinde, Alice ve Bob alırsınız.

Birden çok sorunları etiketleri (örneğin, "band_Beatles" veya "follows_Charlie") kodlamak olsa da, etiketleri basit dizeler ve özellikleri değerlerle ' dir. Bir kayıt yalnızca varlığının veya yokluğunun belirli bir tag üzerinde eşleştirilir.

İlgi alanı gruplarına göndermek için etiketleri kullanma konusunda tam adım adım öğretici için bkz: [sonu haber](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

## <a name="using-tags-to-target-users"></a>Hedef Kullanıcılar etiketleri kullanma
Etiketler kullanmanın başka bir yolu, belirli bir kullanıcının tüm cihazlar belirlemektir. Kayıtları, aşağıdaki resimde olduğu gibi bir kullanıcı kimliği içeren bir etiketle etiketlenebilir:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Bu resim, tüm kayıtlar etiketli uid:Alice etiketli ileti uid:Alice ulaştığında; Bu nedenle, tüm Alice'in cihazlarını.

## <a name="tag-expressions"></a>Etiket ifadeleri
Tek bir etiket tarafından değil, ancak etiketleri hakkında Boole ifadesi tarafından tanımlanan bir kayıt kümesine hedeflemek bir bildirim sahip olduğu durumlar vardır.

Kırmızı Sox ve Cardinals arasında oyun hakkında Boston herkese anımsatıcısı gönderir Spor uygulama göz önünde bulundurun. İstemci uygulaması etiketler ekipleri ve konum ilgi hakkında kaydederse, ardından bildirim herkesin kırmızı Sox veya Cardinals ilgilenmektedir Boston hedeflenecek. Bu durum aşağıdaki Boolean ifade ile ifade edilebilir:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Etiket ifadeleri içerebilir tüm Boole işleçleri gibi ve (& &), ya da (|) ve (!). Bunlar ayrıca parantez içerebilir. Yalnızca ORs içeriyorsa etiket ifadeleri 20 etiketleri sınırlıdır; Aksi halde bunlar 6 etiketleri sınırlıdır.

Burada, SDK'sını kullanarak etiketi ifadelerle bildirimleri göndermek için bir örnek verilmiştir.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)";    

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
