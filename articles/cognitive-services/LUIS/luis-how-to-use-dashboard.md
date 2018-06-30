---
title: Uygulama Panosu HALUK uygulamalar için | Microsoft Docs
description: Uygulama Panosu, uygulamalarınızı tek bir bakışta izlemenizi sağlar görselleştirilmiş bir raporlama aracı hakkında bilgi edinin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: c7ef38e2f2edaf795d3d76706afd4aa09b3b6959
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110057"
---
# <a name="application-dashboard"></a>Uygulama Panosu
Uygulama Panosu uygulamanızın tek bir bakışta izlemenizi sağlar. **Pano** uygulama adı tıklayarak bir uygulama açtığınızda görüntüler **My uygulamaları** seçin sayfasında **Pano** üst panelinden. 

> [!CAUTION]
> HALUK için en güncel ölçümleri istiyorsanız için gerekir:
> * Bir HALUK kullanmak [uç noktası anahtarı](luis-how-to-azure-subscription.md) Azure'da oluşturuldu
> * Kullanım HALUK uç noktası anahtarı HALUK dahil olmak üzere tüm uç nokta istekler için [API](https://aka.ms/luis-endpoint-apis) ve bot
> * Farklı uç noktası anahtarı her HALUK uygulama için kullanın. Tek bir uç noktası anahtarı tüm uygulamalar için kullanmayın. Uç noktası anahtarı, uygulama düzeyinde anahtar düzeyinde izlenir.  

**Pano** sayfa size geçerli modeli de dahil olmak üzere HALUK uygulama genel bir bakış yanı durum [endpoint](luis-glossary.md#endpoint) zamanla kullanımı. <!--The following image shows the **Dashboard** page.-->

<!-- TBD: Get a working screen shot
![The Dashboard](./media/luis-how-to-use-dashboard/dashboard.png)
-->

<!-- TBD: IS THIS STILL TRUE?
At the top of the **Dashboard** page, a contextual notification bar constantly displays notifications to update you on the required or recommended actions appropriate for the current state of your app. It also provides useful tips and alerts as needed. A detailed description of the data reported on the **Dashboard** page follows.
-->
  
## <a name="app-status"></a>Uygulama durumu
Pano uygulamanın eğitim görüntüler ve yayımlama durumu, uygulamanın ne zaman son saat ve tarihi dahil olmak üzere eğitilmiş yayımlanır.  

![Pano - uygulama durumu](./media/luis-how-to-use-dashboard/app-state.png)

## <a name="model-data-statistics"></a>Model veri istatistikleri
Pano toplam sayıda hedefleri, varlıkları ve uygulamada varolan etiketli utterances görüntüler. 

![Uygulama verileri istatistikleri](./media/luis-how-to-use-dashboard/app-model-count.png)

## <a name="endpoint-hits"></a>Uç noktası isabet sayısı
Pano HALUK uygulama alır ve bir süre içinde görüntülemek için isabetler etkinleştirir belirten toplam uç noktası isabet görüntüler. Görüntülenen toplam sayısı kullanan uç noktası isabet toplamıdır bir [uç noktası anahtarı](./luis-concept-keys.md#endpoint-key) ve uç nokta isabetler kullanan bir [yazma anahtar](./luis-concept-keys.md#authoring-key).

<!-- TBD: this image is old but I don't have a new one based on usage -->
![Uç noktası isabet sayısı](./media/luis-how-to-use-dashboard/dashboard-endpointhits.png)

> [!NOTE] 
> En güncel uç noktası isabet sayısı HALUK hizmetine genel bakış Azure Portal'da bulunmaktadır. 
 
### <a name="total-endpoint-hits"></a>Toplam uç noktası isabet sayısı
Uç noktası isabet, uygulamanızın uygulama oluşturma içinden, geçerli tarihe kadar bu yana alınan toplam sayısı.

### <a name="endpoint-hits-per-period"></a>Nokta başına uç noktası isabet
İsabet sayısı günde görüntülenen bir son dönemde aldı. Başlangıç ve bitiş tarihleri arasında noktaları bu dönemde dönmeden günleri temsil eder. Fare işaretçisini görmek için her noktası üzerinde süre içinde her gün içinde isabet sayısı. 

Grafikte görüntülemek için bir süre seçmek için:
 
1. Tıklatın **ek ayarlar** ![ek ayarlar düğmesi](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) nokta listesine erişmek için. Bir haftada bir yıla kadar uzanan dönemlerde seçebilirsiniz. 

    ![Nokta başına uç noktası isabet](./media/luis-how-to-use-dashboard/timerange.png)

2. Listeden bir süre seçin ve sonra geri oku tıklatın ![Geri oku](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) Grafik görüntülemek için.

### <a name="key-usage"></a>Anahtar kullanımı
Uygulama uç noktası anahtarından tüketilen isabet sayısı. Uç nokta anahtarları hakkında daha fazla bilgi için bkz: [HALUK anahtarlarında](luis-concept-keys.md). 
  
## <a name="intent-breakdown"></a>Hedefi dökümü
**Hedefi çözümleme** etiketli utterances veya uç noktası isabet göre hedefleri dökümünü gösterir. Bu Özet Grafiği uygulamada her amacı göreceli önemini gösterir. Fare işaretçisini bir dilim geldiğinizde hedefi adı ve etiketli utterances/uç noktanın isabetli okuma sayısının toplam sayı bu sayıyı yüzdesini temsil ettiği bakın. 

![Hedefi dökümü](./media/luis-how-to-use-dashboard/intent-breakdown.png)

Çözümleme etiketli utterances veya uç noktası isabet göre denetlemek için:

1. Tıklatın **ek ayarlar** ![ek ayarlar düğmesi](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) aşağıdaki görüntüde olduğu gibi listesine erişmek için:

    ![Hedefi çözümleme listesi](./media/luis-how-to-use-dashboard/intent-breakdown-based-on.png)
2. Listeden bir değer seçin ve sonra geri oku tıklatın ![Geri oku](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) Grafik görüntülemek için.

## <a name="entity-breakdown"></a>Varlık dökümü
Pano etiketli utterances veya uç noktası isabet göre varlıklar dökümünü gösterir. Bu Özet Grafiği, uygulamada her varlık göreceli önemini gösterir. Fare işaretçisini bir dilim getirdiğinizde, varlık adı ve etiketli utterances/uç noktası isabet yüzde bakın. 

![Varlık dökümü](./media/luis-how-to-use-dashboard/entity-breakdown.png)

Çözümleme etiketli utterances veya uç noktası isabet göre denetlemek için:

1. Tıklatın **ek ayarlar** ![ek ayarlar düğmesi](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) aşağıdaki görüntüde olduğu gibi listesine erişmek için:

    ![Varlık çözümleme listesi](./media/luis-how-to-use-dashboard/entity-breakdown-based-on.png)
2. Listeden bir değer seçin ve sonra geri oku tıklatın ![Geri oku](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) Grafik buna göre görüntülemek için.
