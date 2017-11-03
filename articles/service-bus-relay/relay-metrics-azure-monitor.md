---
title: "Azure İzleyicisi'ni (Önizleme) Azure geçiş ölçümlerini | Microsoft Docs"
description: "Azure Monitoring Azure geçişi izlemek için kullanın"
services: service-bus-relay
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/20/2017
ms.author: sethm
ms.openlocfilehash: 3652e80c20c425570ba90a1f3ce7a3035762a34d
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="azure-relay-metrics-in-azure-monitor-preview"></a>Azure geçiş ölçümlerini Azure İzleyicisi'ni (Önizleme)

Azure geçiş ölçümleri, Azure aboneliğinizin kaynaklarında durumunu sağlar. Ölçüm verilerini zengin ile geçiş kaynaklarınızı değil yalnızca ad alanı düzeyinde, aynı zamanda varlık düzeyinde genel durumunu değerlendirebilirsiniz. Bu istatistikler Azure geçiş durumunu izlemek için yardımcı gibi önemli olabilir. Ölçümleri, ayrıca Azure desteğine başvurun gerek kalmadan kök neden sorunları gidermenize yardımcı olabilir.

Azure İzleyicisi, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş kullanıcı arabirimleri sağlar. Daha fazla bilgi için bkz: [Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve [.NET ile Azure İzleyici almak ölçümleri](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) github'da örnek.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure monitör, erişim ölçümleri için birden çok yollar sağlar. Ya da erişim ölçümleri aracılığıyla yapabilecekleriniz [Azure portal](https://portal.azure.com), veya Azure İzleyici API'leri (REST ve .NET) ve analiz çözümleri operasyon Management Suite ve Event Hubs gibi kullanın. Daha fazla bilgi için bkz: [Azure İzleyici ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

Ölçümleri varsayılan olarak etkindir ve en son 30 gün veri erişebilir. Uzun bir süre için verileri korumak gerekiyorsa, ölçüm verilerini bir Azure depolama hesabı arşivleyebilirsiniz. Bu yapılandırılan [tanılama ayarlarını](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) Azure İzleyicisi'nde.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümleri

Zaman içinde ölçümleri izleyebilirsiniz [Azure portal](https://portal.azure.com). Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için ad alanınızı seçin ve ardından **ölçümleri (Peview)**. 

Boyutlar destekleyen ölçümleri için istenen boyut değeriyle filtre gerekir.

## <a name="billing"></a>Faturalandırma

Azure İzleyicisi'nde ölçümleri kullanarak Önizleme'de şu anda çalışırken ücretsizdir. Ölçüm verilerini alma ek çözümler kullanırsanız, ancak, bu çözümleri tarafından fatura. Örneğin, bir Azure depolama hesabı ölçüm verilerini arşivlerseniz Azure Storage göre faturalandırılır. Gelişmiş analiz için OMS ölçümleri veri akış sahipse işlemi Yönetim Paketi (OMS) tarafından ayrıca faturalandırılır.

Aşağıdaki ölçümleri, hizmeti genel bir bakış sağlar. 

> [!NOTE]
> Farklı bir adla geçildiği biz çeşitli ölçümleri onaysız kılınmadan. Bu, başvuruları güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğü ile işaretlenmiş ölçümleri ileride desteklenecektir değil.

Tüm ölçüm değerleri dakikada Azure İzleyicisi için gönderilir. Ölçüm değerleri sunulduğu zaman aralığını saat ayrıntı düzeyi tanımlar. Tüm Azure geçiş ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="connection-metrics"></a>Bağlantı ölçümleri

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
| ListenerConnections başarı (Önizleme) | Belirtilen bir süre boyunca Azure geçişi yapılan başarılı dinleyici bağlantı sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ListenerConnections ClientError (Önizleme)|Belirtilen bir sürede dinleyicisi bağlantılarında istemci hataların sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ListenerConnections ServerError (Önizleme)|Belirtilen bir sürede dinleyicisi bağlantılarında sunucu hataların sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderConnections başarı (Önizleme)|Belirtilen süre içinde yapılan başarılı gönderen bağlantılarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderConnections ClientError (Önizleme)|Belirtilen bir sürede gönderen bağlantılarında istemci hataların sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderConnections ServerError (Önizleme)|Belirtilen bir sürede gönderen bağlantılarında sunucu hataların sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ListenerConnections TotalRequests (Önizleme)|Belirtilen bir sürede dinleyici bağlantı toplam sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderConnections TotalRequests (Önizleme)|Belirtilen bir süredeki Gönderenler tarafından yapılan bağlantı istekleri.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ActiveConnections (Önizleme)|Belirtilen bir süre boyunca etkin bağlantı sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ActiveListeners (Önizleme)|Belirtilen bir süre boyunca etkin dinleyicileri sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ListenerDisconnects (Önizleme)|Belirtilen bir sürede bağlantısı kesilmiş dinleyicileri sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderDisconnects (Önizleme)|Belirtilen bir sürede bağlantısı kesilmiş Gönderenler sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="memory-usage-metrics"></a>Bellek kullanım ölçümleri

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
|BytesTransferred (Önizleme)|Belirtilen bir süredeki aktarılan toplam bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure geçiş ölçümleri Azure İzleyicisi'nde aşağıdaki boyutlar destekler. Boyutlar, ölçümlerinizi eklemek isteğe bağlıdır. Boyutlar eklemezseniz ölçümleri ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|EntityName| Ad alanı altındaki Mesajlaşma varlıkları Azure geçişini destekler.|

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/relay-metrics-azure-monitor/relay-monitor1.png




