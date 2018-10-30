---
title: Language Understanding uygulamalar için uygulama Panosu
titleSuffix: Azure Cognitive Services
description: Uygulama Panosu, uygulamalarınızı tek bir bakışta izlemenizi sağlar görselleştirilmiş bir raporlama aracına hakkında bilgi edinin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: diberry
ms.openlocfilehash: 6a4e8dbee34402f57d3e697e93d10573aaf10998
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212826"
---
# <a name="application-dashboard"></a>Uygulama Panosu
Uygulama Panosu uygulamanız tek bir bakışta izlemenizi sağlar. **Pano** uygulama adını tıklatarak bir uygulamayı açtığınızda görüntüler **uygulamalarım** seçin sayfasında **Pano** üst panelinden. 

> [!CAUTION]
> LUIS için en güncel ölçümleri isterseniz için gerekir:
> * Bir LUIS'i kullanmayı [uç noktası anahtarı](luis-how-to-azure-subscription.md) Azure'da oluşturuldu
> * LUIS dahil olmak üzere tüm uç nokta istekleri LUIS uç noktası anahtarı kullan [API](https://aka.ms/luis-endpoint-apis) Robotu
> * Farklı uç noktası anahtarı her LUIS uygulaması için kullanın. Tek bir uç noktası anahtarı tüm uygulamalar için kullanmayın. Uç nokta, temel düzeyde, uygulama düzeyinde izlenir.  

**Pano** sayfa size geçerli modeli de dahil olmak üzere LUIS uygulaması genel bir bakış ile birlikte durum [uç nokta](luis-glossary.md#endpoint) kullanım zaman içinde. <!--The following image shows the **Dashboard** page.-->

<!-- TBD: Get a working screen shot
![The Dashboard](./media/luis-how-to-use-dashboard/dashboard.png)
-->

<!-- TBD: IS THIS STILL TRUE?
At the top of the **Dashboard** page, a contextual notification bar constantly displays notifications to update you on the required or recommended actions appropriate for the current state of your app. It also provides useful tips and alerts as needed. A detailed description of the data reported on the **Dashboard** page follows.
-->
  
## <a name="app-status"></a>Uygulama durumu
Uygulamanın eğitim panoyu görüntüler ve eğitim ve yayımlanan uygulama en son ne zaman saat ve tarihi içeren durumu yayımlama.  

![Pano - uygulama durumu](./media/luis-how-to-use-dashboard/app-state.png)

## <a name="model-data-statistics"></a>Model veri istatistikleri
Pano amacı, varlıkları ve uygulamada mevcut etiketli konuşma toplam sayıda görüntüler. 

![Uygulama verilerini istatistikleri](./media/luis-how-to-use-dashboard/app-model-count.png)

## <a name="endpoint-hits"></a>Uç noktası İsabeti
Pano LUIS uygulaması alıp belirtmek, görüntülenecek bir süre içinde İsabetleri etkinleştirir toplam uç noktası İsabeti görüntüler. Toplam görüntülenme sayısı kullanan uç noktası İsabeti toplamıdır. bir [uç noktası anahtarı](./luis-concept-keys.md#endpoint-key) ve uç noktası kullanan isabet bir [yazma anahtar](./luis-concept-keys.md#authoring-key).

<!-- TBD: this image is old but I don't have a new one based on usage -->
![Uç noktası İsabeti](./media/luis-how-to-use-dashboard/dashboard-endpointhits.png)

> [!NOTE] 
> En güncel uç noktası isabet sayısı LUIS hizmetine genel bakış Azure portalında bulunmaktadır. 
 
### <a name="total-endpoint-hits"></a>Toplam uç noktası İsabeti
Uç nokta isabet içinden, geçerli tarihe kadar uygulama oluşturulmasından itibaren uygulamanıza alınan toplam sayısı.

### <a name="endpoint-hits-per-period"></a>Uç noktası İsabeti dönemi başına
İsabet sayısı günde görüntülenen son bir süre içinde aldı. Başlangıç ve bitiş tarihleri arasında noktaları bu dönemde kalan gün temsil eder. Fare işaretçisini görmek için her noktası üzerinde her gün dönemi içinde isabet sayısı. 

Grafikte görüntülenecek bir süre seçmek için:
 
1. Tıklayın **ek ayarlar** ![ek ayarlar düğmesi](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) nokta listesine erişmek için. Bir haftadan oluşturan bir yıla kadar uzanan dönemlerde seçebilirsiniz. 

    ![Uç noktası İsabeti dönemi başına](./media/luis-how-to-use-dashboard/timerange.png)

2. Listeden bir süre seçin ve ardından geri okunu tıklatın ![Geri oku](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) Grafiğe görüntülenecek.

### <a name="key-usage"></a>Anahtar kullanımı
Uygulamanın uç nokta anahtarından tüketilen isabet sayısı. Uç nokta anahtarları hakkında daha fazla bilgi için bkz. [LUIS anahtarlarında](luis-concept-keys.md). 
  
## <a name="intent-breakdown"></a>Çözümleme hedefi
**Hedefi dökümü** ıntents etiketli konuşma veya uç noktası İsabeti göre dökümünü gösterir. Bu Özet Grafiği, her amaç göreceli önemini uygulamada gösterir. Fare işaretçisini bir dilim geldiğinizde hedefi adı ve temsil ettiği etiketli konuşma/uç noktanın isabetli okuma sayısının toplam sayısı yüzdesi bakın. 

![Çözümleme hedefi](./media/luis-how-to-use-dashboard/intent-breakdown.png)

Dökümü etiketli konuşma veya uç noktası İsabeti dayalı olup olmadığını denetlemek için:

1. Tıklayın **ek ayarlar** ![ek ayarlar düğmesi](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) aşağıdaki görüntüde gösterildiği gibi listesine erişmek için:

    ![Hedefi çözümleme listesi](./media/luis-how-to-use-dashboard/intent-breakdown-based-on.png)
2. Listeden bir değer seçin ve ardından geri okunu tıklatın ![Geri oku](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) Grafiğe görüntülenecek.

## <a name="entity-breakdown"></a>Varlık dökümü
Pano varlık etiketli konuşma veya uç noktası İsabeti göre dökümünü gösterir. Bu Özet Grafiği, uygulamayı her varlık göreceli önemini gösterir. Bir dilim fare işaretçisini getirdiğinizde, varlık adı ve yüzde olarak etiketlenmiş konuşma/uç noktası İsabeti bakın. 

![Varlık dökümü](./media/luis-how-to-use-dashboard/entity-breakdown.png)

Dökümü etiketli konuşma veya uç noktası İsabeti dayalı olup olmadığını denetlemek için:

1. Tıklayın **ek ayarlar** ![ek ayarlar düğmesi](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) aşağıdaki görüntüde gösterildiği gibi listesine erişmek için:

    ![Varlık çözümleme listesi](./media/luis-how-to-use-dashboard/entity-breakdown-based-on.png)
2. Listeden bir değer seçin ve ardından geri okunu tıklatın ![Geri oku](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) Grafiği uygun şekilde görüntülemek için.
