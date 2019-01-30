---
title: Uygulama panosu
titleSuffix: Language Understanding - Azure Cognitive Services
description: Uygulama Panosu, uygulamalarınızı tek bir bakışta izlemenizi sağlar görselleştirilmiş bir raporlama aracına hakkında bilgi edinin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 3f263e6e6b74c1d9392ec58f176962b0c37d70c5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210004"
---
# <a name="model-and-usage-statistics-in-the-dashboard"></a>Panodaki modeli ve kullanım istatistikleri
Uygulama Panosu uygulamanız tek bir bakışta izlemenizi sağlar. **Pano** uygulama adını tıklatarak bir uygulamayı açtığınızda görüntüler **uygulamalarım** seçin sayfasında **Pano** üst panelinden. 

> [!CAUTION]
> LUIS için en güncel ölçümleri isterseniz için gerekir:
> * Bir LUIS'i kullanmayı [uç noktası anahtarı](luis-how-to-azure-subscription.md) Azure'da oluşturuldu
> * LUIS dahil olmak üzere tüm uç nokta istekleri LUIS uç noktası anahtarı kullan [API](https://aka.ms/luis-endpoint-apis) Robotu
> * Farklı uç noktası anahtarı her LUIS uygulaması için kullanın. Tek bir uç noktası anahtarı tüm uygulamalar için kullanmayın. Uç nokta, temel düzeyde, uygulama düzeyinde izlenir.  

**Pano** sayfa size geçerli modeli de dahil olmak üzere LUIS uygulaması genel bir bakış ile birlikte durum [uç nokta](luis-glossary.md#endpoint) kullanım zaman içinde. 
  
## <a name="app-status"></a>Uygulama durumu
Uygulamanın eğitim panoyu görüntüler ve eğitim ve yayımlanan uygulama en son ne zaman saat ve tarihi içeren durumu yayımlama.  

![Pano - uygulama durumu](./media/luis-how-to-use-dashboard/app-state.png)

## <a name="model-data-statistics"></a>Model veri istatistikleri
Pano amacı, varlıkları ve uygulamada mevcut etiketli konuşma toplam sayıda görüntüler. 

![Uygulama verilerini istatistikleri](./media/luis-how-to-use-dashboard/app-model-count.png)

## <a name="endpoint-hits"></a>Uç noktası İsabeti
Pano LUIS uygulaması alıp belirtmek, görüntülenecek bir süre içinde İsabetleri etkinleştirir toplam uç noktası İsabeti görüntüler. Toplam görüntülenme sayısı kullanan uç noktası İsabeti toplamıdır. bir [uç noktası anahtarı](./luis-concept-keys.md#endpoint-key) ve uç noktası kullanan isabet bir [yazma anahtar](./luis-concept-keys.md#authoring-key).

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

1. Tıklayın **ek ayarlar** ![erişim listesine ek ayarlar düğmesi](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) aşağıdaki görüntüde gösterildiği gibi listesine erişmek için:

    ![Hedefi çözümleme listesi](./media/luis-how-to-use-dashboard/intent-breakdown-based-on.png)
2. Listeden bir değer seçin ve ardından geri okunu tıklatın ![Geri oku](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) Grafiğe görüntülenecek.

## <a name="entity-breakdown"></a>Varlık dökümü
Pano varlık etiketli konuşma veya uç noktası İsabeti göre dökümünü gösterir. Bu Özet Grafiği, uygulamayı her varlık göreceli önemini gösterir. Bir dilim fare işaretçisini getirdiğinizde, varlık adı ve yüzde olarak etiketlenmiş konuşma/uç noktası İsabeti bakın. 

![Varlık dökümü](./media/luis-how-to-use-dashboard/entity-breakdown.png)

Dökümü etiketli konuşma veya uç noktası İsabeti dayalı olup olmadığını denetlemek için:

1. Tıklayın **ek ayarlar** ![listesini almak için ek ayarlar düğmesi](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) aşağıdaki görüntüde gösterildiği gibi listesine erişmek için:

    ![Varlık çözümleme listesi](./media/luis-how-to-use-dashboard/entity-breakdown-based-on.png)
2. Listeden bir değer seçin ve ardından geri okunu tıklatın ![Geri oku](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) Grafiği uygun şekilde görüntülemek için.
