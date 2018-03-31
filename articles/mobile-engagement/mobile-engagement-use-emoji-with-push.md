---
title: Azure Mobile Engagement içinde Emoji ifadeler kullanın
description: Anında iletme bildirimleri içinde Emoji ifadeler kullanma
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 663317d7-3c93-4e8f-b13d-c6fb342124ee
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b5b0e7bfe07054d093dc164cb5f72bde4ba28170
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="use-emoji-emoticon-within-push-notifications"></a>Anında iletme bildirimleri içinde Emoji ifade kullanın
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Emoji içerebilir, ifadeleri anında iletilen bildirim birkaç kolay adımda: 

1. Tüm Emoji bulmanız gereken ilk ileti göndermek istiyor. Lütfen cihaz materyalleri üreten cihaz platformları için yeni onaylanmış Emojis eklemek için biraz zaman olarak seçmiş olursunuz Emoji hedef aygıt tarafından desteklenen emin olun. 
2. Üzerinde **Windows** -bu gidebilirsiniz [bağlantı](http://apps.timwhitlock.info/emoji/tables/unicode) ve Kopyala 'Native' simgesi.
   
    ![][7] 
3. Üzerinde **Mac** -bulabileceğiniz Emoji & Simgeleri Düzenle altında sözlük uygulamada Emojis ->.
   
    ![][6] 
4. Şimdi, Git **ulaşmak** sekmesi Azure Mobile Engagement portalına. (Duyuru yoklar vb.), anında iletme bildirimi türünü seçin. Bu örnek için bir duyuru itme seçin.
5. Bildirim metninde ulaşana kadar bildirim farklı alanları belirtin. Burada Emoji ifadeniz ekleyeceksiniz budur. Başlık, ileti veya her ikisi de put seçebilirsiniz. Sürükleme ve bırakma ya da yukarıdaki konumlardan bulmak Emoji kopyalama gerekecektir. 
   
    ![][1]
6. Bildirim için diğer alanları doldurun ve kaydedin. 
7. Test çalıştırma veya duyuru etkinleştirdiğinizde belirtildiği şekilde ifadeyi içeren bir bildirim görürsünüz.   
   
    ![][3] ![][4] ![][5]

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

