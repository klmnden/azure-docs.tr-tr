---
title: Azure İzleyici (Önizleme) Azure Service Bus ölçümleri | Microsoft Docs
description: Service Bus varlıkları izlemek için Azure izleme kullanın
services: service-bus-messaging
documentationcenter: .NET
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 11/06/2018
ms.author: aschhab
ms.openlocfilehash: 175d5d5d4495986c29b75427a325088c14279e17
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58849669"
---
# <a name="azure-service-bus-metrics-in-azure-monitor-preview"></a>Azure İzleyici (Önizleme) Azure Service Bus ölçümleri

Service Bus ölçümler durumu, Azure aboneliğinizdeki kaynaklar sunar. Zengin bir ölçüm verileri kümesiyle, yalnızca ad alanı düzeyinde, aynı zamanda varlık düzeyinde, hizmet veri yolu kaynaklarını genel durumunu değerlendirebilirsiniz. Bu istatistikler, hizmet veri yolu durumunu izlemek için yardımcı önemli olabilir. Ölçümler, Azure desteğine başvurun gerek kalmadan kök neden sorunlarını da yardımcı olabilir.

Azure İzleyici, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş bir kullanıcı arabirimi sağlar. Daha fazla bilgi için [Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve [almak Azure İzleyici ölçümleri .NET ile](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) GitHub üzerinde örnek.

> [!IMPORTANT]
> Değil olduğunda herhangi bir varlık etkileşim 2 saat için varlık artık boşta olana kadar bir değer olarak "0" gösteren ölçümleri başlar.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure İzleyici ölçümlerine erişim birden çok yol sağlar. Ya da erişim ölçümleri ile yapabilecekleriniz [Azure portalında](https://portal.azure.com), veya Azure İzleyici API'leri (REST ve .NET) ve Azure İzleyici günlüklerine ve Event Hubs gibi analiz çözümleri kullanın. Daha fazla bilgi için [Azure İzleyicisi'nde ölçümler](../azure-monitor/platform/data-platform-metrics.md).

Ölçümler, varsayılan olarak etkindir ve en son 30 Günün verilerini erişebilir. Uzun bir süre saklamak istiyorsanız ölçüm verileri bir Azure depolama hesabına arşivleyebilir. Bu değeri yapılandırılan [tanılama ayarları](../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings) Azure İzleyici'de.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümlerini

Zaman içinde ölçümleri izleyebilirsiniz [Azure portalında](https://portal.azure.com). Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için ad alanını seçin ve ardından **ölçümler (Önizleme)**. Varlık kapsamına filtrelenmiş ölçümleri görüntülemek için bir varlık seçin ve ardından **ölçümler (Önizleme)**.

![][2]

Boyutlar destekleyen ölçümler için istenen boyut değeri ile filtrelemesi gerekir.

## <a name="billing"></a>Faturalandırma

Ölçümleri kullanarak Azure İzleyici'de, Önizleme boyunca ücretsizdir. Ölçüm verilerini alma, ek çözümleri kullanırsanız, ancak bu çözümler tarafından faturalandırılırsınız. Örneğin, ölçüm verileri bir Azure depolama hesabına arşivleme, Azure Depolama tarafından faturalandırılır. Ölçüm verilerini Gelişmiş analiz için Azure İzleyici günlüklerine akış sahipse Azure İzleyici günlüklerine göre ayrıca faturalandırılır.

Aşağıdaki ölçümler size sistem hizmetinizin genel bakış sunar. 

> [!NOTE]
> Farklı bir adla geçildiği size çeşitli ölçümleri kullanımdan. Bu, başvurularını güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretli ölçümleri, bundan sonra desteklenmeyecek.

Azure İzleyici, tüm ölçüm değerleri dakikada gönderilir. Zaman ayrıntı düzeyi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Tüm Service Bus ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Veri ve yönetim işlemleri istek sayısını sayar.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| Gelen istekler (Önizleme) | Belirtilen bir süredeki Service Bus hizmetine yapılan isteklerin sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Başarılı istekler (Önizleme)|Belirtilen bir süredeki Service Bus hizmetine gönderilen başarılı isteklerin sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Sunucu hataları (Önizleme)|Service Bus hizmetinde bir hata nedeniyle belirtilen bir süredeki işlenmedi istek sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Kullanıcı hataları (Önizleme - aşağıdaki alt bölüme bakın)|Kullanıcı hataları nedeniyle, belirtilen bir süredeki işlenmedi istek sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Daraltılmış istekler (Önizleme)|Kullanım aşıldığından bulunduğu için kısıtlanan istek sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

### <a name="user-errors"></a>Kullanıcı hataları

Aşağıdaki iki türde hatalar, kullanıcı hataları sınıflandırılan:

1. İstemci tarafı hataları (içinde 400 hataları olan HTTP).
2. Gibi iletilerini işleme sırasında oluşan hataları [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception).


## <a name="message-metrics"></a>İleti ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Gelen iletiler (Önizleme)|Olayları veya belirtilen bir süredeki Service Bus'a gönderilen iletilerin sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Giden iletiler (Önizleme)|Olayları veya belirtilen bir süredeki Service Bus'tan alınan iletilerin sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
| İletiler (Önizleme) | Bir sıradaki/konudaki iletilerin sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Boyut: EntityName |
| ActiveMessages (Önizleme) | Bir sıradaki/konudaki etkin iletilerin sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Boyut: EntityName |

## <a name="connection-metrics"></a>Bağlantı ölçümü

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|ActiveConnections (Önizleme)|Bir varlığın yanı sıra bir ad alanı etkin bağlantı sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Bağlantılar açık (Önizleme)|Açık bağlantıları sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Bağlantı kapalı (Önizleme)|Kapalı bağlantılarının sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName |

## <a name="resource-usage-metrics"></a>Kaynak kullanım ölçümleri

> [!NOTE] 
> Yalnızca aşağıdaki ölçümler kullanılabilir **premium** katmanı. 

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Ad alanı (Önizleme) başına CPU kullanımı|Yüzdesi CPU kullanımı ad alanı.<br/><br/> Birim: Yüzde <br/> Toplama türü: Maksimum <br/> Boyut: EntityName|
|Ad alanı (Önizleme) başına bellek boyutu kullanımı|Ad alanı bellek kullanım yüzdesi.<br/><br/> Birim: Yüzde <br/> Toplama türü: Maksimum <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure Service Bus, Azure İzleyicisi'nde ölçümler için aşağıdaki boyutlarını destekler. Boyutları için ölçümlerinizi eklemek isteğe bağlıdır. Ölçümleri, boyutları eklemezseniz, ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|EntityName| Service Bus ad alanı altında Mesajlaşma varlıkları destekler.|

## <a name="set-up-alerts-on-metrics"></a>Ölçümler ile ilgili uyarılar ayarlayın

1. Üzerinde **ölçümleri** sekmesinde **Service Bus Namespace** sayfasında **uyarılarını yapılandırma**. 

    ![Ölçümleri sayfasında - uyarılar menüsünde yapılandırma](./media/service-bus-metrics-azure-monitor/metrics-page-configure-alerts-menu.png)
2. Seçin **hedefi seçme**, ve aşağıdaki işlemleri yapmak **bir kaynak seçin** sayfası: 
    1. Seçin **Service Bus ad alanları** için **kaynak türüne göre filtre** alan. 
    2. Aboneliğinizi seçin **aboneliğe göre filtrele** alan.
    3. Seçin **service bus ad alanı** listeden. 
    4. **Done** (Bitti) öğesini seçin. 
    
        ![Ad alanı seçin](./media/service-bus-metrics-azure-monitor/select-namespace.png)
1. Seçin **Ölçüt Ekle**, ve aşağıdaki işlemleri yapmak **sinyal mantığını yapılandırma** sayfası:
    1. Seçin **ölçümleri** için **sinyal türü**. 
    2. Bir sinyal seçin. Örneğin: **Hizmet hataları (Önizleme)**. 

        ![Sunucu hataları seçin](./media/service-bus-metrics-azure-monitor/select-server-errors.png)
    1. Seçin **büyüktür** için **koşul**.
    2. Seçin **toplam** için **zaman toplama**. 
    3. Girin **5** için **eşiği**. 
    4. **Done** (Bitti) öğesini seçin.    

        ![Koşulu belirtin](./media/service-bus-metrics-azure-monitor/specify-condition.png)    
1. Üzerinde **oluşturma kuralı** sayfasında **uyarı ayrıntılarını tanımlama**, ve aşağıdaki eylemleri gerçekleştirin:
    1. Girin bir **adı** uyarı. 
    2. Girin bir **açıklama** uyarı.
    3. Seçin **önem derecesi** uyarı. 

        ![Uyarı ayrıntıları](./media/service-bus-metrics-azure-monitor/alert-details.png)
1. Üzerinde **oluşturma kuralı** sayfasında **tanımla eylem grubu**seçin **yeni eylem grubu**, ve aşağıdaki işlemleri yapmak **Ekle eylem grubu sayfasını**. 
    1. Eylem grubu için bir ad girin.
    2. Eylem grubu için kısa bir ad girin. 
    3. Aboneliğinizi seçin. 
    4. Kaynak grubu seçin. 
    5. Bu kılavuz için girin **e-posta Gönder** için **eylem adı**.
    6. Seçin **e-posta/SMS/anında iletme/ses** için **eylem türü**. 
    7. Seçin **Ayrıntıları Düzenle**. 
    8. Üzerinde **e-posta/SMS/anında iletme/ses** sayfasında, aşağıdaki eylemleri gerçekleştirin:
        1. Seçin **e-posta**. 
        2. Tür **e-posta adresi**. 
        3. **Tamam**’ı seçin.

            ![Uyarı ayrıntıları](./media/service-bus-metrics-azure-monitor/add-action-group.png)
        4. Üzerinde **eylem grubu Ekle** sayfasında **Tamam**. 
1. Üzerinde **oluşturma kuralı** sayfasında **uyarı kuralı oluştur**. 

    ![Uyarı kuralı düğme oluşturma](./media/service-bus-metrics-azure-monitor/create-alert-rule.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


