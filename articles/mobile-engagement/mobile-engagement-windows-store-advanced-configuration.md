---
title: Gelişmiş Windows Evrensel uygulamaları Engagement SDK'sı için yapılandırma
description: Gelişmiş yapılandırma seçenekleri için Azure Mobile Engagement Windows Evrensel uygulamaları ile
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: c0f666a0dd3441dfe3de0e82953b78c139091672
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Gelişmiş Windows Evrensel uygulamaları Engagement SDK'sı için yapılandırma
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

> [!div class="op_single_selector"]
> * [Evrensel Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
> 
> 

Bu yordam, Azure Mobile Engagement Android uygulamaları için çeşitli yapılandırma seçeneklerini yapılandırmak açıklar.

## <a name="prerequisites"></a>Önkoşullar
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma
### <a name="disable-automatic-crash-reporting"></a>Otomatik Kilitlenme bildirimini devre dışı bırak
Otomatik Kilitlenme raporlama katılım özelliği devre dışı bırakabilirsiniz. Ardından, işlenmeyen bir özel durum oluştuğunda katılım hiçbir şey yapmaz.

> [!WARNING]
> Bu özelliği devre dışı sonra uygulamanızda işlenmeyen bir kilitlenme ortaya çıktığında, katılım kilitlenme göndermez **ve** işleri ve oturum kapatma değil.
> 
> 

Otomatik Kilitlenme bildirimini devre dışı bırakmak için yapılandırmanıza bağlı olarak, bildirilen biçimini Özelleştir:

#### <a name="from-engagementconfigurationxml-file"></a>Gelen `EngagementConfiguration.xml` dosyası
Kümesine rapor kilitlenme `false` arasında `<reportCrash>` ve `</reportCrash>` etiketler.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Gelen `EngagementConfiguration` çalışma zamanında nesne
Rapor kilitlenme EngagementConfiguration nesnesini kullanarak false olarak ayarlayın.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Gerçek zamanlı raporlama devre dışı bırak
Varsayılan olarak, katılım hizmet raporları gerçek zamanlı olarak günlüğe kaydeder. Uygulamanızı günlükleri sık bildirirse, günlükleri arabellek ve bunları aynı anda normal saat bazında bildirmek için daha iyi olur. Bu, "veri bloğu modu" adı verilir.

Bunu yapmak için yöntemi çağırın:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Bağımsız değişkeni olarak bir değerdir **milisaniye**. Gerçek zamanlı günlük yeniden etkinleştirmek istediğiniz zaman herhangi bir parametre olmadan veya 0 değeri yöntemini çağırın.

Veri bloğu modu biraz pil ömrünün artırır ancak Engagement İzleyicisi üzerinde bir etkisi vardır: tüm oturumları ve işleri süre (dolayısıyla, oturumlar ve işleri veri bloğu eşik görünmeyebilir daha kısa) veri bloğu eşik yuvarlanır. Bir veri bloğu eşikten artık 30000 (30s) kullanmanızı öneririz. Kaydedilen günlükler 300 öğe ile sınırlıdır. Gönderme çok uzunsa, bazı günlükleri kaybedebilir.

> [!WARNING]
> Veri bloğu eşik nokta değerinden küçük bir saniye yapılandırılamaz. Bunu yaparsanız, SDK izleme şu hata ile gösterir ve otomatik olarak sıfır saniye varsayılan değere sıfırlanır. Bu günlükler gerçek zamanlı rapor için SDK tetikler.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
