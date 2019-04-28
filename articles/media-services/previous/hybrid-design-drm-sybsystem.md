---
title: Azure Media Services'i kullanarak DRM subsystem(s) sisteminin karma tasarımı | Microsoft Docs
description: Bu konuda, Azure Media Services'i kullanarak DRM subsystem(s) sisteminin karma tasarımı anlatılmaktadır.
services: media-services
documentationcenter: ''
author: willzhan
manager: femila
editor: ''
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: willzhan;juliako
ms.openlocfilehash: 5c86a49cd9dc26f724de12ed2e5e77e645e4ab53
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466604"
---
# <a name="hybrid-design-of-drm-subsystems"></a>DRM alt sisteminin karma tasarımı 

Bu konuda, Azure Media Services'i kullanarak DRM subsystem(s) sisteminin karma tasarımı anlatılmaktadır.

## <a name="overview"></a>Genel Bakış

Azure Media Services aşağıdaki üç DRM sistemi için destek sağlar:

* PlayReady
* Widevine (modüler)
* FairPlay

DRM desteği DRM şifreleme (dinamik şifreleme) ve lisans dağıtımı, Azure Media Player, tüm 3 benzeri DRM destekleyen bir tarayıcı oynatıcı SDK'sı içerir.

DRM/CENC alt tasarım ve uygulama ayrıntılı bağlantısında için lütfen başlıklı belge bakın [çoklu DRM ve Access Control ile CENC](media-services-cenc-with-multidrm-access-control.md).

Üç DRM sistemi için tam destek sunuyoruz, bazen müşteriler DRM alt sisteminin karma oluşturmak için kendi altyapısı/alt sistemlerin Azure Media Services ek olarak çeşitli bölümlerini kullanmanız gerekir.

Aşağıdaki sık sorulan bazı müşteriler tarafından istenir:

* "Verilerimi kendi DRM lisans sunucusu kullanabilir miyim?" (Bu durumda, müşterilerin DRM lisans sunucusu grubunda katıştırılmış iş mantığına sahip yatırım yapmış).
* "Yalnızca kendi DRM lisans teslimat Azure Media Services'da AMS içeriğini barındırma olmadan kullanabilir miyim?"

## <a name="modularity-of-the-ams-drm-platform"></a>AMS DRM platform tanılanmasını

Kapsamlı bir bulut video platformu bir parçası olarak, Azure Media Services DRM göz önünde bir tasarım esneklik ve modülerlik sahiptir. Azure Media Services herhangi biriyle (aşağıdaki tabloda kullanılan gösterim açıklaması) aşağıdaki tabloda açıklanan aşağıdaki farklı birleşimlerini kullanabilirsiniz. 

|**İçerik barındırma & Başlangıcı**|**İçerik şifreleme**|**DRM lisansı teslimi**|
|---|---|---|
|AMS|AMS|AMS|
|AMS|AMS|Üçüncü taraf|
|AMS|Üçüncü taraf|AMS|
|AMS|Üçüncü taraf|Üçüncü taraf|
|Üçüncü taraf|Üçüncü taraf|AMS|

### <a name="content-hosting--origin"></a>İçerik barındırma & Başlangıcı

* AMS: video varlık AMS barındırılır ve akış AMS akış uç noktaları (ancak mutlaka dinamik paketleme) kullanılır.
* Üçüncü taraf: video barındırılan ve AMS dışında bir üçüncü taraf akış platformunda sunulur.

### <a name="content-encryption"></a>İçerik şifreleme

* AMS: gerçekleştirilen dinamik olarak/isteğe bağlı AMS dinamik şifreleme tarafından içerik şifrelemesidir.
* Üçüncü taraf: önceden işleme iş akışını kullanarak AMS dışında içerik şifreleme gerçekleştirilir.

### <a name="drm-license-delivery"></a>DRM lisansı verme

* AMS: DRM lisansı AMS lisans teslimat hizmeti tarafından sağlanır.
* Üçüncü taraf: DRM lisansı, bir üçüncü taraf DRM lisans sunucusu AMS dışında teslim edilir.

## <a name="configure-based-on-your-hybrid-scenario"></a>Yapılandırma, karma senaryo temel alınarak

### <a name="content-key"></a>İçerik anahtarı

