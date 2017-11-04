---
title: "Azure Mobile Engagement Web SDK genel bakış | Microsoft Docs"
description: "En son güncelleştirmeleri ve Azure Mobile Engagement için Web SDK'sı için yordamlar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 5bbc2fda-0f3f-43d0-a73d-0f2c0f8dc25b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 10/18/2016
ms.author: piyushjo
ms.openlocfilehash: 770a83131a3e661771db50b22ce7de25b2d541cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a>Azure Mobile Engagement Web SDK
Bir web uygulamasını Azure Mobile Engagement tümleştirme hakkında tüm ayrıntılar için buradan başlayın. İsterseniz, kendi web uygulaması ile çalışmaya başlama önce bir denemelisiniz bkz bizim [15 dakikalık Öğreticisi](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Tümleştirme yordamları
1. Bilgi [web uygulamanızı Mobile Engagement tümleştirme](mobile-engagement-web-integrate-engagement.md).
2. Etiket planı uygulama için bilgi [API web uygulamanızda etiketleme Gelişmiş Mobile Engagement kullanmayı](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Sürüm notları
### <a name="202-10182016"></a>2.0.2 (10/18/2016)
* Özel (Safari) gözatma sabit kilitlenme.
* Sabit kilitlenme tarayıcılarında tanımlama bilgileri devre dışı ile.

Tüm sürümler için lütfen bkz. [tamamlamak sürüm notları](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Yükseltme yordamları
### <a name="upgrade-from-121-to-200"></a>1.2.1 2.0.0 için yükseltme
Aşağıdaki bölümlerde, nasıl Azure Mobile Engagement uygulamayı Capptain SAS tarafından sunulan Capptain hizmetinden bir Mobile Engagement Web SDK tümleştirmesi geçirileceği açıklanmaktadır. 1.2.1'den önceki bir sürümden geçiriyorsanız, Lütfen 1.2.1 için ilk geçirmek için Capptain Web sitesine bakın ve ardından aşağıdaki yordamları uygulayın.

Mobile Engagement Web SDK'ın bu sürümü Samsung akıllı TV, Opera TV, webOS veya Reach özelliğini desteklemiyor.

> [!IMPORTANT]
> Aynı hizmeti Capptain ve Azure Mobile Engagement değildir ve aşağıdaki yordamları istemci uygulaması yalnızca nasıl geçirileceği vurgulayın. Mobile Engagement Web SDK uygulamasında geçirme verilerinizi Capptain sunucusundan bir Mobile Engagement sunucuya geçişi yapılmaz.
> 
> 

#### <a name="javascript-files"></a>JavaScript dosyaları
Dosya capptain sdk.js dosya azure engagement.js ile değiştirin ve ardından komut dosyasını içeri aktarmalar uygun şekilde güncelleştirin.

#### <a name="remove-capptain-reach"></a>Capptain Reach Kaldır
Mobile Engagement Web SDK'ın bu sürümü ulaşma özelliği desteklemiyor. Uygulamanıza Capptain ulaşma bütünleştirdiyseniz kaldırmanız gerekir.

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

#### <a name="remove-deprecated-apis"></a>Kullanım dışı API'leri Kaldır
Bazı Capptain API'lerden Mobile Engagement Web SDK'ın kullanım dışı bırakılmıştır.

Aşağıdaki API'leri tüm çağrıları kaldırın: `agent.connect`, `agent.disconnect`, `agent.pause`, ve `agent.sendMessageToDevice`.

Aşağıdaki geri çağırmaları hiçbirini Capptain yapılandırmasından kaldırın: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, ve `onPushMessageReceived`.

#### <a name="configuration"></a>Yapılandırma
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

#### <a name="javascript-apis"></a>JavaScript API'leri
Genel JavaScript nesne `window.capptain` adlandırıldı `window.azureEngagement`, ancak kullanabilirsiniz `window.engagement` API çağrıları için diğer ad. SDK yapılandırmasını tanımlamak için bu diğer ad kullanılamıyor.

Örneğin, `capptain.deviceId` hale `engagement.deviceId`, `capptain.agent.startActivity` hale `engagement.agent.startActivity`ve benzeri.

Lütfen okuyun, uygulamasına Azure Mobile Engagement Web SDK önceki bir sürümü zaten bütünleştirdiyseniz [yükseltme yordamları](mobile-engagement-web-upgrade-procedure.md).

