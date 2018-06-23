---
title: Azure üzerinde özel konuşma hizmetiyle bir özel konuşma uç noktası oluşturma | Microsoft Docs
description: Bilişsel hizmetler özel konuşma hizmetinde ile özel bir konuşma metin uç noktası oluşturmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: 99bc275db1f0c1b45b3db440d2e03d0db9ab5cf6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351634"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Özel bir konuşma metin uç noktası oluşturma
Özel akustik modelleri veya dil modelleri oluşturduktan sonra özel bir konuşma metin uç noktası dağıtabilirsiniz. 

## <a name="create-an-endpoint"></a>Uç nokta oluşturma
Yeni bir özel uç noktası oluşturmak için seçin **dağıtımları** üzerinde **özel konuşma** sayfanın üstündeki menü. Bu, olanak eylemde **dağıtımları** sayfasında, geçerli özel uç noktaları bir tablo içeriyor. Uç nokta henüz oluşturmadıysanız, tablonun boş olur. Geçerli yerel tablo başlığı yansıtılır. 

Farklı bir dil için bir dağıtım oluşturmak için seçin **yerel ayarını Değiştir**. Desteklenen diller hakkında daha fazla bilgi için bkz: [yerel özel konuşma hizmetinde desteklenen](cognitive-services-custom-speech-change-locale.md).

Yeni bir uç noktası oluşturmak için seçin **Yeni Oluştur**. İçinde **oluşturma dağıtım** bölmesinde, bilgi girin **adı** ve **açıklama** özel dağıtımınızın kutuları.

İçinde **abonelik** açılan kutusunda, kullanmak istediğiniz aboneliği seçin. S2 abonelik ise, Ölçek birimleri ve içerik günlük seçebilirsiniz. Ölçek birimleri ve günlüğe kaydetme hakkında daha fazla bilgi için bkz: [özel konuşma hizmet Ölçümler ve kotaları](../cognitive-services-custom-speech-meters.md).

Aşağıdaki tabloda, Ölçek birimleri için kullanılabilir eşzamanlı istek nasıl karşıladığı gösterilmektedir:

| Ölçek birimi | Eşzamanlı istek sayısı |
| ------ | ----- |
| 0 | 1 |
| 1 | 5 |
| 2 | 10 |
| 3 | 15 |
| n | 5 * n |

İçerik Günlük açmak veya kapatmak anahtarlanır de seçebilirsiniz. Uç nokta trafiğini Microsoft dahili kullanımı için depolanan olup olmadığını diğer bir deyişle, seçmiş olursunuz. Seçilmezse, trafiği depolama gizlenir. Ek bir maliyet içerik günlük sonuçlarında engelleniyor. Başvurun [fiyatlandırma bilgileri sayfası](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) Ayrıntılar için.

> [!NOTE]
> İçerik günlüğe kaydetme, fiyatlandırma sayfası "No izleme" adı verilir.
>


Ayrıca, Ölçek birimleri ve içerik günlüğü maliyetlerinde etkisi farkında; böylece Microsoft kabaca bir maliyetleri tahmin sağlar. Bu tahmin kaba bir tahmindir ve fiili maliyetleriniz farklı olabilir.

> [!NOTE]
> Bu ayarları F0 (ücretsiz katmanı) abonelikleri ile kullanılabilir değil.
>

İçinde **akustik modeli** listesinde, kullanmak istediğiniz akustik modeli seçin ve **dil modeli** listesinde, kullanmak istediğiniz dil modeli seçin. Kullanım ve dil modelleri için her zaman temel Microsoft modelleri seçenekleri. Temel model seçimi birleşimleri sınırlar. Konuşma temel modelleri ile arama karıştırmak ve temel modelleri dikte.

![Dağıtım oluşturma sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-create2.png)

> [!NOTE]
> Kullanım koşulları ve onay kutusunu seçerek fiyatlandırma bilgilerini kabul edecek şekilde emin olun.
>

Kullanım ve dil Modellerinizi seçtikten sonra Seç **oluşturma**. Bu eylem için döndürür **dağıtımları** sayfası. Tablo artık yeni uç noktanızı karşılık gelen bir giriş içerir. Bunu oluşturulurken uç noktanın durumu geçerli durumunu yansıtır. Özel Modellerinizi ile yeni bir uç noktası örneği 30 dakika kadar sürebilir. Dağıtım durumunu olduğunda *tam*, uç noktayı kullanıma hazırdır.

![Dağıtımları sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-ready.png)

Dağıtıma hazır olduğunda, dağıtım adı bir bağlantı olur. Bağlantısını seçerek görüntüler **dağıtım bilgileri** ya da bir HTTP isteğiyle kullanmak için özel uç noktanızı URL'lerini veya Microsoft Bilişsel hizmetler konuşma istemci web Yuvalarını kullanan kitaplığı, görüntüler sayfa.

![Dağıtım bilgileri sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-info2.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla öğretici için bkz:
* [Özel bir konuşma metin uç noktası kullan](cognitive-services-custom-speech-use-endpoint.md)
* [Özel bir akustik modeli oluşturma](cognitive-services-custom-speech-create-acoustic-model.md)
* [Özel dil modeli oluşturma](cognitive-services-custom-speech-create-language-model.md)
