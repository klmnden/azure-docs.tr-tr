---
title: Abonelik anahtarları - özel konuşma hizmeti alın
titlesuffix: Azure Cognitive Services
description: Özel konuşma hizmeti çağrıları için Abonelik anahtarları alma konusunda bilgi edinin.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: e4694928baf98bdb0d6aacead8dffec6bb73d6f7
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224786"
---
# <a name="obtain-subscription-keys"></a>Abonelik anahtarları alma
Azure özel konuşma hizmeti kullanmaya başlamak için ilk kullanıcı hesabınız bir Azure aboneliğine bağlamanız gerekir. Ücretsiz ve Ücretli katmanlar için abonelik yok. Katmanları hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-key"></a>Bir abonelik anahtarı edinirler
1. Bir abonelik anahtarı iki yoldan biriyle Azure portalından alabilirsiniz:

    * Git [Azure portalında](https://ms.portal.azure.com), ve yeni bir Bilişsel hizmetler API'si için arama yaparak ekleyin _Bilişsel Hizmetler_ seçip **Bilişsel hizmetler API'leri**.

      ![Bilişsel hizmetler arama](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription.png)

    * Veya doğrudan [Bilişsel hizmetler API'leri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServices).

        ![Bilişsel Hizmetler API’leri](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription2.png)

    
1. Aşağıdaki gerekli alanları doldurun:

      a. **Hesap adı**. Sizin için uygun bir ad kullanın. Bu ad kaynaklar listesine Bilişsel Hizmetleri aboneliğinize bulabilmesi unutmayın.

      b. **Abonelik**. Bir Azure aboneliklerinizi seçin.

      c. **API türü**. Seçin **özel konuşma hizmeti (Önizleme)**.

      d. **Konum**. Şu anda **Batı ABD**.

      e. **Fiyatlandırma katmanı**. Sizin için uygun katmanını seçin. **F0** ücretsiz katmanı gösterir. İzin verilen kotaları üzerinde açıklanmıştır [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

      ![Bilişsel Hizmetler hesabı oluşturma](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-blade.png)

1. Panonuzda bir görünüm veya sağlanan hesap adına sahip bir hizmet kaynakları listenizde bulmanız gerekir. Bu seçeneği belirlediğinizde, hizmetinizin genel bakış görebilirsiniz. Soldaki listedeki altında **kaynak yönetimi**seçin **anahtarları**. Kopyalama **anahtar 1**.

      Sonraki adımlarda Bu abonelik anahtarı gereklidir.

      ![ANAHTAR 1](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys2.png)

      > [!NOTE]
      > Kopyalamayın **abonelik kimliği** genel bakış sayfasında. Sonraki adımda abonelik anahtarı gerekir.
      >

      ![Genel abonelik kimliği](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys.png)

1. Sağ üst Şeritteki abonelik anahtarınızı girmek için kullanıcı hesabınızı seçin. Aşağı açılan menüden **abonelikleri**.

      ![Abonelik menü öğesi](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-selection.png)

    İlk kez açılmasından boş olduğu aboneliklerinin bir tablo görüntülenir.

    ![Abonelikleri tablo](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-list.png)

1. Seçin **yeni Ekle**. Abonelik ve abonelik anahtarı için bir ad girin. Ya da olabilir **anahtar 1** (birincil anahtar) veya **anahtar 2** (ikincil anahtarı) aboneliğinizde.

      ![Abonelik anahtarı adı](../../../media/cognitive-services/custom-speech-service/custom-speech-enter-subsciption.png)

Verileri yüklemek için bir model eğitip veya bir dağıtım yapın, özel konuşma hizmeti etkinliklerinizi bir Azure aboneliğine bağlamanız gerekir. Ücretsiz katman veya bir Ücretli katman abonelik olabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-id"></a>Bir abonelik Kimliğini alın
Abonelik Kimliği almak için Azure portalına gidin. Arama *Bilişsel Hizmetler* ve *özel konuşma hizmeti*ve yönergeleri izleyin.

> [!NOTE]
> Bu işlemde daha sonra abonelik anahtarı gereklidir.
>

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturmaya başlamak, [özel akustik model](cognitive-services-custom-speech-create-acoustic-model.md).
* Oluşturmaya başlamak, [özel dil modeli](cognitive-services-custom-speech-create-language-model.md).
