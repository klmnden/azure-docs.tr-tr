---
title: Azure İzleyicisi'ni (Önizleme) Azure Service Bus ölçümlerini | Microsoft Docs
description: Azure Monitoring Service Bus varlıklarının izlemek için kullanın
services: service-bus-messaging
documentationcenter: .NET
author: christianwolf42
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/05/2018
ms.author: sethm
ms.openlocfilehash: 3660f0a6794a2fd784ec8846177da7effe7fe681
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-service-bus-metrics-in-azure-monitor-preview"></a>Azure Service Bus ölçümlerini Azure İzleyicisi'ni (Önizleme)

Hizmet veri yolu ölçümleri, Azure aboneliğinizin kaynaklarında durumunu sağlar. Ölçüm verilerini zengin ile Service Bus kaynaklarınızı değil yalnızca ad alanı düzeyinde, aynı zamanda varlık düzeyinde genel durumunu değerlendirebilirsiniz. Bu istatistikler, hizmet veri yolu durumunu izlemenize yardımcı olur gibi önemli olabilir. Ölçümleri, ayrıca Azure desteğine başvurun gerek kalmadan kök neden sorunları gidermenize yardımcı olabilir.

Azure İzleyicisi, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş kullanıcı arabirimleri sağlar. Daha fazla bilgi için bkz: [Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve [.NET ile Azure İzleyici almak ölçümleri](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) github'da örnek.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure monitör, erişim ölçümleri için birden çok yollar sağlar. Ya da erişim ölçümleri aracılığıyla yapabilecekleriniz [Azure portal](https://portal.azure.com), veya Azure İzleyici API'leri (REST ve .NET) ve analiz çözümleri günlük analizi ve Event Hubs gibi kullanın. Daha fazla bilgi için bkz: [Azure İzleyici ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

Ölçümleri varsayılan olarak etkindir ve en son 30 gün veri erişebilir. Uzun bir süre için verileri korumak gerekiyorsa, ölçüm verilerini bir Azure depolama hesabı arşivleyebilirsiniz. Bu yapılandırılan [tanılama ayarlarını](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) Azure İzleyicisi'nde.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümleri

Zaman içinde ölçümleri izleyebilirsiniz [Azure portal](https://portal.azure.com). Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için ad alanınızı seçin ve ardından **ölçümleri (Peview)**. Varlık kapsamına filtre ölçümleri görüntülemek için varlığı seçin ve ardından **ölçümleri (Önizleme)**.

![][2]

Boyutlar destekleyen ölçümleri için istenen boyut değeriyle filtre gerekir.

## <a name="billing"></a>Faturalandırma

Azure İzleyicisi'nde ölçümleri kullanarak Önizleme'de çalışırken ücretsizdir. Ölçüm verilerini alma ek çözümler kullanırsanız, ancak, bu çözümleri tarafından fatura. Örneğin, bir Azure depolama hesabı ölçüm verilerini arşivlerseniz Azure Storage göre faturalandırılır. Gelişmiş analiz için günlük analizi için ölçüm verilerini akış sahipse günlük analizi tarafından ayrıca faturalandırılır.

Aşağıdaki ölçümleri, hizmeti genel bir bakış sağlar. 

> [!NOTE]
> Farklı bir adla geçildiği biz çeşitli ölçümleri onaysız kılınmadan. Bu, başvuruları güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğü ile işaretlenmiş ölçümleri ileride desteklenecektir değil.

Tüm ölçüm değerleri dakikada Azure İzleyicisi için gönderilir. Ölçüm değerleri sunulduğu zaman aralığını saat ayrıntı düzeyi tanımlar. Tüm Service Bus ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Verileri ve yönetim işlemleri isteklerinin sayısını sayar.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| Gelen istekleri (Önizleme) | Belirtilen süre içinde Service Bus hizmetine yapılan isteklerin sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Başarılı istekler (Önizleme)|Belirtilen süre içinde Service Bus hizmetine iletilen başarılı istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Sunucu hataları (Önizleme)|Service Bus hizmetinde bir hata nedeniyle belirtilen bir süredeki işlenmedi istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Kullanıcı hataları (Önizleme)|Kullanıcı hataları nedeniyle, belirtilen bir süredeki işlenmedi istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Daraltılmış istekleri (Önizleme)|Kullanım aşıldığından kısıtlanan istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="message-metrics"></a>İleti ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Gelen iletileri (Önizleme)|Olay veya hizmet veri yolu için belirtilen süre içinde gönderilen ileti sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Giden iletiler (Önizleme)|Olay veya hizmet yolundan belirtilen süre içinde alınan iletilerin sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="connection-metrics"></a>Bağlantı ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|ActiveConnections (Önizleme)|Bir varlık yanı sıra bir ad alanı etkin bağlantı sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Bağlantılar açık (Önizleme)|Açık bağlantıları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Bağlantı kapalı (Önizleme)|Kapalı bağlantılarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName |

## <a name="resource-usage-metrics"></a>Kaynak kullanım ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Ad alanı (Önizleme) başına CPU kullanımı|Yüzde CPU kullanımı ad.<br/><br/> Birim: yüzde <br/> Toplama türü: en fazla <br/> Boyut: EntityName|
|Ad alanı (Önizleme) başına bellek boyutu kullanımı|Ad alanı bellek kullanım yüzdesi.<br/><br/> Birim: yüzde <br/> Toplama türü: en fazla <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure Service Bus ölçümleri Azure İzleyicisi'nde aşağıdaki boyutlar destekler. Boyutlar, ölçümlerinizi eklemek isteğe bağlıdır. Boyutlar eklemezseniz ölçümleri ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|EntityName| Hizmet veri yolu ad alanı altındaki Mesajlaşma varlıkları destekler.|

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


