---
title: "Azure Media Services akış uç genel bakış | Microsoft Docs"
description: "Bu konu, akış uç noktaları Azure Media Services genel bakış sağlar."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: 097ab5e5-24e1-4e8e-b112-be74172c2701
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: e454778c558b9c17c47ad9eb651737aa0b5e2605
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="streaming-endpoints-overview"></a>Akış uç noktaları genel bakış 

##<a name="overview"></a>Genel Bakış

Microsoft Azure Media Services (AMS) içinde bir **akış uç noktası** istemci oynatıcı uygulaması için doğrudan veya bir içerik teslim ağı (CDN) için daha fazla dağıtım için içerik ileten bir akış hizmetini temsil eder. Media Services, ayrıca Azure CDN entegrasyon sağlar. Giden akış StreamingEndpoint hizmetinden bir canlı akış, isteğe bağlı veya aşamalı indirme Media Services hesabınızda, varlık, bir video olabilir. Her Azure Media Services hesabı varsayılan StreamingEndpoint içerir. Ek akış hesabı altında oluşturulabilir. Akış, 1.0 ve 2. 0'ın iki sürümü vardır. 10 Ocak 2017 ile başlayarak, yeni oluşturulan tüm AMS hesapları sürüm 2.0 içerecektir **varsayılan** StreamingEndpoint. Bu hesaba eklediğiniz ek akış uç noktalarını de sürüm 2.0. Bu değişiklik, var olan hesapları etkilemez; Varolan akış sürüm 1.0 olacaktır ve 2.0 sürümüne yükseltilebilir. Bu değişiklikle olacaktır davranışı, faturalama ve özellik değişiklikleri (daha fazla bilgi için bkz: **türleri ve sürümleri akış** bölümünde belgelenen aşağıda).

