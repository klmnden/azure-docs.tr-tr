---
title: Azure Media Services ölçümleri ve tanılama günlüklerini Azure İzleyici ile izleme | Microsoft Docs
description: Bu makalede, Azure Media Services ölçümleri ve tanılama günlüklerini Azure İzleyici ile izleme hakkında genel bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 6d26cd809d78bf05f66c9fa03be5063ca4d2d5e4
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806005"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs"></a>Media Services ölçümleri ve tanılama günlüklerini izleyin

[Azure İzleyici](../../azure-monitor/overview.md) ölçümlerini izleme ve yardımcı olacak tanılama günlüklerini anlama, uygulamalarınızın performansını sağlar. Azure İzleyici tarafından toplanan tüm verileri ölçüm ve günlükleri olmak üzere iki temel türünden birini uyar. Media Services tanılama günlüklerini izleyin ve uyarılar ve bildirimler için toplanan bir ölçüm ve günlükleri oluşturun. Görselleştirme ve ölçüm verileri kullanarak Analiz [ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md). Günlükleri size gönderebilir [Azure depolama](https://azure.microsoft.com/services/storage/), kendisine akış [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)ve bunları dışarı aktarma [Log Analytics](https://azure.microsoft.com/services/log-analytics/), veya 3. taraf hizmetleri kullanın.

Ayrıntılı bir genel bakış için bkz. [Azure İzleyici ölçümleri](../../azure-monitor/platform/data-platform.md) ve [Azure İzleyici tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md).

Bu konuda ele alınmıştır desteklenen [Media Services ölçüm](#media-services-metrics) ve [Media Services tanılama günlükleri](#media-services-diagnostic-logs).

## <a name="media-services-metrics"></a>Medya Hizmetleri ölçümleri

Ölçümler, değeri değiştirir olup olmadığını düzenli aralıklarla toplanır. Bunlar sık örneklenebilir ve bir uyarı ile göreceli olarak basit bir mantıksal hızla harekete çünkü uyarmak için yararlıdır. Ölçüm uyarıları oluşturma hakkında daha fazla bilgi için bkz: [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak ölçüm Uyarıları yönetme](../../azure-monitor/platform/alerts-metric.md).

Media Services için aşağıdaki kaynakları izleme ölçümleri destekler:

* Hesap
* Akış uç noktası
 
### <a name="account"></a>Hesap

Aşağıdaki hesap ölçümleri izleyebilirsiniz. 

|Ölçüm adı|Display name|Açıklama|
|---|---|---|
|AssetCount|Varlık sayısı|Varlıklar, hesabınızdaki.|
|AssetQuota|Varlık kota|Varlık kotası hesabınızdaki.|
|AssetQuotaUsedPercentage|Varlık kullanılan kota yüzdesi|Zaten kullanılan varlık kotası yüzdesi.|
|ContentKeyPolicyCount|İçerik anahtarı ilke sayısı|Hesabınızdaki içerik anahtar ilkeleri.|
|ContentKeyPolicyQuota|İçerik anahtarı ilkesi kota|İçerik anahtarı ilkeleri kotası hesabınızdaki.|
|ContentKeyPolicyQuotaUsedPercentage|İçerik anahtarı ilkesi kota kullanılan yüzdesi|Zaten kullanılan içerik anahtarı ilke kotası yüzdesi.|
|StreamingPolicyCount|Akış ilke sayısı|Akış ilkeleri, hesabınızdaki.|
|StreamingPolicyQuota|Akış İlkesi kota|Hesabınızdaki ilkeleri kota akış.|
|StreamingPolicyQuotaUsedPercentage|Akış İlkesi kota kullanılan yüzdesi|Zaten kullanılan akış ilke kotası yüzdesi.|
 
Da gözden geçirmelisiniz [hesabı kotaları ve sınırlamaları](limits-quotas-constraints.md).

### <a name="streaming-endpoint"></a>Akış uç noktası

Aşağıdaki medya Hizmetleri [akış uç noktalarını](https://docs.microsoft.com/rest/api/media/streamingendpoints) ölçümleri desteklenir:

|Ölçüm adı|Display name|Açıklama|
|---|---|---|
|İstekler|İstekler|Akış uç noktası tarafından sunulan HTTP isteklerinin toplam sayısını sağlar.|
|Çıkış|Çıkış|Toplam çıkış bayt sayısı. Örneğin, akış uç noktası tarafından akışa bayt.|
|Başarı E2e|Başarı uçtan uca gecikme süresi|Akış uç noktası olduğunda isteği alındığında gelen süre yanıtın son baytını gönderildi.|

### <a name="why-would-i-want-to-use-metrics"></a>Neden ölçümleri kullanmak istiyorsunuz? 

Media Services ölçümleri izleme uygulamalarınızı nasıl performans gösterdiğini anlamak nasıl yardımcı olabileceğini örnekleri aşağıda verilmiştir. Media Services Ölçümleriyle çözülebilir bazı sorular şunlardır:

* Standart Akış sınırları aşıldığında bilmek Noktam nasıl izleyebilirim?
* Premium akış uç noktası ölçek birimleri yeterli olup olmadığını nasıl anlarım? 
* My akış uç noktaları ayarlama ölçeklendirmek ne zaman bilmek bir uyarı nasıl ayarlayabilirim?
* Bir uyarı hesabında yapılandırılmış en fazla çıkış ulaşıldığında bilmek nasıl ayarlayabilirim?
* Başarısız istekleri çözümlemesini nasıl görebilirim ve hataya neden oluyor?
* Nasıl Paketleyici kaç HLS veya tire istek çekilen görebilirim?
* Başarısız istek sayısı eşik değerini ulaşılmadan önce bilmeniz gereken uyarı nasıl ayarlayabilirim? 

### <a name="example"></a>Örnek

Bkz: [Media Services ölçümlerini izleme](media-services-metrics-howto.md)

## <a name="media-services-diagnostic-logs"></a>Media Services tanılama günlükleri

Tanılama günlükleri, bir Azure kaynağının işlemiyle ilgili zengin, sık sık veri sağlar. Daha fazla bilgi için [toplamak ve Azure kaynaklarınızdan günlük verilerini kullanmak nasıl](../../azure-monitor/platform/diagnostic-logs-overview.md).

Media Services tanılama günlükleri destekler:

* Anahtar teslim

### <a name="key-delivery"></a>Anahtar teslim

|Ad|Açıklama|
|---|---|
|Anahtar teslim hizmet isteği|Anahtar dağıtımı hizmetiyle Göster günlükleri bilgi isteyin. Daha fazla ayrıntı için [şemaları](media-services-diagnostic-logs-schema.md).|

### <a name="why-would-i-want-to-use-diagnostics-logs"></a>Neden tanılama günlükleri kullanmak istiyorsunuz? 

Anahtar teslim tanılama günlükleri ile geçirebileceğiniz bazıları şunlardır:

* DRM türü tarafından sunulan lisans sayısını bakın
* İlke tarafından sunulan lisans sayısını bakın 
* DRM veya ilke türüne göre görüyorsunuz
* İstemcilerden gelen istekleri yetkisiz lisans sayısını görebilirsiniz

### <a name="example"></a>Örnek

Bkz: [tanılama günlükleri medya hizmeti izleme](media-services-diagnostic-logs-howto.md)

## <a name="next-steps"></a>Sonraki adımlar 

* [Toplama ve Azure kaynaklarınızdan günlük verilerini kullanma](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Oluşturun, görüntüleyin ve ölçüm uyarıları Azure İzleyicisi'ni kullanarak yönetme](../../azure-monitor/platform/alerts-metric.md)
* [Media Services ölçümlerini izleme](media-services-metrics-howto.md)
* [Tanılama günlükleri medya hizmeti izleme](media-services-diagnostic-logs-howto.md)
