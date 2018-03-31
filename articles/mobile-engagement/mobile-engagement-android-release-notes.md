---
title: Azure Mobile Engagement Android SDK tümleştirmesi
description: En son güncelleştirmeler ve yordamlar için Azure Mobile Engagement Android SDK
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: a01bd74649fa28e0d86cea4ed03da80326c06234
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="release-notes"></a>Sürüm notları
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 


## <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Çağrılırken nadiren bir kilitlenme düzeltme `EngagementAgentUtils.isInDedicatedEngagementProcess`, ayrıca kullanılan tarafından `EngagementApplication` sınıfı.

## <a name="430-06272017"></a>4.3.0 (06/27/2017)
* Android 8 desteği (SDK'ın önceki sürümleri üzerinde Android 8 çalışmaz).
* Daha fazla hiçbir bağımlılık destek kitaplığı.
* Kaldırma `EngagementFragmentActivity` sınıfı.
* Nedeniyle [arka plan yürütme sınırları](https://developer.android.com/preview/features/background.html) arka planda günlükleri Android 8'de kullanıcı aygıt ile etkileşim kadar gecikebilir, bu bir itme kampanya etkileyecek **teslim edildi** ve **sistem bildirimi görüntülenir** Aygıt uyku durumunda erteleniyor istatistikleri (bildirim görüntülenmeye devam eder, halka ve gerçek zamanlı sorun olmadan Titret).
* Nedeniyle [arka plan konum sınırları](https://developer.android.com/preview/features/background-location-limits.html), arka plan konumda güncelleştirilmeyecek sık üzerinde Android 8 gerçek zamanlı.

## <a name="424-03302017"></a>4.2.4 (03/30/2017)
* Uygulama içi bildirim metin renkleri Android 7 eski Android sürümleri ile aynı olacak şekilde düzeltin.

## <a name="423-08102016"></a>4.2.3 (08/10/2016)
* Daha fazla WIFI kilit yok.
* Init (hata 4.2.0 içinde sunulan) önce getDeviceId çağrılırken bir kilitlenme düzeltin.

## <a name="422-05172016"></a>4.2.2 (05/17/2016)
* İstikrara yönelik iyileştirmeler.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)
* Güvenlik: web görünümü yerel dosya erişimini devre dışı bırakın.
* Güvenliği: kaldırma `EngagementPreferenceActivity` artık kullanılmayan ve güvenli olmayan genişleten sınıf `PreferenceActivity` sınıfı.
* Güvenlik: etkinlikleri kullanmak için şimdi belgelenmiştir ulaşma `exported="false"`, bu bayrak, önceki SDK sürümlerinde kullanılabilir.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)
* SDK şimdi MIT altında lisanslanmıştır.
* Bir özel cihaz tanımlayıcısı SDK başlatma zamanında belirtmeye izin.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)
* İstikrara yönelik iyileştirmeler.

## <a name="414-01262016"></a>4.1.4 (01/26/2016)
* İstikrara yönelik iyileştirmeler.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)
* İstikrara yönelik iyileştirmeler.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)
* İstikrara yönelik iyileştirmeler.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)
* İstikrara yönelik iyileştirmeler.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)
* Yeni izni modeli için Android M işleyin.
* Yerine çalışma zamanında konumu özellikleri artık yapılandırabilirsiniz `AndroidManifest.xml`.
* Bir izin hatayı düzeltmek: kullanırsanız `ACCESS_FINE_LOCATION`, ardından `ACCESS_COARSE_LOCATION` artık gerekli değildir.
* İstikrara yönelik iyileştirmeler.

## <a name="400-07062015"></a>4.0.0 (07/06/2015)
* Analizler ve anında iletme daha güvenilir sağlamak için iç protokol değişiklikleri.
* İtme kampanya herhangi bir türde yerel gönderim kimlik bilgilerini yapılandırmak için yerel gönderim (GCM/ADM) şimdi de için uygulama içi Bildirimlerde kullanılır.
* Büyük resim bildirim düzeltin: gönderilen sonra görüntülenen yalnızca 10'luk oldukları.
* Web görünümünde bir hatayı düzeltmek: bir bağlantıyı tıklatmak olduğu da yürütülürken varsayılan eylem URL'si.
* Yerel depolama yönetimiyle ilgili nadir bir kilitlenme düzeltin.
* Dinamik yapılandırma dizesi yönetim düzeltin.
* EULA güncelleştirin.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)
* Azure Mobile Engagement ilk sürümünü
* AppID yapılandırma bağlantı dizesini yapılandırma tarafından değiştirilir.
* Rastgele XMPP varlıklardan rasgele XMPP ileti alıp göndermek için API kaldırıldı.
* Cihazlar arasında ileti gönderme ve alma için API kaldırıldı.
* Güvenlik geliştirmeleri.
* Google Play ve SmartAd izleme kaldırıldı.

