---
title: "Azure Mobile Engagement Web SDK yükseltme yordamları | Microsoft Docs"
description: "En son güncelleştirmeleri ve Azure Mobile Engagement için Web SDK'sı için yordamlar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a20529b4-ec8d-4503-8ae9-09b5f0846d5b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: afa8037dcb7a53042fa606e2c4014b442d4be326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile Engagement Web SDK yükseltme yordamları
Web uygulamasına Azure Mobile Engagement Web SDK önceki bir sürümü zaten bütünleştirdiyseniz, SDK'yı yükselttiğinizde aşağıdaki noktaları dikkate almanız gerekir.

Mobile Engagement Web SDK birden fazla sürümünü atladıysanız, yükseltme işlemi sırasında birçok yordamı tamamlamak gerekebilir. Örneğin, 1.4.0 1.6.0 için geçiş işlemi gerçekleştirirseniz, ilk 1.4.0 1.5.0 için yükseltme yordamları izleyin. Ardından 1.5.0 için 1.6.0 sürümüne yükseltme yordamları izleyin.

Yükseltme, hangi sürümü dosya azure engagement.js önceki sürümünü dosyasının en son sürümle değiştirin.

## <a name="upgrade-from-121-to-200"></a>1.2.1 2.0.0 için yükseltme
Bu bölümde, nasıl Azure Mobile Engagement uygulamayı Capptain SAS tarafından sunulan Capptain hizmetinden bir Mobile Engagement Web SDK tümleştirmesi geçirileceği açıklanmaktadır. Önceki bir sürümünden geçiş yapıyorsanız, Lütfen ilk 1.2.1 için geçirmek için Capptain Web sitesine bakın ve ardından aşağıdaki yordamları uygulayın.

Mobile Engagement Web SDK'ın bu sürümü Samsung akıllı TV, Opera TV, webOS veya Reach özelliğini desteklemiyor.

> [!IMPORTANT]
> Capptain ve Azure Mobile Engagement aynı hizmeti değildir. Aşağıdaki yordam, istemci uygulaması yalnızca nasıl geçirileceği vurgular. Mobile Engagement Web SDK uygulamasında geçirme verilerinizi Capptain sunucusundan bir Mobile Engagement sunucuya geçişi yapılmaz.
> 
> 

### <a name="javascript-files"></a>JavaScript dosyaları
Dosya capptain sdk.js dosya azure engagement.js ile değiştirin ve ardından komut dosyasını içeri aktarmalar uygun şekilde güncelleştirin.

### <a name="remove-capptain-reach"></a>Capptain Reach Kaldır
Mobile Engagement Web SDK'ın bu sürümü ulaşma özelliği desteklemiyor. Uygulamanıza Capptain erişim'i bütünleştirdiyseniz kaldırmanız gerekir.

CSS ulaşmak alma sayfanızdan kaldırın ve ilgili .css dosyasını (capptain-reach.css, varsayılan olarak) silin.

Aşağıdaki ulaşma kaynakları silin: Kapat görüntünün (capptain-close.png, varsayılan olarak) ve marka simgesi (capptain bildirim-simgesi, varsayılan olarak).

Uygulama bildirimleri için ulaşmak UI kaldırın. Varsayılan düzen şöyle görünür:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Metin ve web duyuruları ve yoklamaları ulaşmak UI kaldırın. Varsayılan düzen şöyle görünür:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Kaldırma `reach` varsa, yapılandırmasından nesne. Şöyle görünür:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Tüm diğer ulaşma özelleştirme, kategorileri gibi kaldırın.

### <a name="remove-deprecated-apis"></a>Kullanım dışı API'leri Kaldır
Bazı Capptain API'lerden Mobile Engagement Web SDK'ın kullanım dışı bırakılmıştır.

Aşağıdaki API'leri tüm çağrıları kaldırın: `agent.connect`, `agent.disconnect`, `agent.pause`, ve `agent.sendMessageToDevice`.

Aşağıdaki geri çağırmaları tüm örneklerini Capptain yapılandırmasından kaldırın: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, ve `onPushMessageReceived`.

### <a name="configuration"></a>Yapılandırma
Mobile Engagement SDK'sı tanımlayıcıları, örneğin, uygulama tanımlayıcısı yapılandırmak için bir bağlantı dizesi kullanır.

Uygulama kimliği, bağlantı dizesi ile değiştirin. SDK yapılandırması için genel nesne değişiklikleri Not `capptain` için `azureEngagement`.

Geçişten önce:

    window.capptain = {
      appId: ...,
      [...]
    };

Geçişten sonra:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Bağlantı dizesi, uygulamanız için Azure Portalı'nda görüntülenir.

### <a name="javascript-apis"></a>JavaScript API'leri
Genel JavaScript nesne `window.capptain` adlandırıldı `window.azureEngagement` ancak kullanabilirsiniz `window.engagement` API çağrıları için diğer ad. SDK yapılandırmasını tanımlamak için diğer adı kullanamazsınız.

Örneğin, `capptain.deviceId` hale `engagement.deviceId`, `capptain.agent.startActivity` hale `engagement.agent.startActivity`ve benzeri.

