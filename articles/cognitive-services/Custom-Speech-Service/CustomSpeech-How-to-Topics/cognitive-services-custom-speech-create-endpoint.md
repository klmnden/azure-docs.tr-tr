---
title: Azure'da özel konuşma hizmeti ile özel konuşma tanıma uç noktası oluşturma | Microsoft Docs
description: Bilişsel hizmetler'deki özel konuşma hizmeti ile özel bir konuşmayı metne uç noktası oluşturmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: ed93afa8e10fdfbb0d45f4500b4a648716e25e00
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46952231"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Özel bir konuşmayı metne dönüştürme uç noktası oluşturma
Özel akustik modeller veya dil modellerini oluşturduktan sonra özel bir konuşmayı metne uç noktasına dağıtabilirsiniz. 

## <a name="create-an-endpoint"></a>Uç nokta oluşturma
Yeni özel uç nokta oluşturmak için Seç **dağıtımları** üzerinde **özel konuşma** sayfanın üstündeki menü. Bu eylem açılır **dağıtımları** geçerli özel uç noktalar tablosunu içeren sayfa. Uç nokta henüz oluşturmadıysanız, boş bir tablodur. Geçerli yerel ayar, tablo başlığında gösterilir. 

Farklı bir dil için bir dağıtım oluşturmak için Seç **yerel ayarını Değiştir**. Desteklenen diller hakkında daha fazla bilgi için bkz. [desteklenen yerel ayarlar özel konuşma hizmeti](cognitive-services-custom-speech-change-locale.md).

Yeni bir uç nokta oluşturmak için Seç **Yeni Oluştur**. İçinde **dağıtım oluşturma** bölmesinde, bilgi girin **adı** ve **açıklama** özel dağıtımınızın kutuları.

İçinde **abonelik** birleşik giriş kutusunda, kullanmak istediğiniz aboneliği seçin. Bir S2 aboneliği ise, Ölçek birimleri ve içerik günlüğünü seçebilirsiniz. Ölçek birimleri ve günlüğe kaydetme hakkında daha fazla bilgi için bkz: [özel konuşma hizmeti ölçümleri ve kotalar](../cognitive-services-custom-speech-meters.md).

Aşağıdaki tabloda, Ölçek birimleri kullanılabilir eş zamanlı isteğin nasıl eşleştiği gösterilmektedir:

| Ölçek birimi | Eş zamanlı istek sayısı |
| ------ | ----- |
| 0 | 1 |
| 1 | 5 |
| 2 | 10 |
| 3 | 15 |
| n | 5 * n |

İçerik günlüğü açıp anahtarlanır de seçebilirsiniz. Uç nokta trafiği için yalnızca Microsoft dahili kullanımı depolanmasından bağımsız diğer bir deyişle, seçeneğini belirliyoruz. Seçilmezse, trafiği depolama gizlenir. Ek ücret sonuçları içerik günlüğü engelleme. Başvurun [fiyatlandırma bilgileri sayfası](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) Ayrıntılar için.

> [!NOTE]
> İçerik günlüğü fiyatlandırma sayfası "No izleme" adı verilir.
>


Ayrıca, Ölçek birimleri ve içerik günlüğü maliyetlerini üzerindeki etkiyi farkında, böylece Microsoft maliyetleri kabaca bir tahmin sağlar. Bu tahmin, kaba bir tahmindir ve gerçek maliyetlerinizi farklı olabilir.

> [!NOTE]
> Bu ayarlar F0 (ücretsiz katman için) abonelikleriyle kullanılamaz.
>

İçinde **akustik Model** listesinde, kullanmak istediğiniz akustik model seçin ve **dil modeli** listesinde, istediğiniz dil modelini seçin. Akustik ve dil modellerini seçenekleri, her zaman temel Microsoft modelleri ekleyin. Birleşimler ve temel modele seçimini sınırlar. Temel konuşma modelleri ile arama karışımı ve temel modele dikte olamaz.

![Dağıtım oluşturma sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-create2.png)

> [!NOTE]
> Kullanım koşulları ve onay kutusunu seçerek fiyatlandırma bilgileri kabul emin olun.
>

Akustik ve dil Modellerinizi seçtikten sonra seçin **Oluştur**. Bu eylem, döndürür **dağıtımları** sayfası. Tablo, artık yeni uç noktanıza karşılık gelen bir giriş içerir. Bunu oluşturulurken uç noktasının durumu geçerli durumunu yansıtır. Bu özel Modellerinizi ile yeni bir uç noktayı örneklemek için en fazla 30 dakika sürebilir. Dağıtım durumu olduğunda *tam*, uç noktayı kullanıma hazırdır.

![Dağıtımları sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-ready.png)

Dağıtım adı, dağıtıma hazır olduğunda, bir bağlantı olur. Bağlantıyı seçerek görüntüler **dağıtım bilgilerini** ya da bir HTTP isteği ile kullanılacak özel uç noktanıza URL'lerini veya Microsoft Bilişsel hizmetler konuşma istemcisi kullanan web yuvalarını kitaplığını, görüntüleyen sayfa.

![Dağıtım bilgileri sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-info2.png)

## <a name="next-steps"></a>Sonraki adımlar

Diğer öğreticiler için bkz:
* [Özel bir konuşmayı metne uç noktası kullan](cognitive-services-custom-speech-use-endpoint.md)
* [Özel akustik model oluşturma](cognitive-services-custom-speech-create-acoustic-model.md)
* [Özel dil modeli oluşturma](cognitive-services-custom-speech-create-language-model.md)
