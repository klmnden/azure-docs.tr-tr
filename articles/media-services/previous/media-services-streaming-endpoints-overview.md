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
ms.openlocfilehash: a45e2af6f2cb9c105c084585a03a6de615fa1397
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64573048"
---
# <a name="streaming-endpoints-overview"></a>Akış uç noktalarına genel bakış  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

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

Media Services'ın Ocak 2017 sürümünden başlayarak, iki akış tür vardır: **Standart** (Önizleme) ve **Premium**. Bu türler, akış uç noktası sürüm "2. 0" bir parçasıdır.


|Tür|Açıklama|
|--------|--------|  
|**Standart**|Varsayılan akış uç noktası olan bir **standart** yazın, akış birimlerini ayarlayarak Premium türüne değiştirilebilir.|
|**Premium** |Bu seçenek, daha yüksek ölçek veya denetim gerektiren profesyonel senaryolar için uygundur. Geçmeden bir **Premium** akış birimlerini ayarlayarak türü.<br/>Adanmış akış uç noktalarını yalıtılmış bir ortamda dinamik ve kaynaklar için rekabet değil.|

İnternet geniş kitlelere içerik sağlamak isteyen müşteriler için akış uç noktasında CDN'yi etkinleştirmenizi öneririz.

Daha ayrıntılı bilgi için bkz. [karşılaştırma akış türleri](#comparing-streaming-types) aşağıdaki bölümde.

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

|Tür|StreamingEndpointVersion|ScaleUnits|CDN|Faturalandırma|
|--------------|----------|-----------------|-----------------|-----------------|
|Klasik|1.0|0|NA|Boş|
|Standart akış uç noktası (Önizleme)|2.0|0|Evet|Ücretli|
|Premium Akış Birimleri|1.0|>0|Evet|Ücretli|
|Premium Akış Birimleri|2.0|>0|Evet|Ücretli|

### <a name="features"></a>Özellikler

Özellik|Standart|Premium
---|---|---
İlk 15 gün ücretsiz <sup>1</sup>| Evet |Hayır
Aktarım hızı |600 MB/sn için ve CDN kullanıldığında bir çok daha yüksek maliyetli performans sağlayabilir.|Akış birimi (SU) başına 200 MB/sn. CDN kullanıldığında bir çok daha yüksek maliyetli performans sağlayabilir.
CDN|Azure CDN, üçüncü taraf CDN veya hiçbir CDN.|Azure CDN, üçüncü taraf CDN veya hiçbir CDN.
Faturalama saatlere eşit olarak dağıtılır| Günlük|Günlük
Dinamik şifreleme|Evet|Evet
Dinamik paketleme|Evet|Evet
Ölçek|Otomatik yönelik hedeflenen aktarım hızını ölçeklendirir.|Ek akış birimleri.
IP filtrelemeyi/G20/özel konak <sup>2</sup>|Evet|Evet
Aşamalı indirme|Evet|Evet
Önerilen kullanım |Büyük bir çoğunluğu senaryoları akış önerilir.|Profesyonel kullanımı. 

<sup>1</sup> ücretsiz deneme sürümünü yalnızca yeni oluşturulan media services hesapları ve varsayılan akış uç noktası için geçerlidir.<br/>
<sup>2</sup> CDN uç noktasında etkinleştirilmediğinde yalnızca doğrudan akış uç kullanılır.<br/>

SLA bilgileri için bkz. [fiyatlandırma ve SLA](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="migration-between-types"></a>Türler arasında geçiş

Başlangıç | Bitiş | Eylem
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

