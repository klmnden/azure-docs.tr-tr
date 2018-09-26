---
title: Azure İzleyici (Önizleme) Azure Service Bus ölçümleri | Microsoft Docs
description: Service Bus varlıkları izlemek için Azure izleme kullanın
services: service-bus-messaging
documentationcenter: .NET
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/24/2018
ms.author: spelluru
ms.openlocfilehash: 293cde00e53171e848263df8564ec85f273c1a40
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166342"
---
# <a name="azure-service-bus-metrics-in-azure-monitor-preview"></a>Azure İzleyici (Önizleme) Azure Service Bus ölçümleri

Service Bus ölçümler durumu, Azure aboneliğinizdeki kaynaklar sunar. Zengin bir ölçüm verileri kümesiyle, yalnızca ad alanı düzeyinde, aynı zamanda varlık düzeyinde, hizmet veri yolu kaynaklarını genel durumunu değerlendirebilirsiniz. Bu istatistikler, hizmet veri yolu durumunu izlemek için yardımcı önemli olabilir. Ölçümler, Azure desteğine başvurun gerek kalmadan kök neden sorunlarını da yardımcı olabilir.

Azure İzleyici, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş bir kullanıcı arabirimi sağlar. Daha fazla bilgi için [Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve [almak Azure İzleyici ölçümleri .NET ile](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) GitHub üzerinde örnek.

> [!IMPORTANT]
> Değil olduğunda herhangi bir varlık etkileşim 2 saat için varlık artık boşta olana kadar bir değer olarak "0" gösteren ölçümleri başlar.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure İzleyici ölçümlerine erişim birden çok yol sağlar. Ya da erişim ölçümleri ile yapabilecekleriniz [Azure portalında](https://portal.azure.com), veya Azure İzleyici API'leri (REST ve .NET) ve Log Analytics ve Event Hubs gibi analiz çözümleri kullanın. Daha fazla bilgi için [İzleme verilerine Azure İzleyicisi tarafından toplanan](../monitoring/monitoring-data-collection.md).

Ölçümler, varsayılan olarak etkindir ve en son 30 Günün verilerini erişebilir. Uzun bir süre saklamak istiyorsanız ölçüm verileri bir Azure depolama hesabına arşivleyebilir. Bu yapılandırılan [tanılama ayarları](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) Azure İzleyici'de.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümlerini

Zaman içinde ölçümleri izleyebilirsiniz [Azure portalında](https://portal.azure.com). Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için ad alanını seçin ve ardından **ölçümler (Önizleme)**. Varlık kapsamına filtrelenmiş ölçümleri görüntülemek için bir varlık seçin ve ardından **ölçümler (Önizleme)**.

![][2]

Boyutlar destekleyen ölçümler için istenen boyut değeri ile filtrelemesi gerekir.

## <a name="billing"></a>Faturalandırma

Ölçümleri kullanarak Azure İzleyici'de, Önizleme boyunca ücretsizdir. Ölçüm verilerini alma, ek çözümleri kullanırsanız, ancak bu çözümler tarafından faturalandırılırsınız. Örneğin, ölçüm verileri bir Azure depolama hesabına arşivleme, Azure Depolama tarafından faturalandırılır. Gelişmiş analiz için ölçüm verileri Log analytics'e akış sahipse Log Analytics tarafından ayrıca faturalandırılır.

Aşağıdaki ölçümler size sistem hizmetinizin genel bakış sunar. 

> [!NOTE]
> Farklı bir adla geçildiği size çeşitli ölçümleri kullanımdan. Bu, başvurularını güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretli ölçümleri, bundan sonra desteklenmeyecek.

Azure İzleyici, tüm ölçüm değerleri dakikada gönderilir. Zaman ayrıntı düzeyi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Tüm Service Bus ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Veri ve yönetim işlemleri istek sayısını sayar.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| Gelen istekler (Önizleme) | Belirtilen bir süredeki Service Bus hizmetine yapılan isteklerin sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Başarılı istekler (Önizleme)|Belirtilen bir süredeki Service Bus hizmetine gönderilen başarılı isteklerin sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Sunucu hataları (Önizleme)|Service Bus hizmetinde bir hata nedeniyle belirtilen bir süredeki işlenmedi istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Kullanıcı hataları (Önizleme - aşağıdaki alt bölüme bakın)|Kullanıcı hataları nedeniyle, belirtilen bir süredeki işlenmedi istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Daraltılmış istekler (Önizleme)|Kullanım aşıldığından bulunduğu için kısıtlanan istek sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

### <a name="user-errors"></a>Kullanıcı hataları

Aşağıdaki iki türde hatalar, kullanıcı hataları sınıflandırılan:

1. İstemci tarafı hataları (içinde 400 hataları olan HTTP).
2. Gibi iletilerini işleme sırasında oluşan hataları [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception).


## <a name="message-metrics"></a>İleti ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Gelen iletiler (Önizleme)|Olayları veya belirtilen bir süredeki Service Bus'a gönderilen iletilerin sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Giden iletiler (Önizleme)|Olayları veya belirtilen bir süredeki Service Bus'tan alınan iletilerin sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="connection-metrics"></a>Bağlantı ölçümü

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|ActiveConnections (Önizleme)|Bir varlığın yanı sıra bir ad alanı etkin bağlantı sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Bağlantılar açık (Önizleme)|Açık bağlantıları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Bağlantı kapalı (Önizleme)|Kapalı bağlantılarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName |

## <a name="resource-usage-metrics"></a>Kaynak kullanım ölçümleri

> [!NOTE] 
> Yalnızca aşağıdaki ölçümler kullanılabilir **premium** katmanı. 

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Ad alanı (Önizleme) başına CPU kullanımı|Yüzdesi CPU kullanımı ad alanı.<br/><br/> Birim: yüzde <br/> Toplama türü: en fazla <br/> Boyut: EntityName|
|Ad alanı (Önizleme) başına bellek boyutu kullanımı|Ad alanı bellek kullanım yüzdesi.<br/><br/> Birim: yüzde <br/> Toplama türü: en fazla <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure Service Bus, Azure İzleyicisi'nde ölçümler için aşağıdaki boyutlarını destekler. Boyutları için ölçümlerinizi eklemek isteğe bağlıdır. Ölçümleri, boyutları eklemezseniz, ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|EntityName| Service Bus ad alanı altında Mesajlaşma varlıkları destekler.|

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


