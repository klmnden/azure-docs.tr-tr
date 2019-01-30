---
title: Özel konuşma tanıma hizmetinden konuşma Services'a geçme
titlesuffix: Azure Cognitive Services
description: Özel konuşma hizmeti artık konuşma hizmeti bir parçasıdır. En son kalite ve özellik güncelleştirmeleri yararlanmak için konuşma hizmeti geçin.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 594233b9e345f9578c218b042a64ea167d50addb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211041"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Konuşma hizmeti için özel konuşma hizmeti geçirme

Uygulamalarınızı konuşma hizmeti için özel konuşma tanıma hizmetinden geçirmek için bu makaleyi kullanın.

Özel konuşma hizmeti artık konuşma hizmeti bir parçasıdır. En son kalite ve özellik güncelleştirmeleri yararlanmak için konuşma hizmeti geçin.

## <a name="migration-for-new-customers"></a>Yeni müşteriler için geçiş

Fiyatlandırma modeli konuşma hizmeti için bir saat dayalı bir fiyatlandırma modeli kullanarak basittir.  

1. Uygulamanızın kullanılabilir olduğu her bölgede bir Azure kaynağı oluşturun. Azure kaynak adı **konuşma**. Tek bir Azure kaynağı için ayrı kaynaklar oluşturmak yerine aynı bölgede aşağıdaki hizmetleri kullanabilirsiniz:

    * Konuşmayı Metne Dönüştürme
    * Özel Konuşmayı metne dönüştürme
    * Metin okuma
    * Konuşma çevirisi

2. İndirme [konuşma SDK](speech-sdk.md).

3. Hızlı Başlangıç kılavuzları ve SDK'sı örnekleri doğru API'lerini kullanmayı izleyin. REST API'lerini kullanmanız durumunda Ayrıca kaynak anahtarları ve doğru Uç noktalara kullanmanız gerekir.

4. Konuşma hizmeti ve API'leri kullanmak için İstemci uygulamayı güncelleştirin.

> [!NOTE]
> * Konuşma Language Understanding (LUIS) etkinleştirilirse, tek bir LUIS kaynağı aynı bölgede tüm konuşma hizmetlerinin yanı sıra LUIS çalışır. Daha fazla bilgi için [amaçlardan tutun konuşma tanıma](how-to-recognize-intents-from-speech-csharp.md).
> * Metni metin çevirisi, konuşma hizmeti bir parçası değil. Bu işlev, kendi Azure kaynak aboneliği gerektirir.
 


## <a name="migration-for-existing-customers"></a>Mevcut müşteriler için geçiş

Konuşma hizmeti portalı konuşma hizmeti için mevcut kaynak anahtarlarınızı geçirin. Aşağıdaki adımları kullanın:

> [!NOTE]
> Kaynak anahtarları yalnızca aynı bölge içinde geçirilebilir.

1. Oturum [cris.ai](http://www.cris.ai) Portalı ' nı seçip sağ üst menüdeki abonelikte.

2. Seçin **seçili abonelik geçişi**.

3. Metin kutusuna abonelik anahtarını girin ve seçin **geçirme**.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetini ücretsiz deneyin](get-started.md).
* Bilgi [Konuşmayı metne dönüştürme](./speech-to-text.md) kavramları.

## <a name="see-also"></a>Ayrıca bkz.

* [Konuşma hizmeti nedir](overview.md)
* [Konuşma hizmeti ve SDK Belgeleri](speech-sdk.md#get-the-sdk)
