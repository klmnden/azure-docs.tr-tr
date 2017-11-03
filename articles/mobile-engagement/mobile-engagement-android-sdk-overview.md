---
title: "Azure Mobile Engagement için Android SDK tümleştirmesi"
description: "Android uygulamalarında Azure Mobile Engagement SDK'sını tümleştirmek açıklar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a91ed04f-f3ce-4692-a6dd-b56a28d7dee8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo;ricksal
ms.openlocfilehash: 35935e911f1f17989beb71978396c6d1b7d601d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Azure Mobile Engagement için Android SDK tümleştirmesi
> [!div class="op_single_selector"]
> * [Evrensel Windows](mobile-engagement-windows-store-sdk-overview.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
> * [iOS](mobile-engagement-ios-sdk-overview.md)
> * [Android](mobile-engagement-android-sdk-overview.md)
> 
> 

Bu belgede, tümleştirme ve yapılandırma için tüm seçenekleri kullanılabilir Azure Mobile Engagement Android SDK açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Gelişmiş Özellikler
### <a name="reporting-features"></a>Raporlama özellikleri
Bu özellikler ekleyebilirsiniz:

1. [Gelişmiş raporlama seçenekleri](mobile-engagement-android-advanced-reporting.md)
2. [Konum raporlama seçenekleri](mobile-engagement-android-location-reporting.md)
3. [Gelişmiş yapılandırma seçenekleri](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Bildirimler:
[Android uygulamanızda ulaşma (bildirimler) tümleştirme](mobile-engagement-android-integrate-engagement-reach.md)

1. Google bulut (GCM) Mesajlaşma: [GCM mobil katılım ile tümleştirme](mobile-engagement-android-gcm-integrate.md)
2. Amazon cihaz Mesajlaşma hizmeti (ADM): [ADM mobil katılım ile tümleştirme](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Etiket planı uygulama:
[Android uygulamanızda API etiketleme Gelişmiş Mobile Engagement kullanma](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Sürüm notları

### <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Çağrılırken nadiren bir kilitlenme düzeltme `EngagementAgentUtils.isInDedicatedEngagementProcess`, ayrıca kullanılan tarafından `EngagementApplication` sınıfı.

### <a name="430-06272017"></a>4.3.0 (06/27/2017)
* Android 8 desteği (SDK'ın önceki sürümleri üzerinde Android 8 çalışmaz).
* Daha fazla hiçbir bağımlılık destek kitaplığı.
* Kaldırma `EngagementFragmentActivity` sınıfı.
* Nedeniyle [arka plan yürütme sınırları](https://developer.android.com/preview/features/background.html) arka planda günlükleri Android 8'de kullanıcı aygıt ile etkileşim kadar gecikebilir, bu bir itme kampanya etkileyecek **teslim edildi** ve **sistem bildirimi görüntülenir** Aygıt uyku durumunda erteleniyor istatistikleri (bildirim görüntülenmeye devam eder, halka ve gerçek zamanlı sorun olmadan Titret).
* Nedeniyle [arka plan konum sınırları](https://developer.android.com/preview/features/background-location-limits.html), arka plan konumda güncelleştirilmeyecek sık üzerinde Android 8 gerçek zamanlı.

Tüm sürümler için bkz: [tamamlamak sürüm notları](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Yükseltme yordamları
Uygulamanıza bizim SDK daha eski bir sürümü zaten bütünleştirdiyseniz başvurun [yükseltme yordamları](mobile-engagement-android-upgrade-procedure.md).

