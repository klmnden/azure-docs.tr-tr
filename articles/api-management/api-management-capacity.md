---
title: Bir Azure API Management örneğinin kapasite | Microsoft Docs
description: Bu makalede, kapasite ölçüm nedir ve bilinçli kararlar mi Azure API Management örneği ölçeklendirmek nasıl oluşturulacağı açıklanmaktadır.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/18/2018
ms.author: apimpm
ms.openlocfilehash: 4983854a14a6efe9214692dc677dedeada73933b
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296109"
---
# <a name="capacity-of-an-azure-api-management-instance"></a>Azure API Management örneği kapasitesi

**Kapasite** tek en önemli olan [Azure İzleyici ölçüm](api-management-howto-use-azure-monitor.md#view-metrics-of-your-apis) bilinçli kararlar mi daha fazla yük uyum sağlamak için API Management örneği ölçeklendirmek sağlama. Kendi yapım karmaşıktır ve belirli bir davranış uygular.

Bu makalede ne açıklanmaktadır **kapasite** olduğu ve nasıl davranacağını. Nasıl erişeceğinizi gösterir **kapasite** Azure portalında ölçümleri ve ne zaman ölçeklendirme veya API Management Örneğinize yükseltmeyi düşünün önerir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede adımları için olması gerekir:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için bkz: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="what-is-capacity"></a>Kapasitesi nedir

![Kapasite ölçüm](./media/api-management-capacity/capacity-ingredients.png)

**Kapasite** yük APIM örneğindeki bir göstergesidir. Kaynak Kullanımı (CPU, bellek) ve ağ kuyruk uzunluğu yansıtır. CPU ve bellek kullanımı kaynak tüketimi ortaya çıkarır:

+ İstekleri iletme veya bir ilke çalıştıran içerebilen yönetim eylemleri ya da istek işleme gibi APIM Hizmetleri
+ SSL el sıkışmaları yeni bağlantı maliyeti ile ilgili işlemler de dahil olmak üzere seçili işletim sistemi işlemleri.

Toplam **kapasite** her birim bir API Management örneğinin kendi değerleri ortalama bir değer.

## <a name="capacity-metric-behavior"></a>Kapasite ölçüm davranışı

Gerçek Hayatta, yapı nedeniyle **kapasite** tarafından birçok değişkenleri, örneğin etkilenebilir:

+ bağlantı desenleri (var olan bağlantıyı yeniden isteği vs yeni bağlantı)
+ İstek ve yanıt boyutu
+ Her API veya istemci istekleri gönderirken sayısı yapılandırılan ilkeleri.

Daha karmaşık işlemleri isteklerinde olan, daha yüksek **kapasite** tüketim olacaktır. Örneğin, karmaşık dönüştürme ilkelerini basit isteği iletme daha çok daha fazla CPU kullanır. Yavaş arka uç hizmeti yanıtları, çok artacaktır.

> [!IMPORTANT]
> **Kapasite** işlenmekte olan istek sayısının doğrudan bir ölçü değil.

![Kapasite ölçüm ani](./media/api-management-capacity/capacity-spikes.png)

**Kapasite** aralıklı çıkmasına ya da işlenmekte olan hiçbir istek olsa bile sıfırdan büyük olmalıdır. Sistem veya platforma özgü eylemleri nedeniyle olur ve bir örnek ölçeklendirmek karar verirken dikkate alınmamalıdır.
  
## <a name="use-the-azure-portal-to-examine-capacity"></a>Kapasite incelemek için Azure Portalı'nı kullanın
  
![Kapasite ölçüm](./media/api-management-capacity/capacity-metric.png)  

1. APIM örneğinizi gidin [Azure portal](https://portal.azure.com/).
2. Seçin **ölçümleri (Önizleme)**.
3. Mor bölümünden seçin **kapasite** kullanılabilir Ölçümler ve varsayılan olarak bırakın ölçüm **Avg** toplama.

    > [!TIP]
    > Her zaman gözden geçirmeniz gereken bir **kapasite** yanlış yorumlar önlemek için konum başına ölçüm dökümü.

4. Yeşil bölümünden seçin **konumu** boyuta göre ölçüm bölmek için.
5. İstenen bir zaman çerçevesi bölümünün üst çubuğundan seçin.

    Beklenmeyen bir şey olduğunda gerçekleştiği size bildirmek için bir ölçüm uyarı ayarlayabilirsiniz. Örneğin, APIM örneğinizi exceededing kaldığında bildirimleri beklenen en yüksek kapasitesi üzerinde 20 dakika alın.

    >[!TIP]
    > Uyarılar hizmetinizi kapasite azaldığında bilmenize Azure İzleyicisi otomatik ölçeklendirmeyi işlevlerini otomatik olarak bir Azure API Management birimi eklemek için izin verecek şekilde yapılandırabilirsiniz. Kurallarınızı uygun şekilde planlamanız gerekir böylece işlemi ölçeklendirme yaklaşık 30 dakika sürebilir.  
    > Yalnızca ana konum ölçekleme izin verilir.

## <a name="use-capacity-for-scaling-decisions"></a>Kapasite kararları ölçekleme için kullanma

**Kapasite** mi daha fazla yük uyum sağlamak için API Management örneği ölçeklendirmek kararları için ölçümüdür. Göz önünde bulundurun:

+ Bir uzun vadeli eğilim ve ortalama aranıyor.
+ Büyük olasılıkla ani ani yoksayılıyor değil ilgili herhangi bir artış yük (açıklama için "Kapasite ölçüm davranışı" bölümüne bakın).
+ Yükseltme veya, örnek, ölçeklendirme zaman **kapasite**ait değer %60 veya % 70 bir daha uzun süre (örneğin 30 dakika) aşıyor. Farklı değerleri, hizmet veya senaryo için daha iyi çalışabilir.

>[!TIP]  
> Trafiğinizi önceden tahmin tamamlayabilirseniz, APIM örneğinizi beklediğiniz iş yükleri üzerinde sınayın. İstek yükü, Kiracı'aşamalı olarak artırmak ve kapasite ölçüm hangi değerini, yoğun yük karşılık gelen izleyin. Kapasite ne kadar herhangi bir zamanda kullanılan anlamak için Azure portalı kullanmak için önceki bölümdeki adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Ölçeklendirme veya bir Azure API Management hizmeti örneği yükseltme](upgrade-and-scale.md)