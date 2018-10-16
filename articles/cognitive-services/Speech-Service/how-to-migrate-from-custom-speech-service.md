---
title: Konuşma hizmeti için özel konuşma Hizmeti'nden geçiş
titlesuffix: Azure Cognitive Services
description: Bilgi konuşma hizmeti için özel konuşma hizmeti nasıl geçiş yapılır.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 10/01/2018
ms.author: panosper
ms.openlocfilehash: db09a85daff553dc911d039d37c826343e93d240
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338531"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Konuşma hizmeti için özel konuşma hizmeti geçirme

Uygulamalarınızı konuşma hizmeti için özel konuşma hizmeti geçirmek için bu makaleyi kullanın.

Özel konuşma hizmeti artık konuşma hizmeti bir parçasıdır. En son kalite ve özellik güncelleştirmeleri yararlanmak için konuşma hizmeti geçin.
 
## <a name="migration-for-new-customers"></a>Yeni müşteriler için geçiş

Fiyatlandırma modeli konuşma hizmeti için bir saat tabanlı fiyatlandırma modeline taşıma basittir.   

1. Uygulamanızın kullanılabilir olduğu her bölgede bir Azure kaynağı oluşturun. Azure kaynak adı **konuşma**. Tek bir Azure kaynağı için ayrı kaynaklar oluşturmak yerine aynı bölgede aşağıdaki hizmetleri kullanabilirsiniz:

    * Konuşma metin
    * Özel Konuşmayı metne dönüştürme
    * Metin okuma
    * Konuşma çevirisi

2. İndirme [konuşma SDK](speech-sdk.md). 

3. Hızlı Başlangıç kılavuzları ve SDK'sı örnekleri doğru API'lerini kullanmayı izleyin. REST API'lerini kullanmanız durumunda Ayrıca kaynak anahtarları ve doğru Uç noktalara kullanmanız gerekir. 

4. Konuşma hizmeti ve API'leri kullanmak için İstemci uygulamayı güncelleştirin. 

> [!NOTE]
> * Konuşma, dil anlama (LUIS), etkinleştirilirse LUIS - konuşma hizmetlerinin yanı sıra LUIS için tek bir LUIS kaynağı aynı bölgede çalışır. Bkz: [amaçlardan tutun konuşma tanıma](how-to-recognize-intents-from-speech-csharp.md) belgeleri.
> * Metni metin çevirisi, konuşma hizmeti bir parçası değil. Bu, kendi Azure kaynak aboneliği gerekir.
  


## <a name="migration-for-existing-customers"></a>Mevcut müşteriler için geçiş

Mevcut müşteriler, konuşma tanıma hizmeti portalı konuşma hizmeti, var olan kaynak anahtarları geçirmek için gereklidir. Aşağıdaki adımları kullanın: 

> [!NOTE] 
> Kaynak anahtarları yalnızca aynı bölge içinde geçirilebilir. 

1. Oturum [cris.ai](http://www.cris.ai) portalı ve sağ üst menüdeki aboneliği seçin. 

2. Seçin **seçili abonelik geçişi**.

3. Metin kutusuna abonelik anahtarını girin ve seçin **geçirme**.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetini ücretsiz deneyin](get-started.md)
* Bilgi [Konuşmayı metne dönüştürme](./speech-to-text.md) kavramları

## <a name="see-also"></a>Ayrıca bkz.

* [Konuşma hizmeti nedir](overview.md)
* [Konuşma hizmeti ve SDK Belgeleri](speech-sdk.md#get-the-sdk)