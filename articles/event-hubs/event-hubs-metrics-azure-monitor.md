---
title: Azure Event Hubs ölçümlerini Azure İzleyicisi'ni (Önizleme) | Microsoft Docs
description: Olay hub'ları izlemek için Azure Monitoring kullanın
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: sethm
ms.openlocfilehash: 8ca00b234c00bfeb52a5b601e8780d56a0732dd9
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="azure-event-hubs-metrics-in-azure-monitor-preview"></a>Azure Event Hubs ölçümlerini Azure İzleyicisi'ni (Önizleme)

Olay hub'ları ölçümleri olay hub'ları, Azure aboneliğinizin kaynaklarında durumunu sağlar. Zengin bir ölçüm verileri ile olay hub'larınız değil yalnızca ad alanı düzeyinde, aynı zamanda varlık düzeyinde genel durumunu değerlendirebilirsiniz. Bu istatistikler, olay hub'larınız durumunu izlemenize yardımcı olur gibi önemli olabilir. Ölçümleri, ayrıca Azure desteğine başvurun gerek kalmadan kök neden sorunları gidermenize yardımcı olabilir.

Azure İzleyicisi, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş kullanıcı arabirimleri sağlar. Daha fazla bilgi için bkz: [Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve [.NET ile Azure İzleyici almak ölçümleri](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) github'da örnek.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure monitör, erişim ölçümleri için birden çok yollar sağlar. Ya da erişim ölçümleri aracılığıyla yapabilecekleriniz [Azure portal](https://portal.azure.com), veya Azure İzleyici API'leri (REST ve .NET) ve analiz çözümleri operasyon Management Suite ve Event Hubs gibi kullanın. Daha fazla bilgi için bkz: [Azure İzleyici ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

Ölçümleri varsayılan olarak etkindir ve en son 30 gün veri erişebilir. Uzun bir süre için verileri korumak gerekiyorsa, ölçüm verilerini bir Azure depolama hesabı arşivleyebilirsiniz. Bu yapılandırılan [tanılama ayarlarını](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) Azure İzleyicisi'nde.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümleri

Zaman içinde ölçümleri izleyebilirsiniz [Azure portal](https://portal.azure.com). Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için ad alanınızı seçin ve ardından **ölçümleri (Peview)**. Olay hub'ı kapsamına filtre ölçümleri görüntülemek için olay hub'ı seçin ve ardından **ölçümleri (Önizleme)**.

Boyutları destekleme ölçümler için aşağıdaki örnekte gösterildiği gibi istenen boyut değerine sahip filtre gerekir:

![][2]

## <a name="billing"></a>Faturalandırma

Azure İzleyicisi'nde ölçümleri kullanarak Önizleme'de şu anda çalışırken ücretsizdir. Ölçüm verilerini alma ek çözümler kullanırsanız, ancak, bu çözümleri tarafından fatura. Örneğin, bir Azure depolama hesabı ölçüm verilerini arşivlerseniz Azure Storage göre faturalandırılır. Gelişmiş analiz için günlük analizi için ölçüm verilerini akış sahipse Azure tarafından ayrıca faturalandırılır.

Aşağıdaki ölçümleri, hizmeti genel bir bakış sağlar. 

> [!NOTE]
> Farklı bir adla geçildiği biz çeşitli ölçümleri onaysız kılınmadan. Bu, başvuruları güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğü ile işaretlenmiş ölçümleri ileride desteklenecektir değil.

Tüm ölçüm değerleri dakikada Azure İzleyicisi için gönderilir. Ölçüm değerleri sunulduğu zaman aralığını saat ayrıntı düzeyi tanımlar. Tüm olay hub'ları ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Verileri ve yönetim işlemleri isteklerinin sayısını sayar.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| Gelen istekleri (Önizleme) | Belirtilen süre içinde Azure Event Hubs hizmetine yapılan isteklerin sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName |
| Başarılı istekler (Önizleme)   | Belirtilen süre içinde Azure Event Hubs hizmetine iletilen başarılı istek sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName |
| Sunucu hataları (Önizleme) | Azure Event Hubs hizmetinde bir hata nedeniyle belirtilen bir süredeki işlenmedi istek sayısı. <br/><br/>Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName |
|Kullanıcı hataları (Önizleme)|Kullanıcı hataları nedeniyle, belirtilen bir süredeki işlenmedi istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Daraltılmış istekleri (Önizleme)|Üretilen iş birimi kullanımına aşıldığından kısıtlanan istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Kota aşıldı hataları (Önizleme)|İstek sayısı kullanılabilir kota aşıldı. Bkz: [bu makalede](event-hubs-quotas.md) olay hub'ları kotaları hakkında daha fazla bilgi.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="throughput-metrics"></a>Verimlilik metriklerini

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Daraltılmış istekleri (Önizleme)|Üretilen iş birimi kullanımına aşıldığından kısıtlanan istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="message-metrics"></a>İleti ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Gelen iletileri (Önizleme)|Olay veya olay hub'ları için belirtilen süre içinde gönderilen ileti sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Giden iletiler (Önizleme)|Olay veya ileti sayısı, belirtilen bir süredeki olay hub'larından aldı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Gelen bayt sayısı (Önizleme)|Belirtilen süre içinde Azure Event Hubs hizmetine gönderilen bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Giden bayt sayısı (Önizleme)|Bayt sayısı belirtilen süre içinde Azure Event Hubs hizmetinden alındı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="connection-metrics"></a>Bağlantı ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|ActiveConnections (Önizleme)|Bir varlık yanı sıra bir ad alanı etkin bağlantı sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Bağlantılar açık (Önizleme)|Açık bağlantıları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Bağlantı kapalı (Önizleme)|Kapalı bağlantılarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="event-hubs-capture-metrics"></a>Olay hub'ları yakalama ölçümleri

Olay hub'larınız için yakalama özelliği etkinleştirdiğinizde, olay hub'ları yakalama ölçümleri izleyebilirsiniz. Aşağıdaki ölçümleri etkin yakalama ile izleyebilirsiniz açıklanmaktadır.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Biriktirme listesi (Önizleme) yakalama|Seçilen hedef Yakalanacak henüz burada bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Yakalanan iletileri (Önizleme)|İletileri veya seçilen hedef için belirtilen bir süredeki yakalanır olayları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Yakalanan bayt (Önizleme)|Belirtilen süre içinde seçilen hedef yakalanan bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure Event Hubs ölçümleri Azure İzleyicisi'nde aşağıdaki boyutlar destekler. Boyutlar, ölçümlerinizi eklemek isteğe bağlıdır. Boyutlar eklemezseniz ölçümleri ad alanı düzeyinde belirtilir. 

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|EntityName| Event Hubs olay hub'ı varlıklar ad alanı altındaki destekler.|

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).
* [.NET ile Azure İzleyici ölçümleri alma](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) github'da örnek. 

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