Ayrıca, Azure Media Services 2,15 sürümünden (Ocak 2017 ' serbest) başlayarak aşağıdaki özellikleri akış uç noktası varlığa eklenen: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime**, **StreamingEndpointVersion**. Bu özellikleri ayrıntılı bakış için bkz: [bu](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Standart akış uç noktası için size oluşturulduğunda varsayılan Azure Media Services hesabı oluşturduğunuzda **durduruldu** durumu. Varsayılan akış uç silemezsiniz. Hedeflenen bölgede Azure CDN kullanılabilirliğine bağlı olarak, yeni oluşturulan varsayılan varsayılan akış uç noktası da "StandardVerizon" CDN içerir sağlayıcısı tümleştirme. 

>[!NOTE]
>Azure CDN tümleştirme istediğiniz akış uç noktası başlamadan önce devre dışı bırakılabilir.

Bu konuda akış uç noktaları tarafından sağlanan ana işlevlerine genel bir bakış sağlar.

## <a name="streaming-types-and-versions"></a>Akış türleri ve sürümleri

### <a name="standardpremium-types-version-20"></a>Standart/Premium türleri (sürüm 2.0)

Media Services Ocak 2017 sürümünden itibaren akış iki tür vardır: **standart** ve **Premium**. Bu tür akış uç noktası sürüm "2.0" bir parçasıdır.

Tür|Açıklama
---|---
**Standart**|Senaryoları çoğunluğu için işe yaramayacaktır varsayılan seçenek budur.<br/>Bu seçenek, sabit sınırlı SLA almak için ilk 15, başlattıktan sonra gün akış uç ücretsizdir.<br/>Birinci ilk 15 gün boyunca ücretsizdir yalnızca birden fazla akış uç noktaları, oluşturursanız, bunları başlar başlamaz, diğerleri faturalandırılır. <br/>Ücretsiz deneme sürümü yalnızca yeni oluşturulan medya Hizmetleri hesapları ve varsayılan akış uç noktası için geçerli olduğunu unutmayın. Varolan akış uç noktalarını ve ayrıca oluşturulan akış uç noktalarını ücretsiz deneme süresi içerir değil sürüm 2.0 bile yükseltti veya sürüm 2.0 oluşturulur.
**Premium**|Bu seçenek daha yüksek ölçek veya denetim gerektiren profesyonel senaryolar için uygundur.<br/>Satın alınan premium akış birimi (SU) kapasite temel alarak değişken SLA, ayrılmış akış uç noktalarını yalıtılmış bir ortamda dinamik ve kaynaklar için rekabet değil.

Daha ayrıntılı bilgi için bkz: **karşılaştırmak akış türleri** bölümden.

### <a name="classic-type-version-10"></a>Klasik türü (sürüm 1.0)

AMS hesapları 10 Ocak 2017 sürüm önce oluşturulan kullanıcılar için elinizde bir **Klasik** akış uç noktası türü. Bu tür akış uç noktası sürüm "1.0" bir parçasıdır.

Varsa, **sürüm "1.0"** akış uç noktası olan > akış birimleri (SU), 1 premium = premium akış uç noktası olur ve tüm AMS özellikleri sağlar (olduğu gibi **standart/Premium** türü) herhangi bir ek yapılandırma adımı.

>[!NOTE]
>**Klasik** akış uç noktaları (sürüm "1.0" ve 0 SU), sınırlı özellikleri sağlar ve bir SLA içermez. Geçirmek için önerilen **standart** türü daha iyi bir deneyim almak ve dinamik paketleme veya şifreleme gibi özellikleri ve birlikte gelen diğer özellikleri kullanmak için **standart** türü. Geçirmek için **standart** türü, Git [Azure portal](https://portal.azure.com/) seçip **katılımı için standart**. Geçiş hakkında daha fazla bilgi için bkz: [geçiş](#migration-between-types) bölümü.
>
>Bu işlem geri alınamaz ve fiyatlandırma bir etkisi kaybolacağını unutmayın.
>
 
## <a name="comparing-streaming-types"></a>Akış türleri karşılaştırma

### <a name="versions"></a>Sürümler

|Tür|StreamingEndpointVersion|ScaleUnits|CDN|Faturalandırma|SLA| 
|--------------|----------|-----------------|-----------------|-----------------|-----------------|    
|Klasik|1.0|0|NA|Ücretsiz|NA|
|Standart Akış Uç Noktası|2.0|0|Evet|Ücretli|Evet|
|Premium Akış Birimleri|1.0|>0|Evet|Ücretli|Evet|
|Premium Akış Birimleri|2.0|>0|Evet|Ücretli|Evet|

### <a name="features"></a>Özellikler

Özellik|Standart|Premium
---|---|---
Ücretsiz ilk 15 gün| Evet |Hayır
Aktarım hızı |Azure CDN kullanılmadığında en fazla 600 MB/sn. CDN ölçekler.|Birim (SU) akış başına 200 MB/sn. CDN ölçekler.
SLA | 99.9|99,9 (200 MB/sn SU başına).
CDN|Azure CDN, üçüncü taraf CDN ya da hiçbir CDN.|Azure CDN, üçüncü taraf CDN ya da hiçbir CDN.
Faturalama eşit olarak bölünür| Günlük|Günlük
Dinamik şifreleme|Evet|Evet
Dinamik paketleme|Evet|Evet
Ölçek|Otomatik ölçeklendirme kadar hedeflenen işleme.|Ek akış birimleri
IP filtre/G20/özel konak|Evet|Evet
Aşamalı indirme|Evet|Evet
Önerilen kullanımı |Büyük bir çoğunluğu senaryoları akış önerilir.|Profesyonel kullanımı.<br/>Düşünüyorsanız standart ötesinde gereksinimlerine sahip olabilir. Eş zamanlı İzleyici boyutu 50.000 görüntüleyiciler büyük bekliyorsanız, bize (amsstreaming microsoft.com adresindeki) başvurun.


## <a name="migration-between-types"></a>Türleri arasında geçiş

Kaynak | Bitiş | Eylem
---|---|---
Klasik|Standart|Katılımı gerekir
Klasik|Premium| Ölçek (ek akış birimleri)
Standart/Premium|Klasik|Kullanılabilir değil (akış uç noktası sürüm 1.0 ise. "0" scaleunits ayar Klasik değiştirmesine izin verilir)
Standart (ile/CDN olmadan)|Aynı yapılandırmaya sahip Premium|İzin verilen **başlatılan** durumu. (Azure portalı üzerinden)
Premium (ile/CDN olmadan)|Aynı yapılandırmaya sahip standart|İzin verilen **başlatılan** durum (aracılığıyla Azure portalı)
Standart (ile/CDN olmadan)|Premium ile farklı yapılandırma|İzin verilen **durduruldu** durum (aracılığıyla Azure portalı). Çalışır durumda izin verilmiyor.
Premium (ile/CDN olmadan)|Standart farklı yapılandırma|İzin verilen **durduruldu** durum (aracılığıyla Azure portalı). Çalışır durumda izin verilmiyor.
1.0 sürümünü SU > = 1 CDN ile|Standart/Premium hiçbir CDN ile|İzin verilen **durduruldu** durumu. İçinde izin verilmiyor **başlatılan** durumu.
1.0 sürümünü SU > = 1 CDN ile|Standart ile/CDN olmadan|İzin verilen **durduruldu** durumu. İçinde izin verilmiyor **başlatılan** durumu. Sürüm 1.0 oluşturulur ve başlatılan silinir ve yeni bir CDN olacaktır.
1.0 sürümünü SU > = 1 CDN ile|Premium ile/CDN olmadan|İzin verilen **durduruldu** durumu. İçinde izin verilmiyor **başlatılan** durumu. Klasik CDN oluşturulur ve başlatılan silinir ve yeni bir olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

