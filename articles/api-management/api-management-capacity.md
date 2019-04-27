---
title: Bir Azure API Management örneğinin kapasite | Microsoft Docs
description: Bu makalede, Azure API Management örneği ölçeklendirme yapılıp bilinçli kararlar yapmak nasıl kapasite ölçüm nedir ve açıklanmaktadır.
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
ms.openlocfilehash: fe77361c4c9bed9310f8443ed4ff37faf7ea53a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60658329"
---
# <a name="capacity-of-an-azure-api-management-instance"></a>Azure API Management örneği kapasitesi

**Kapasite** tek en önemli olan [Azure İzleyici ölçüm](api-management-howto-use-azure-monitor.md#view-metrics-of-your-apis) daha fazla yük uyum sağlamak için API Management örneği ölçeklendirme yapılıp bilinçli bir karar almadan için. Kendi yapı karmaşıktır ve belirli bir davranış uygular.

Bu makalede ne açıklanır **kapasite** olduğu ve nasıl davranacağını. Nasıl erişeceğinizi gösterir **kapasite** Azure portalında ölçümleri ve ne zaman ölçeklendirme veya API Management örneğinizin yükseltme önerir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede adımları takip etmek için şunlara sahip olmalısınız:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="what-is-capacity"></a>Kapasite nedir

![Kapasite ölçümü](./media/api-management-capacity/capacity-ingredients.png)

**Kapasite** yük APIM örneği üzerinde bir göstergesidir. Bunu, kaynak kullanımı (CPU, bellek) ve ağ sıra uzunlukları yansıtır. CPU ve bellek kullanımı, kaynakların tüketimini gösterir:

+ İstekleri iletmek veya bir ilkesi çalıştırmaktan içerebilen yönetim eylemleri veya istek işleme gibi APIM Hizmetleri
+ yeni bağlantılar SSL el sıkışmaları maliyetini gerektiren işlemler de dahil olmak üzere seçili işletim sistemi işlemler.

Toplam **kapasite** her birim bir API Management örneğinin kendi değerlerini bir ortalamasıdır.

## <a name="capacity-metric-behavior"></a>Kapasite ölçüm davranışı

Gerçek Hayatta, yapı nedeniyle **kapasite** tarafından birçok değişkenleri, örneğin etkilenebilir:

+ bağlantı modelleri (var olan bağlantıyı yeniden bir istek vs yeni bağlantı)
+ İstek ve yanıt boyutu
+ Her bir API veya istemci istekleri gönderme sayısı yapılandırılmış ilkeler.

İstekler daha karmaşık işlemleri olduğundan, daha yüksek **kapasite** tüketim olacaktır. Örneğin, çok daha fazla CPU basit isteği iletme daha karmaşık dönüştürme ilkeleri kullanır. Yavaş arka uç hizmeti yanıtlarını çok artırmış olursunuz.

> [!IMPORTANT]
> **Kapasite** işlenmekte olan istek sayısının doğrudan bir ölçü değil.

![Kapasite ölçüm ani](./media/api-management-capacity/capacity-spikes.png)

**Kapasite** ayrıca aralıklı olarak çıkmasına veya işlenmekte olan istek olsa bile, sıfırdan büyük olması. Sistem veya platforma özgü eylemleri nedeniyle olur ve bir örnek karar verirken dikkate alınmamalıdır.
  
## <a name="use-the-azure-portal-to-examine-capacity"></a>Kapasite incelemek için Azure Portal'ı kullanın
  
![Kapasite ölçümü](./media/api-management-capacity/capacity-metric.png)  

1. APIM Örneğinize gidin [Azure portalında](https://portal.azure.com/).
2. Seçin **ölçümler (Önizleme)**.
3. Mor bölümünden seçin **kapasite** kullanılabilir Ölçümler ve varsayılan olarak bırakın ölçüm **ortalama** toplama.

    > [!TIP]
    > Her zaman gözden geçirmeniz gereken bir **kapasite** ölçüm döküm başına yanlış yorum önlemek için konum.

4. Yeşil bölümünden seçin **konumu** ölçüm boyutuna göre bölmek için.
5. İstenen bir zaman çerçevesi bölümün üst çubuğundan seçin.

    Beklenmeyen bir sorun olduğunda oluşmasını bildirmek için ölçüm uyarısı ayarlayabilirsiniz. Örneğin, APIM Örneğinize 20 dakikadan için beklenen en yüksek kapasitesi aşıldığında bildirimleri alın.

    >[!TIP]
    > Uyarılar hizmetinizi kapasite azaldığında bilmenize Azure İzleyici otomatik ölçeklendirme işlevi otomatik olarak bir Azure API Management birim eklemek için izin verecek şekilde yapılandırabilirsiniz. Ölçeklendirme işlemi yaklaşık 30 dakika sürebilir, bu şekilde kurallarınıza uygun şekilde planlamanız gerekir.  
    > Yalnızca ana konum ölçekleme izin verilir.

## <a name="use-capacity-for-scaling-decisions"></a>Kapasite kararları ölçeklendirmeye yönelik kullanın

**Kapasite** daha fazla yük uyum sağlamak için API Management örneği ölçeklendirme yapılıp kararları için unsurdur. Göz önünde bulundurun:

+ Bir uzun vadeli eğilim ve ortalama aranıyor.
+ Büyük olasılıkla ani yoksayılıyor değil ilgili herhangi bir artış yük (Açıklama "Kapasite ölçüm davranışı" bölümüne bakın).
+ Yükseltme veya ölçeklendirme, örnek zaman **kapasite**ait değer %60 veya % 70'in bir uzun süre (örneğin, 30 dakika) aşıyor. Farklı değerler, hizmeti veya senaryo için daha iyi çalışabilir.

>[!TIP]  
> Trafiğiniz önceden tahmin tamamlayabilirseniz, APIM Örneğinize beklediğiniz iş yükleri üzerinde test edin. Kiracınızda yavaş yavaş istek yükünü artırmak ve kapasite ölçüm hangi değeri için en yüksek yük karşılık gelen izleyin. Önceki bölümde kapasite ne kadar herhangi bir zamanda kullanılan anlamak için Azure portal'ı kullanmak için adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Ölçeklendirin veya bir Azure API Management hizmet örneği yükseltme](upgrade-and-scale.md)