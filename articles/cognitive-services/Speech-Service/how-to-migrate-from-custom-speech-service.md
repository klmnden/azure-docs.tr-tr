---
title: Özel konuşma tanıma hizmetinden konuşma Services'a geçme
titlesuffix: Azure Cognitive Services
description: Özel konuşma hizmeti artık konuşma Hizmetleri bir parçasıdır. En son kalite ve özellik güncelleştirmeleri yararlanmak için konuşma Hizmetleri geçin.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 8a2c149faa0ec9d135713a123a33d7c220522496
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60995669"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Konuşma hizmeti için özel konuşma hizmeti geçirme

Uygulamalarınızı konuşma hizmeti için özel konuşma tanıma hizmetinden geçirmek için bu makaleyi kullanın.

Özel konuşma hizmeti artık konuşma hizmeti bir parçasıdır. En son kalite ve özellik güncelleştirmeleri yararlanmak için konuşma Hizmetleri geçin.

## <a name="migration-for-new-customers"></a>Yeni müşteriler için geçiş

Fiyatlandırma modeli konuşma hizmeti için bir saat dayalı bir fiyatlandırma modeli kullanarak basittir.  

1. Uygulamanızın kullanılabilir olduğu her bölgede bir Azure kaynağı oluşturun. Azure kaynak adı **konuşma**. Tek bir Azure kaynağı için ayrı kaynaklar oluşturmak yerine aynı bölgede aşağıdaki hizmetleri kullanabilirsiniz:

    * Konuşmayı Metne Dönüştürme
    * Özel Konuşmayı metne dönüştürme
    * Metin okuma
    * Konuşma çevirisi

2. İndirme [konuşma SDK](speech-sdk.md).

3. Hızlı Başlangıç kılavuzları ve SDK'sı örnekleri doğru API'lerini kullanmayı izleyin. REST API'lerini kullanmanız durumunda Ayrıca kaynak anahtarları ve doğru Uç noktalara kullanmanız gerekir.

4. İstemci uygulamayı konuşma Hizmetleri ve API'ler kullanacak şekilde güncelleştirin.

## <a name="migration-for-existing-customers"></a>Mevcut müşteriler için geçiş

Konuşma Hizmetleri portalında konuşma Hizmetleri için mevcut kaynak anahtarlarınızı geçirin. Aşağıdaki adımları kullanın:

> [!NOTE]
> Kaynak anahtarları yalnızca aynı bölge içinde geçirilebilir.

1. Oturum [cris.ai](https://cris.ai/Home/CustomSpeech) Portalı ' nı seçip sağ üst menüdeki abonelikte.

2. Seçin **seçili abonelik geçişi**.

3. Metin kutusuna abonelik anahtarını girin ve seçin **geçirme**.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetlerini ücretsiz denemek](get-started.md).
* Bilgi [Konuşmayı metne dönüştürme](./speech-to-text.md) kavramları.

## <a name="see-also"></a>Ayrıca bkz.

* [Konuşma hizmeti nedir](overview.md)
* [Konuşma Hizmetleri ve Speech SDK'sı belgeleri](speech-sdk.md#get-the-sdk)
