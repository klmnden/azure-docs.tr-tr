---
title: Azure Media Services akış uç noktası genel bakış | Microsoft Docs
description: Bu konu, akış uç noktaları Azure Media Services genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: c5979fa7ff67c5acda9ab653bc4ee52d8b5129a5
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58293813"
---
# <a name="streaming-endpoints-overview"></a>Akış uç noktalarına genel bakış  

## <a name="overview"></a>Genel Bakış

Microsoft Azure Media Services (AMS) içinde bir **akış uç noktası** içeriği doğrudan bir istemci Yürütücü uygulamasına veya daha fazla dağıtım bir içerik teslim ağı'için (CDN) teslim eden bir akış hizmetini temsil eder. Media Services, Azure CDN sorunsuz tümleştirme de sağlar. Giden akıştan StreamingEndpoint hizmetinin, canlı akış, bir video isteğe bağlı veya Media Services hesabı, varlığı aşamalı indirme olabilir. Her Azure Media Services hesabı bir varsayılan StreamingEndpoint içerir. Ek akış hesabı altında oluşturulabilir. Akış, 1.0 ve 2.0 iki sürümü vardır. 10 Ocak 2017'den itibaren yeni oluşturulan tüm AMS hesaplarını sürüm 2.0 içerecektir **varsayılan** StreamingEndpoint. Bu hesaba eklediğiniz ek akış uç noktaları, aynı zamanda sürüm 2.0 olacaktır. Bu değişiklik, mevcut hesapları etkilemez; Mevcut akış sürümü 1.0 ve 2.0 sürümüne yükseltilebilir. Bu değişiklikle birlikte olacaktır davranışı, faturalama ve özellik değişiklikleri (daha fazla bilgi için **akış türleri ve sürümleri** bölümde belgelenen aşağıda).

