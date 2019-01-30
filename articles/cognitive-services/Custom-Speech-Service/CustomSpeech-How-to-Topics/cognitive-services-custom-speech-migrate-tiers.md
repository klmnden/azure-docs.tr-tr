---
title: Azure üzerinde fiyatlandırma katmanları özel konuşma Hizmeti uç noktaları, geçiş | Microsoft Docs
description: Bilişsel hizmetler, S2 özel konuşma Hizmeti uç noktalarına S0 ve S1 katmanı arasından dağıtımlarını geçirmeyi öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 07/05/2017
ms.author: panosper
ms.openlocfilehash: 71aa20c779ae0c73db3d7ce6f267524c5bf71ea5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214577"
---
# <a name="migrate-deployments-to-the-new-pricing-model"></a>Dağıtımlar için yeni fiyatlandırma modeline geçirme

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Temmuz 2017'den itibaren özel konuşma hizmeti sunan bir [yeni fiyatlandırma modeline](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/). Yeni model *daha kolay anlaşılır*, *maliyetleri hesaplamak daha basit*, ve *daha esnek* ölçeklendirme bakımından. Ölçeklendirme için bir ölçek birimi kavramı Microsoft sundu. Her ölçek birimi, beş eş zamanlı istekleri işleyebilir. 5 eşzamanlı istek S0 katmanı için en eski modeldeki eş zamanlı istekleri için ölçeklendirme ayarlandı ve S1 katmanı için 12 eş zamanlı istek sırasında ayarlandı. Biz size daha fazla esneklik, kullanım örneği gereksinimlerine sahip sunmak için limitler açtınız.

Eski S0 ya da S1 katmanı çalıştırırsanız, mevcut dağıtımlarınızı yeni S2 katmanı için geçiş öneririz. Yeni S2 katmanı S0 ve S1 katmanlarını kapsamaktadır. Aşağıdaki şekilde kullanılabilir seçenekleri görebilirsiniz:

!["Fiyatlandırma katmanınızı seçin" sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-pricing-tier.png)

Microsoft geçiş yarı otomatik bir şekilde işler. İlk olarak, yeni bir fiyatlandırma katmanı seçerek geçiş tetikleyin. Ardından, biz dağıtımınızı otomatik olarak geçirin.

Ölçek birimleri için eski katmanları eşlemesinden aşağıdaki tabloda gösterilmiştir:

| Katman | Eş zamanlı istek (eski model) | Geçiş | Eş zamanlı istek |
|----- | ----- | ---- | ---- |
| S0 |  5   |   => **S2** 1 ölçek birimi ile |   5 |
| S1 |  12  |   => **S2** 3 ölçek birimleri ile |  15 |

Yeni katmanına geçirmek için aşağıdakileri yapın:

## <a name="step-1-check-your-existing-deployment"></a>1. Adım: Mevcut dağıtımınızı denetleyin
Git [özel konuşma hizmeti portalı](http://cris.ai)ve mevcut dağıtımlarınızı denetleyin. Bu örnekte iki dağıtım vardır. Bir dağıtım bir S0 katmanı üzerinde çalışır ve diğer dağıtım bir S1 katmanında çalışır. Dağıtımları gösterilen **dağıtım seçenekleri** aşağıdaki tablo, sütun:

![Dağıtımları sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments.png)

## <a name="step-2-select-your-new-pricing-tier-in-the-azure-portal"></a>2. Adım: Azure portalında yeni fiyatlandırma katmanınızı seçin
1. Yeni bir tarayıcı sekmesi açın ve oturum [Azure portalında](http://ms.portal.azure.com/). 

2. İçinde **Bilişsel Hizmetler** bölmesinde, **abonelikleri** listesinde, özel konuşma aboneliğinizi seçin. 

3. Aboneliğiniz için bölmesinde seçin **fiyatlandırma katmanı**.

    !["Fiyatlandırma katmanı" bağlantısı](../../../media/cognitive-services/custom-speech-service/custom-speech-update-tier.png)

4. Üzerinde **fiyatlandırma katmanınızı seçin** sayfasında **S2 standart**. Bu fiyatlandırma katmanı, yeni, basit ve daha esnek bir fiyatlandırma katmanı içindir.

5. Seçin **seçin** düğmesi.

    !["Fiyatlandırma katmanınızı seçin" sayfası](../../../media/cognitive-services/custom-speech-service/custom-speech-update-pricing.png)

## <a name="step-3-check-the-migration-status-in-the-custom-speech-service-portal"></a>3. Adım: Özel konuşma hizmeti Portalı'nda geçiş durumunu denetle
Özel konuşma hizmeti portala geri dönün ve dağıtımlarınızı denetleyin. (Tarayıcı pencerenizi hâlâ açıksa yenileyin.) 

İlgili dağıtım durumu için geçtiniz *işleme*. Denetleyerek geçiş doğrulayabilirsiniz **dağıtım seçenekleri** sütun. Var. Şimdi ölçek birimleri ve günlüğe kaydetme hakkında bilgi bulabilirsiniz. Ölçek birimleri, önceki fiyatlandırma katmanınızı yansıtmalıdır. Günlük kaydı ayrıca tabloda gösterildiği gibi açık:

![Dağıtım seçenekleri sütun](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments-new.png)


> [!NOTE]
> Geçiş sırasında herhangi bir sorun varsa bizimle iletişim kurun.
>

## <a name="next-steps"></a>Sonraki adımlar
Diğer öğreticiler için bkz:
* [Özel akustik model oluşturma](cognitive-services-custom-speech-create-acoustic-model.md)
* [Özel dil modeli oluşturma](cognitive-services-custom-speech-create-language-model.md)
* [Özel Konuşmayı metne uç nokta oluşturma](cognitive-services-custom-speech-create-endpoint.md)