Bir içerik anahtarı konfigürasyonuyla AMS dinamik şifreleme hem de AMS lisans teslimat hizmetinin aşağıdaki öznitelikleri denetleyebilirsiniz:

* Dinamik DRM şifreleme için kullanılan içerik anahtarı.
* DRM lisans içerik lisans teslim hizmetleri tarafından teslim edilecek: hakları, içerik anahtarı ve kısıtlamaları.
* Tür **içerik anahtarı yetkilendirme ilkesi kısıtlama**: açık, IP veya belirteç kısıtlaması.
* Varsa **belirteci** tür **içerik anahtarı yetkilendirme ilkesi kısıtlama kullanılan**, **içerik anahtarı yetkilendirme ilkesi kısıtlaması** lisans verilmeden önce karşılanması gereken.

### <a name="asset-delivery-policy"></a>Varlık teslim İlkesi

Varlık teslim ilkesi yapılandırma yoluyla, AMS dinamik Paketleyici ve AMS akış uç noktası, dinamik şifreleme tarafından kullanılan aşağıdaki öznitelikleri denetleyebilirsiniz:

* Protokol ve DRM şifreleme birleşimi, DASH CENC (PlayReady ve Widevine) altında gibi altında PlayReady, Widevine veya PlayReady altında HLS akış kesintisiz akış.
* Varsayılan/katıştırılmış teslim URL'leri her ilgili benzeri DRM lisans.
* DASH MPD edinme URL'lerinde (LA_URLs) olup olmadığını lisans veya HLS çalma listesi içeren sorgu dizesi anahtarı kimliği (çocuk), Widevine ve FairPlay, sırasıyla.

## <a name="scenarios-and-samples"></a>Senaryolar ve örnekler

Önceki bölümde açıklamaları bağlı olarak, aşağıdaki beş karma senaryoları ilgili kullanın **içerik anahtarı**-**varlık teslim ilkesini** yapılandırması bileşimleri (örnekleri Son sütun izleyin tabloda bahsedilen):

|**İçerik barındırma & Başlangıcı**|**DRM şifreleme**|**DRM lisansı teslimi**|**İçerik anahtarını yapılandırma**|**Varlık teslim ilkesini yapılandırma**|**Örnek**|
|---|---|---|---|---|---|
|AMS|AMS|AMS|Evet|Evet|Örnek 1|
|AMS|AMS|Üçüncü taraf|Evet|Evet|Örnek 2|
|AMS|Üçüncü taraf|AMS|Evet|Hayır|Örnek 3|
|AMS|Üçüncü taraf|Dış|Hayır|Hayır|Örnek 4|
|Üçüncü taraf|Üçüncü taraf|AMS|Evet|Hayır|    

Örnekler, hem DASH hem de kesintisiz akış için PlayReady korumalı çalışır. Aşağıdaki video URL'leri, kesintisiz akış URL'lerini. Karşılık gelen DASH URL'leri almak için yalnızca URL'ye "(biçim mpd-time-CSF)". Kullanabileceğinizi [azure media player test](https://aka.ms/amtest) tarayıcıda test etmek için. Hangi teknik altında kullanılacak hangi Akış Protokolü yapılandırmanıza olanak sağlar. Ie11 ve Windows 10 Microsoft Edge EME üzerinden PlayReady destekler. Daha fazla bilgi için [test aracı hakkındaki ayrıntıları](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).

### <a name="sample-1"></a>Örnek 1

* Kaynak (Temel) URL'si: https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest 
* PlayReady LA_URL (DASH ve kesintisiz): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 
* Widevine LA_URL (DASH): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4 
* FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8 

### <a name="sample-2"></a>Örnek 2

* Kaynak (Temel) URL'si: https://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest 
* PlayReady LA_URL (DASH ve kesintisiz): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx 

### <a name="sample-3"></a>Örnek 3

* Kaynak URL'si: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH ve kesintisiz): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 

### <a name="sample-4"></a>Örnek 4

* Kaynak URL'si: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH ve kesintisiz): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx 

## <a name="summary"></a>Özet

Özet olarak, Azure Media Services DRM bileşenlerini esnektir, bunları bir karma senaryoda düzgün bir şekilde içerik anahtarı ve varlık teslim ilkesini yapılandırarak bu konuda açıklandığı gibi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Görünüm Media Services'i öğrenme yolları.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