Azure Media Services akış uç noktası varlığa şu özellikleri ekledi: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime**, **StreamingEndpointVersion**. Bu özelliklerin ayrıntılı bakış için bkz: [bu](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Standart akış uç noktası oluşturuldu uygulamasında varsayılan bir Azure Media Services hesabı oluşturduğunuzda, **durduruldu** durumu. Varsayılan akış uç noktası silinemiyor. Hedeflenen bölgede Azure CDN kullanılabilirliğine bağlı olarak, varsayılan olarak yeni oluşturulan varsayılan akış uç noktası "StandardVerizon" CDN yöntemlerine sağlayıcısı tümleştirme. 
                
> [!NOTE]
> Akış uç noktasını başlamadan önce Azure CDN tümleştirmesi devre dışı bırakılabilir. `hostname` Ve CDN'yi etkinleştirme olup olmadığını akış URL'si aynı kalır.

Bu konu, akış uç noktaları tarafından sağlanan ana işlevlerini genel bir bakış sağlar.

## <a name="naming-conventions"></a>Adlandırma kuralları

Varsayılan uç nokta için: `{AccountName}.streaming.mediaservices.windows.net`

Tüm ek uç noktalar için: `{EndpointName}-{AccountName}.streaming.mediaservices.windows.net`

## <a name="streaming-types-and-versions"></a>Akış türleri ve sürümleri

### <a name="standardpremium-types-version-20"></a>Standart/Premium türleri (sürüm 2.0)

Media Services'ın Ocak 2017 sürümünden başlayarak, iki akış tür vardır: **Standart** ve **Premium**. Bu türler, akış uç noktası sürüm "2. 0" bir parçasıdır.

Type|Açıklama
---|---
**Standart** |Bu, senaryoların büyük bölümü için işe yarar varsayılan seçenektir.<br/>Bu seçenek, sabit sınırlı SLA'sını alın, akış uç noktasını ilk 15, başlattıktan sonra gün ücretsizdir.<br/>Yalnızca ilk ilk 15 gün boyunca ücretsiz olarak birden fazla akış uç noktaları, oluşturmak, bunları başlar başlamaz, diğerleri faturalandırılır. <br/>Ücretsiz deneme sürümü yalnızca yeni oluşturulan media services hesapları ve varsayılan akış uç noktası için geçerli olduğunu unutmayın. Mevcut akış uç noktaları ve ayrıca oluşturulan akış uç noktalarını ücretsiz deneme süresi içerir değil bile 2.0 sürümüne yükseltilir veya bunlar 2.0 sürümünde oluşturulur.
**Premium** |Bu seçenek, daha yüksek ölçek veya denetim gerektiren profesyonel senaryolar için uygundur.<br/>Satın aldığınız premium akış birimi (SU) kapasite tabanlı değişken SLA, adanmış bir akış uç noktalarını yalıtılmış bir ortamda dinamik ve kaynaklar için rekabet değil.

Daha ayrıntılı bilgi için bkz. **karşılaştırma akış türleri** aşağıdaki bölümde.

### <a name="classic-type-version-10"></a>Klasik tür (sürüm 1.0)

AMS hesapları 10 Ocak 2017 sürüm yayınlanmadan önce oluşturulan kullanıcılar için sahip olduğunuz bir **Klasik** bir akış uç noktası türü. Bu tür, akış uç noktası sürüm "1.0" bir parçasıdır.

Varsa, **sürüm "1.0"** akış uç noktası olan > = 1 premium akış birimleri (SU) premium akış uç noktası olacak ve tüm AMS özellikleri sağlayacaktır (olduğu gibi **standart/Premium** türü) herhangi bir ek yapılandırma adımları.

>[!NOTE]
>**Klasik** akış uç noktaları (sürüm "1.0" ve 0 SU), sınırlı özellikleri sağlar ve bir SLA içermez. Geçirmek için önerilen **standart** daha iyi bir deneyim elde edin ve dinamik paketleme veya şifreleme gibi özellikleri ve birlikte gelen diğer özellikleri kullanmak için tür **standart** türü. Geçirmek için **standart** türü, Git [Azure portalında](https://portal.azure.com/) seçip **katılımı standart**. Geçiş hakkında daha fazla bilgi için bkz: [geçiş](#migration-between-types) bölümü.
>
>Bu işlem geri alınamaz ve fiyatlandırmayı etkiler dikkatli olun.
>
 
## <a name="comparing-streaming-types"></a>Akış türlerini karşılaştırma

### <a name="versions"></a>Sürümler

|Type|StreamingEndpointVersion|ScaleUnits|CDN|Faturalandırma|SLA| 
|--------------|----------|-----------------|-----------------|-----------------|-----------------|    
|Klasik|1.0|0|NA|Ücretsiz|NA|
|Standart Akış Uç Noktası|2.0|0|Evet|Ücretli|Evet|
|Premium Akış Birimleri|1.0|>0|Evet|Ücretli|Evet|
|Premium Akış Birimleri|2.0|>0|Evet|Ücretli|Evet|

### <a name="features"></a>Özellikler

Özellik|Standart|Premium
---|---|---
Ücretsiz ilk 15 gün| Evet |Hayır
Aktarım hızı |Azure CDN olmadığında en fazla 600 MB/sn. CDN ile ölçeklendirilir.|Akış birimi (SU) başına 200 MB/sn. CDN ile ölçeklendirilir.
SLA | 99.9|% 99,9 (SU başına 200 Mbps).
CDN|Azure CDN, üçüncü taraf CDN veya hiçbir CDN.|Azure CDN, üçüncü taraf CDN veya hiçbir CDN.
Faturalama saatlere eşit olarak dağıtılır| Günlük|Günlük
Dinamik şifreleme|Evet|Evet
Dinamik paketleme|Evet|Evet
Ölçek|Otomatik yönelik hedeflenen aktarım hızını ölçeklendirir.|Ek akış birimleri
IP filtrelemeyi/G20/özel konak|Evet|Evet
Aşamalı indirme|Evet|Evet
Önerilen kullanım |Büyük bir çoğunluğu senaryoları akış önerilir.|Profesyonel kullanımı.<br/>Düşünüyorsanız standart ötesinde gereksinimlerine sahip olabilir. Bizimle iletişime geçin (amsstreaming@microsoft.com) görüntüleyiciler 50. 000'den daha büyük bir eş zamanlı hedef kitlesi boyutunu bekliyorsanız.


## <a name="migration-between-types"></a>Türler arasında geçiş

Başlangıç fiyatı | Alıcı | Eylem
---|---|---
Klasik|Standart|Kabul etme gerekir
Klasik|Premium| Ölçek (ek akış birimleri)
Standart/Premium|Klasik|Kullanılabilir değil (akış uç noktası sürüm 1.0 ise. "0" scaleunits ayarı ile klasik değiştirmek için kullanılabilir)
Standart (ile/CDN olmadan)|Aynı yapılandırmaya sahip Premium|İzin verilen **çalışmaya** durumu. (Azure portalı)
Premium (ile/CDN olmadan)|Aynı yapılandırmaya sahip standart|İzin verilen **çalışmaya** durum (aracılığıyla Azure portalı)
Standart (ile/CDN olmadan)|Farklı bir yapılandırma ile Premium|İzin verilen **durduruldu** durum (aracılığıyla Azure portalı). Çalışır durumda izin verilmiyor.
Premium (ile/CDN olmadan)|Farklı bir yapılandırma ile standart|İzin verilen **durduruldu** durum (aracılığıyla Azure portalı). Çalışır durumda izin verilmiyor.
Sürüm 1.0 ile SU > = 1 ile CDN|Standart/Premium hiçbir CDN ile|İzin verilen **durduruldu** durumu. İçinde izin verilmiyor **çalışmaya** durumu.
Sürüm 1.0 ile SU > = 1 ile CDN|Standart ile/CDN olmadan|İzin verilen **durduruldu** durumu. İçinde izin verilmiyor **çalışmaya** durumu. Sürüm 1.0, oluşturulması ve başlatılması silinir ve yeni bir CDN olacaktır.
Sürüm 1.0 ile SU > = 1 ile CDN|Premium ile/CDN olmadan|İzin verilen **durduruldu** durumu. İçinde izin verilmiyor **çalışmaya** durumu. Klasik CDN oluşturulması ve başlatılması silinir ve yeni bir olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

