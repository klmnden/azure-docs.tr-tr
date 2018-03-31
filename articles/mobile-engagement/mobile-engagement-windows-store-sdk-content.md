---
title: Windows Evrensel uygulamaları SDK içeriği
description: Azure Mobile Engagement Windows Evrensel uygulamaları SDK içeriği hakkında bilgi edinin
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7b520adcc6af24f7b092580ea82a787a120668bf
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="windows-universal-apps-sdk-content"></a>Windows Evrensel uygulamaları SDK içeriği
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Bu belge listeler ve uygulamanızda SDK'sı tarafından dağıtılan içeriği açıklar.

## <a name="the-resources-folder"></a>`/Resources` Klasörü
Bu klasör, Mobile Engagement gereken tüm kaynakları içerir. Ayrıca bunları uygulamanızı uyacak şekilde özelleştirebilirsiniz.

* `EngagementConfiguration.xml` : Mobile Engagement'ın yapılandırma dosyası, Mobile Engagement ayarlarını (Mobile Engagement bağlantı dizesi, rapor kilitlenme...) burada özelleştirebilirsiniz budur.

### <a name="html-folder"></a>/HTML klasörü
* `EngagementNotification.html` `Notification` Web görünümü html tasarımını uygulama başlıkları için.
* `EngagementAnnouncement.html` `Announcement` Web görünümü html tasarımını uygulama Interstitial görünümler için.

### <a name="images-folder"></a>Resim klasörü
* `EngagementIconNotification.png` : Bir bildirim sol tarafında görüntülenen marka simge bunu, marka simgesiyle değiştirin.
* `EngagementIconOk.png` `Ok` Reach içerik sayfalarının eylem veya doğrulama düğmesi için simge.
* `EngagementIconNOK.png` `NOK` Reach içerik sayfalarının doğrulama düğmesi devre dışı bırakıldığında kullanılan simge.
* `EngagementIconClose.png` `Close` Ulaşma bildirim ve içerikleri Atla düğmesi için simge.

### <a name="overlay-folder"></a>/overlay klasörü
* `EngagementPageOverlay.cs` : Katılım eklemek için sorumlu katmana sayfa alt uygulama UI ulaşabilirsiniz.

