---
title: "Azure Mobile Engagement iOS SDK sürüm notları | Microsoft Docs"
description: "En son güncelleştirmeler ve iOS için Azure Mobile Engagement SDK'sı için yordamlar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 9bdaa57f9902373ccf796ff109332b64c66bf9e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Azure Mobile Engagement iOS SDK'sı sürüm notları

## <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Sabit rozetleri arka planda temizlendi.
* Sabit uyarılar ana sırada adlı değil API'leri hakkında XCode 9.
* Bellek sızıntısı ulaşma yoklamalarda sabit.
* İOS için destek bırakılan 6.X. Uygulamanızın dağıtım hedef bu sürümünden başlayarak olmalıdır en az iOS 7.

## <a name="401-12132016"></a>4.0.1 (12/13/2016)
* Arka planda günlük teslim geliştirildi.

## <a name="400-09122016"></a>4.0.0 (09/12/2016)
* İOS 10 cihazlarda değil işleme alınan sabit bildirimi.
* XCode 7 alanı onaylanamadı.

## <a name="324-06302016"></a>3.2.4 (06/30/2016)
* Sabit toplama teknik günlükleri ve diğer günlükler arasında.

## <a name="323-06072016"></a>3.2.3 (06/07/2016)
* Hata, uygulama arka planda olduğunda burada teslim geri bildirilmedi sabit.
* Teknik günlükleri gönderme en iyi duruma getirilmiş.

## <a name="322-04072016"></a>3.2.2 (04/07/2016)
* Sabit hatanın bazen çökmesine müşteri adayları, HTTP isteği iptal üzerinde.

## <a name="321-12112015"></a>3.2.1 (12/11/2015)
* Yeni bir uygulama örneği ayrıntılı bağlantılar içeren bir bildirim tarafından tetiklendiğinde gecikme sabit

## <a name="320-10082015"></a>3.2.0 (10/08/2015)
* SDK'sı ile çalışması için Bitcode'u etkin **Xcode 7**.
* Düzeltilen hatalar için uygulama bildirimleri ile ilgili.
* Uygulama bildirimleri daha güvenilir düşük pil ve diğer tür senaryoların durumunda yapılan.
* 3 taraf kitaplığı tarafından oluşturulan ek konsol günlükleri kaldırıldı.

## <a name="310-08262015"></a>3.1.0 (08/26/2015)
* İOS 9 uyumluluk hata sahip bir üçüncü taraf kitaplık düzeltin. Sonuçlar, uygulama bilgilerini veya ek veriler gönderme yoklar sırada kilitlenme neden olmaktadır.

## <a name="300-06192015"></a>3.0.0 (06/19/2015)
* Mobile Engagement sessiz anında iletme bildirimleri kullanır.
* İOS için destek bırakılan 4.X. Uygulamanızın dağıtım hedef bu sürümünden başlayarak olmalıdır en az iOS 6.

## <a name="220-05212015"></a>2.2.0 (05/21/2015)
* Cihazlar için Mobile Engagement cihaz kimliğini < iOS 6 yükleme sırasında oluşturulan GUID şimdi dayanır.

## <a name="210-04242015"></a>2.1.0 (04/24/2015)
* SWIFT uyumluluk eklendi.
* Uygulama açtıktan sonra bir bildirim tıklandığında, eylem URL'si artık sağ yürütüldü.
* Eklenen eksik üstbilgi dosyasında SDK paketi.
* Mobile Engagement kilitlenme Raporlayıcı devre dışı bırakıldığında bir sorun düzeltilmiştir.

## <a name="200-02172015"></a>2.0.0 (02/17/2015)
* Azure Mobile Engagement ilk sürümünü
* AppID/sdkKey yapılandırma bağlantı dizesini yapılandırma tarafından değiştirilir.
* Rastgele XMPP varlıklardan rasgele XMPP ileti alıp göndermek için API kaldırıldı.
* Cihazlar arasında ileti gönderme ve alma için API kaldırıldı.
* Güvenlik geliştirmeleri.
* SmartAd izleme kaldırıldı.
