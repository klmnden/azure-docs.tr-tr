---
title: Azure üzerinde fiyatlandırma katmanlarına uç noktalar özel konuşma hizmetinden geçirme | Microsoft Docs
description: Bilişsel hizmetler, S2 özel konuşma hizmet uç noktalarına S0 ve S1 katmanları dağıtımlarını geçirmeyi öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/05/2017
ms.author: panosper
ms.openlocfilehash: 6d92459deb3464cd97c215cbf9a8320628b6da80
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352427"
---
# <a name="migrate-deployments-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeli dağıtımlarını geçirme
Temmuz 2017 itibariyle özel konuşma hizmeti sunan bir [yeni fiyatlandırma modeli](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/). Yeni model *anlamak daha kolay*, *maliyetlerini hesaplamak daha basit*, ve *daha esnek* ölçeklendirme bakımından. Microsoft, ölçekleme için bir ölçek birimi kavramı anlatılmıştır. Her bir ölçek birimi beş eş zamanlı istekleri işleyebilir. Katmanı S0 5 eş zamanlı istekleri en eski modelinde eş zamanlı istekleri için ölçeklendirme ayarlandı ve 12 eşzamanlı istek S1 katmanı için en ayarlandı. Biz, daha fazla esneklik kullanım örneği gereksinimlerinizi ile sunmak üzere bu sınırlar açtınız.

Eski S0 veya S1 katmanı çalıştırırsanız, varolan dağıtımlarınızı yeni S2 katmanına geçirmek öneririz. Yeni S2 katmanı S0 ve S1 katmanları kapsar. Aşağıdaki şekil kullanılabilir seçenekleri görebilirsiniz:

!["Fiyatlandırma katmanınızı seçin" sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-pricing-tier.png)

Microsoft geçiş yarı otomatik bir şekilde işler. İlk olarak, yeni fiyatlandırma katmanı seçerek geçiş tetikler. Ardından, biz dağıtımınızı otomatik olarak geçirin.

Birimleri ölçeklendirmek için eski katmanları arasında eşlemesi aşağıdaki tabloda gösterilmiştir:

| Katman | Eşzamanlı istek (eski modeli) | Geçiş | Eşzamanlı istek |
|----- | ----- | ---- | ---- |
| S0 |  5   |   => **S2** 1 ölçek birimi ile |   5 |
| S1 |  12  |   => **S2** 3 ölçek birimleri ile |  15 |

Yeni Katman geçirmek için aşağıdakileri yapın:

## <a name="step-1-check-your-existing-deployment"></a>1. adım: mevcut dağıtımınıza denetleyin
Git [özel konuşma hizmet portalı](http://cris.ai)ve varolan dağıtımları denetleyin. Bizim örneğimizde, iki dağıtım vardır. Bir dağıtım S0 katmanı üzerinde çalışır ve diğer dağıtım S1 katmanı üzerinde çalışır. Dağıtımı'nda gösterilen **dağıtım seçenekleri** aşağıdaki tablo, sütun:

![Dağıtımları sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments.png)

## <a name="step-2-select-your-new-pricing-tier-in-the-azure-portal"></a>2. adım: Azure portalında yeni fiyatlandırma katmanınızı seçin
1. Yeni bir tarayıcı sekmesi açın ve oturum [Azure portal](http://ms.portal.azure.com/). 

2. İçinde **Bilişsel Hizmetler** bölmesi, **abonelikleri** listesinde, özel konuşma aboneliğinizi seçin. 

3. Aboneliğiniz için bölmesinde seçin **fiyatlandırma katmanı**.

    !["Fiyatlandırma katmanı" bağlantı](../../../media/cognitive-services/custom-speech-service/custom-speech-update-tier.png)

4. Üzerinde **fiyatlandırma katmanınızı seçin** sayfasında, **S2 standart**. Bu fiyatlandırma katmanı yeni, basit ve daha esnek bir fiyatlandırma Katmanı ' dir.

5. Seçin **seçin** düğmesi.

    !["Fiyatlandırma katmanınızı seçin" sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-update-pricing.png)

## <a name="step-3-check-the-migration-status-in-the-custom-speech-service-portal"></a>3. adım: özel konuşma servis Portalı'nda geçiş durumunu denetle
Özel konuşma hizmet portalına dönün ve dağıtımlarınızı denetleyin. (Tarayıcı pencerenizi hala açıksa yenileyin.) 

İlgili dağıtım durumunu anahtarlı *işleme*. Denetleyerek geçiş doğrulayabilirsiniz **dağıtım seçenekleri** sütun. Var. Şimdi ölçek birimleri ve günlüğe kaydetme hakkında bilgi bulabilirsiniz. Ölçek birimleri önceki fiyatlandırma katmanınızı yansıtmalıdır. Günlüğe kaydetme ayrıca tabloda gösterildiği gibi açık:

![Dağıtım seçenekleri sütun](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments-new.png)


> [!NOTE]
> Geçiş sırasında bir sorunla karşılaşırsanız, bize başvurun.
>

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla öğretici için bkz:
* [Özel bir akustik modeli oluşturma](cognitive-services-custom-speech-create-acoustic-model.md)
* [Özel dil modeli oluşturma](cognitive-services-custom-speech-create-language-model.md)
* [Özel bir konuşma metin uç noktası oluşturma](cognitive-services-custom-speech-create-endpoint.md)
